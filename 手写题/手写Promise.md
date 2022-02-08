参考一系列文章

+ [100行代码实现Promise/A+](https://mp.weixin.qq.com/s/qdJ0Xd8zTgtetFdlJL3P1g)
+ [手写Promise](https://juejin.cn/post/6844903625769091079)

## Promise的实现

首先关注Promise 本身, Promise 内部的状态只能从 pending 转换成 fulfilled 或 rejected

```js
class Promise {
  constructor(fn) {
    this.state = 'pending'
    this.value = undefined
    this.reason = undefined
    
    let resolve = value => {
      if(this.state === 'pending') {
        this.value = value
        this.state = 'fulfilled'
      }
    }
    let reject = reason => {
      if(this.state === 'pending') {
        this.reason = undefined
        this.state = 'rejected'
      }
    }
    
    try {
      fn(resolve, reject)
    } catch(err) {
      reject(err)
    }
  }
}
```

然后定义 `then` 方法,  里面有两个参数 : onFulfilled, onRejected 成功有成功的值, 失败有失败的值

```diff
class Promise {
  constructor(fn) {
    this.state = 'pending'
    this.value = undefined
    this.reason = undefined
    
    let resolve = value => {
      if(this.state === 'pending') {
        this.value = value
        this.state = 'fulfilled'
      }
    }
    let reject = reason => {
      if(this.state === 'pending') {
        this.reason = undefined
        this.state = 'rejected'
      }
    }
    
    try {
      fn(resolve, reject)
    } catch(err) {
      reject(err)
    }
  }
+	then(onFulfilled, onRejected) {
+    if(this.state === 'fulfilled') {
+      onFulfilled(this.value)
+    }
+    if(this.state === 'rejected') {
+      onRejected(this.reason)
+    }
+  }
}
```

解决`异步实现`,  通常我们Promise内的是异步的代码, 比如将 resolve 放在 setTimeout 中实现, 这时的 then 中的 state 还处于 pending 中, 所以我们应该先存起来

```diff
class Promise {
  constructor(fn) {
    this.state = 'pending'
    this.value = undefined
    this.reason = undefined
    
    this.onResolveCallback = []
    this.onRejectCallback = []
    
    let resolve = value => {
      if(this.state === 'pending') {
        this.value = value
        this.state = 'fulfilled'
+        this.onResolveCallback.forEach(fn => fn())
      }
    }
    let reject = reason => {
      if(this.state === 'pending') {
        this.reason = undefined
        this.state = 'rejected'
+        this.onRejectCallback.forEach(fn => fn())
      }
    }
    
    try {
      fn(resolve, reject)
    } catch(err) {
      reject(err)
    }
  }
	then(onFulfilled, onRejected) {
    if(this.state === 'fulfilled') {
      onFulfilled(this.value)
    }
    if(this.state === 'rejected') {
      onRejected(this.reason)
    }
+   if(this.state === 'pending') {
+     this.onResolveCallback.push(() => {
+      onFulfilled(this.value)
+     })
+     this.onRejectCallback.push(() => {
+       onRejected(this.reason)
+     })
+   }
  }
}
```

接下来解决 then 方法的链式调用, 需要注意的是

1. 为了达成链式, 我们默认在 then 里返回一个promise, 暂时称之为 promise2
2. promise2 的返回值, 就是 onFulfilled() 或者 onRejected()的值, 然后通过`resolvePromise`函数转换后的值

`resolvePromise`的参数有 `promise2(then返回的Promise), x(回调函数返回的值), resolve, reject`, 这里的 resolve 和 reject 其实是 promise2 的

```diff
class Promise {
  constructor(fn) {
    this.state = 'pending'
    this.value = undefined
    this.reason = undefined
    
    this.onResolveCallback = []
    this.onRejectCallback = []
    
    let resolve = value => {
      if(this.state === 'pending') {
        this.value = value
        this.state = 'fulfilled'
        this.onResolveCallback.forEach(fn => fn())
      }
    }
    let reject = reason => {
      if(this.state === 'pending') {
        this.reason = undefined
        this.state = 'rejected'
        this.onRejectCallback.forEach(fn => fn())
      }
    }
    
    try {
      fn(resolve, reject)
    } catch(err) {
      reject(err)
    }
  }
	then(onFulfilled, onRejected) {
+    let promise2 = new Promise((resolve, reject) => {
      if(this.state === 'fulfilled') {
+       let x = onFulfilled(this.value)
+       resolvePromise(promise2,x, resolve, reject)
      }
      if(this.state === 'rejected') {
+       let x = onRejected(this.reason)
+       resolvePromise(promise2,x, resolve, reject)
      }
      if(this.state === 'pending') {
        this.onResolveCallback.push(() => {
+         let x = onFulfilled(this.value)
+         resolvePromise(promise2, x, resolve, reject)
        })
        this.onRejectCallback.push(() => {
+         letx = onRejected(this.reason)
+         resolvePromise(promise2, x, resolve, reject)
        })
      }
    })
+    return promsie2
  }
}
```

上面这步就是完成了链式, 接下来就是`resolvePromise`的规则了

+ 首先 x 不能是 promise2 , 不然就会造成循环
+ x 为普通值  直接返回
+ x 为 对象 或者 函数时
+ 获取 x.then , 没有则 reject
+ 拿到 x.then , 如果 x.then 是一个函数 , 则通过 call 方法调用, 第一个参数是this, 后面的是 成功的回调和失败的回调
+ 成功失败的回调只能调用一次 , 所以设定一个 called 来防止多次调用

这个地方主要处理的就是 promise2 的返回值问题, promise2的返回值是then函数的 成功或者失败回调的返回值,  当返回 特殊处理了返回值 为一个 promise 或者 是一个 带 then函数的对象

`resolvePromise`函数的调用是浏览器内核 的异步调用 可用`setTimeout`模拟

```js
function resolvePromise(promsie2,x,resolve,reject) {
  if(x === promise2) {
  	return reject(new TypeError('Are you serious'))
  }
  if(x != null && (typeof x === 'object' || typeof x === 'function') {
   	try {
     const then = x.then
     let called
		 if(typeof then === 'function') {
     	then.call(x, y => {
        if(called) return
        called = true
        // 递归 最终得到 promise2的返回值
				resolvePomise(promise2, y, resolve, reject)
      }, e => {
        if(called) return
				called = true
        reject(err)
      })
  	 } else {
       resolve(x)
     }
    } catch(err) {
    	reject(err)
  	}
  } else {
    resolve(x)
  }
}
```

到这 就结束了



## Promise的其他方法

### Promise.resolve

```js
Promise.resolve = function(value) {
  if(value instanceof Promise) {
    return value
  }
  return new Promise(resolve => resolve(value))
}
```

### Promise.reject

```js
Promise.reject = function(reason) {
  return new Promise((resolve, reject) => reject(reason))
}
```

### Promise.all

Promise.all 的规则是

+ 传入的所有的 Promise 状态都是 fulfilled , 则返回一个由他们值组成的 , 状态为 fulfilled 的新Promise
+ 只要有一个 Promise 是 rejected, 则将 第一个 rejected 的值 , 返回一个 状态为 rejected 的 Promise
+ 有一个为 pending 则返回 一个 pending 的 Promise

```js
Promise.all = function(promisArr) {
	let index = 0, result = []
  return new Promise((resolve, reject) => {
    promiseArr.forEach((p, i) => {
      Promise.resolve(p).then(val => {
        index++
        result[i] = val
        if (index === promiseArr.length){
          resolve(result)
        }
      }, err => {
        reject(err)
      })
    })
  })
}
```

### Promise.race

```js
Promise.race = function(promiseArr) {
  return new Promise((resolve, reject) => {
    promiseArr.forEach(p => {
      Promise.resolve(p).then(val => {
        resolve(val)
      }, err => {
        reject(err)
      })
    })
  })
}
```



## 一个小测试

来来来 , 一起看一个测试题

```js
Promise.resolve().then(() => {
  console.log(0)
  return Promise.resolve(4)
}).then(res => {
  console.log(res)
})

Promise.resolve().then(() => {
  console.log(1)
}).then(() => {
  console.log(2)
}).then(() => {
  console.log(3)
}).then(() => {
  console.log(5)
}).then(() => {
  console.log(6)
})
```

控制台打印的结果为 0,1,2,3,4,5,6  为什么? 知道吗

首先第一步返回的 是一个 `Promise.resolve(4)` , 于是就先执行 一次 `Promise,resolve()`

然后`Promise.resolve()`返回的是又是一个`Promise`,  于是在`resolvePromise`函数中调用了一次 然后才返回了一个普通值

这下子懂了把 （￣。。￣）