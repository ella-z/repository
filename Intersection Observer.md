# Intersection Observer
- 提供了一种异步观察目标元素与其祖先元素或顶级文档视窗(viewport)交叉状态的方法。祖先元素与视窗(viewport)被称为根。
- 实现的步骤：
   1. new IntersectionObserver()实例化一个全局observer，
   2. 把DOM加入到observer的观察列表。
   3. 当该DOM与特定元素（视窗）交集状态发生变化，回调函数被执行。
   4. 取消对该DOM的观察。
- 小栗子:
```
🌰：
    const intersectionObserver = new IntersectionObserver(
      (entries) => {
        entries.forEach(item => {
          // do something
          console.log(item);//输出IntersectionObserverEntry对象，对象中包含了监控对象的状态，是否有离开root等等
        })
      },
      {
        //配置
        root: null, // 必须是目标元素的父级元素。如果未指定或者为null，则默认为浏览器视窗。
        rootMargin: "0px", // root元素的margin，所有的偏移量均可用像素(pixel)(px)或百分比(percentage)(%)来表达, 默认值为"0px 0px 0px 0px"。
        threshold: 0.1, // 回调执行门槛，交集百分比
      }
    );
    const box = document.getElementsByClassName("box");
    intersectionObserver.observe(box[0]);
```
- 作用的场景：
   - 图片懒加载
   - 下拉加载
   - 埋点
   ......
- 参考：
   - [Intersection Observer](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)
