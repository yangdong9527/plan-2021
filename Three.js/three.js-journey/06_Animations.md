### requestAnimations

使用该函数来进行动画渲染

```js
function tick() {
  mesh.position.x += 0.001 * 16
  renderer.render(scene, camera)
  window.requestAnimationFram(tick)
}
tick()
```

### 使用时间戳来控制动画数据

requestAnimationFrame 为没 16 毫秒出发一次

### Clock

```js
const clock = new THREE.Clock()
clock.getElapsedTime() // 这个也能获得一个毫秒值 不过是从零开始的
```

### gsap



### 小结

+ 使用requestAnimationFrame持续渲染页面
+ 使用时间戳来控制动画数据
+ 使用内置的Clock控制动画
+ 修改相机的动画配合sin cos
+ 使用gsap绘制动画