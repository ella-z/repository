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





