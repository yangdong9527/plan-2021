### PerspectiveCamera

创建一个透视相机

```js
const camera = new THREE.PerspectiveCamera(75, sizes.width/size.height, 0.1, 100)
```

相机的第三个和四个属性 可以理解为相机都能看到的最近距离 和 最远的距离, 不在这个范围内的物体将看不见

可以理解为光线从相机的哪一个点的位置 照向 物体, 视野像一个圆锥体

### OrthographicCamera

这个可以理解为相机是一个平行光的方式渲染, 

```js
const aspectRatio = sizes.width / size.height
const camera = new THREE.OrthographicCamera(-1 * aspectRatio, 1 * aspectRatio, 1, -1, 0.1, 100)
```

### 移动相机

获取鼠标在屏幕上的 位置

```js
let cursor = {
  x: 0,
  y: 0
}
window.addEventListener('mousemove', event => {
  cursor.x = event.clientX / sizes.width - 0.5
  cursor.y = event.clientY / sizes.height - 0.5
})
// 得到的值的区间分为 为  [-0.5, 0.5]
```

设置能够围绕着中心点做一个圆周运动

```js
const tick = () => {
  camera.position.x = Math.sin(cursor.x * 2 * Math.PI) * 3
  camera.position.z = Math.con(cursor.x * 2 * Math.PI) * 3
  camera.position.y = cursor.y * 5
  renderer.render(scene, camera)
  requestAnimationFram(tick)
}
tick()
```

sin拿到角度的对边 cos拿到邻边

### Controls

three.js 提供了很多控制器 

#### OrbitControls(轨道控制器)

```js
import { OrbitControls } from 'tree/examples/jsm/controls/OrbitControls'

const controls = new OrbitControls(camear, canvas)
controls.enableDamping = true

controls.update()
```

还有很多其他属性



### 小结

+ 首选介绍了两种常用相机的区别, PerspectiveCamera, OrthographicCamera
+ 如何自己实现一个 轨道控制器 来移动摄像机 (基础的Sin, Cos 知识)
+ 介绍好几种控制器, 主要介绍了 OrbitControls 轨道控制器

