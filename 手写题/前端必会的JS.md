[参考](https://juejin.cn/post/6946022649768181774#heading-14)

## 继承



## 函数防抖

高频触发事件, N 秒后会执行一次, 如果 N 秒内再次触发, 则会重新计时

应用举例 :  滚动事件 滚动结束后 触发事件,  疯狂点击按钮, 停下来才触发事件



函数内部支持使用 this 和 event 对象

```js
function debounce(func, wait) {
    var timeout;
    return function() {
        var context = this;
        var args = arguments;
        clearTimeout(timeout)
        timeout = setTimeout(function() {
            func.apply(context, argus)
        }, wait)
    }
}
```

使用 :

```js
// 效果 鼠标持续移动 停下来 dom 内数字 加一
const dom = document.getElementById('layout')
function getUserAction(e) {
    node.innerHTML = count++
}
dom.onmousemove = debounce(getUserAction, 1000)
```



## 函数节流

触发高频事件, 没过 N 秒执行一次

```js
function throttle(func, wait) {
    var context , args
    var previous = 0
    return function() {
        var now = +new Date()
        context = this
        if (now-previous > wait) {
            func.apply(context, args)
            previous = now
        }
    }
}
```

