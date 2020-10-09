# requestAnimationFrame
### requestAnimationFrame是什么？
- requestAnimationFrame：一个专门请求动画的API。
### requestAnimationFrame的特点
1. 由系统决定回调函数的执行时机，60Hz的刷新频率，那么每次刷新的间隔中会执行一次回调函数，不会引起丢帧，不会卡顿。
2. 具有函数节流的特点，在高频率事件(resize,scroll等)中，为了防止在一个刷新间隔内发生多次函数执行，使用requestAnimationFrame可保证每个刷新间隔内，函数只被执行一次。
### requestAnimationFrame的使用
```
🌰：
        function step(){
            if (left > 1000) return;
            left += 50;
            cube1.style.left = left + 'px';
            window.requestAnimationFrame(step);
        }
        window.requestAnimationFrame(step);
```
### 参考
- [window.requestAnimationFrame](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)
- [requestAnimationFrame](https://www.jianshu.com/p/f6d933670617)

