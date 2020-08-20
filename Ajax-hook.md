# Ajax-hook
### Ajax-hook是什么
- Ajax-hook是一个用于拦截浏览器XMLHttpRequest的库，通过它你可以在底层对请求和响应进行一些预处理。
- Ajax-hook中有三个钩子函数，分别为onRequest、onResponse、onError。在请求发起前，会先进入onRequest钩子，调用handler.next(config) 请求继续，如果请求成功，则会进入onResponse钩子，如果请求发生错误，则会进入onError。
- Ajax-hook中还有个对象：Handler，在钩子函数我们可以通过handler来决定请求的后续流程，它有3个方法：
   1. next(arg)：继续进入后续流程；如果不调用，则请求链便会暂停，这种机制可以支持在钩子中执行一些异步任务。该方法在onResponse钩子中等价于resolve，在onError钩子中等价于reject
   2. resolve(response)：调用后，请求后续流程会被阻断，直接返回响应数据，上层xhr.onreadystatechange或xhr.onload会被调用。
   3. reject(err)：调用后，请求后续流程会被阻断，直接返回错误，上层的xhr.onerror、xhr.ontimeout、xhr.onabort之一会被调用，具体调用哪个取决于err.type的值，比如我们设置err.type为"timeout"，则xhr.ontimeout会被调用。
- 使用示例：
```
🌰：
proxy({
    onRequest: (config, handler) => {
        if (config.url === 'https://aa/') {
            handler.resolve({
                config: config,
                status: 200,
                headers: {'content-type': 'text/text'},
                response: 'hi world'
            })
        } else {
            handler.next(config);
        }
    },
    onError: (err, handler) => {
        if (err.config.url === 'https://bb/') {
            handler.resolve({
                config: err.config,
                status: 200,
                headers: {'content-type': 'text/text'},
                response: 'hi world'
            })
        } else {
            handler.next(err)
        }
    },
    onResponse: (response, handler) => {
        if (response.config.url === location.href) {
            handler.reject({
                config: response.config,
                type: 'error'
            })
        } else {
            handler.next(response)
        }
    }
})
```
### Ajax-hook实现原理
- 整体思路是实现一个XMLHttpRequest的代理对象，然后覆盖全局的XMLHttpRequest，这样一但上层调用 new XMLHttpRequest这样的代码时，其实创建的是Ajax-hook的代理对象实例。
<img src="https://github.com/ella-z/repository/blob/master/image/ajax-hook%E5%8E%9F%E7%90%86%E5%9B%BE.png" />
- 源码：
   - Ajax-hook 一开始先保存了真正的XMLHttpRequest对象到一个全局对象，然后在注释1处，Ajax-hook覆盖了全局的XMLHttpRequest对象，这就是代理对象的具体实现。在代理对象内部，首先创建真正的XMLHttpRequest实例,记为xhr,然后遍历xhr所有属性和方法，在2处hookfun为xhr的每一个方法生成一个代理方法，在3处，通过defineProperty为每一个属性生成一个代理属性。
   
   ```
      ob.hookAjax = function (funs) {
        //保存真正的XMLHttpRequest对象
        window._ahrealxhr = window._ahrealxhr || XMLHttpRequest
        //1.覆盖全局XMLHttpRequest，代理对象
        XMLHttpRequest = function () {
          //创建真正的XMLHttpRequest实例
          this.xhr = new window._ahrealxhr;
          for (var attr in this.xhr) {
            var type = "";
            try {
              type = typeof this.xhr[attr]
            } catch (e) {}
            if (type === "function") {
              //2.代理方法
              this[attr] = hookfun(attr);
            } else {
              //3.代理属性
              Object.defineProperty(this, attr, {
                get: getFactory(attr),
                set: setFactory(attr)
              })
            }
          }
        }
   ```
   
   - 代理方法
   
       ```
         function hookfun(fun) {
          return function () {
             var args = [].slice.call(arguments)
             //1.如果fun拦截函数存在，则先调用拦截函数
             if (funs[fun] && funs[fun].call(this, args, this.xhr)) {
               return;
             }
            //2.调用真正的xhr方法
            this.xhr[fun].apply(this.xhr, args);
          }
         }
      ```     
      
### Ajax-hook的API
- proxy(proxyObject)
   - 拦截全局XMLHttpRequest。
   - 参数：proxyObject是一个对象，包含三个可选的钩子onRequest、onResponse、onError，我们可以直接在这三个钩子中对请求进行预处理。
   - 返回值：浏览器原生的XMLHttpRequest

- unProxy()
   - 取消拦截；取消后XMLHttpRequest将不会再被代理，浏览器原生XMLHttpRequest会恢复到全局变量空间。

- hook(Hooks)
   - 拦截全局XMLHttpRequest，此方法调用后，浏览器原生的XMLHttpRequest将会被代理，代理对象会覆盖浏览器原生XMLHttpRequest，直到调用unHook()后才会取消代理。
   - hooks; 钩子对象，里面是XMLHttpRequest对象的回调、方法、属性的钩子函数，钩子函数会在执行XMLHttpRequest对象真正的回调、方法、属性访问器前执行。
   - 返回值: 浏览器原生的XMLHttpRequest.

- unHook()
   - 取消拦截；取消后XMLHttpRequest将不会再被代理，浏览器原生XMLHttpRequest会恢复到全局变量空间。
   
#### proxy与hook的区别
- proxy是hook的封装，使用起来更加方便。
- hook使用比较麻烦时，因为需要具体到XMLHttpRequest对象的某一方法、属性、回调。与它相比，proxy更加简洁明了。
```
   🌰：
   hook({
     //拦截回调
     onreadystatechange:function(xhr,event){
       console.log("onreadystatechange called: %O")
       //返回false表示不阻断，拦截函数执行完后会接着执行真正的xhr.onreadystatechange回调.
       //返回true则表示阻断，拦截函数执行完后将不会执行xhr.onreadystatechange. 
       return false
     },
     onload:function(xhr,event){
       console.log("onload called")
       return false
     },
     //拦截方法
     open:function(args,xhr){
       console.log("open called: method:%s,url:%s,async:%s",arg[0],arg[1],arg[2])
       //拦截方法的返回值含义同拦截回调的返回值
       return false
     }
   })
```
   
### 浏览器兼容性
- 需要在支持ES5的浏览器环境中运行(不支持IE8)。




