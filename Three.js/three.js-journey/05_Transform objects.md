本节需要掌握四个转换属性

+ position
+ scale
+ rotation
+ quaternion

修改 position 属性

### Vector3

Vector3 向量单位, 有许多方法和属性

v3.length()  获取位置到中心点的距离

v3.distanceTo(camera.position) 获取位置到另一个位置的距离

v3.normalize() 将位置全部设置为 1 



### Axes Helper

添加一个辅助轴

```js
const axesHelper = new THREE.AxesHelper(3)
scene.add(axesHelper)
```



### SCALE OBJECTS



### ROTATE OBJECTS

关于旋转 我们由有两种方法方式实现我们想要的效果

#### ROTATION

需要注意默认情况下 x y z 轴是同时生效的 可设置 reorder 方法设置 旋转顺序

```js
mesh.rotation.reorder('YXZ')
mesh.rotation.x = Math.PI * 0.5
mesh.rotation.y = Math.PI * 0.5
```



### LOOKAT



### SCENE GROUP



### 小结

+ 主要学习如何 操作物体的 平移 缩放 旋转
+ 介绍了两个单位 Vector3 和 Elur
+ 辅助轴的添加
+ Group 概念

### 完成代码

```js
import "./style.css";
import * as THREE from "three";

// Canvas 
const canvas = document.querySelector('canvas.webgl')

// Scene
const scene = new THREE.Scene()

// group
const group = new THREE.Group()
group.position.x = 0
scene.add(group)

const cube1 = new THREE.Mesh(
  new THREE.BoxGeometry(1,1,1),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
)
group.add(cube1)
const cube2 = new THREE.Mesh(
  new THREE.BoxGeometry(1,1,1),
  new THREE.MeshBasicMaterial({ color: 0x00ff00 })
)
cube2.position.x = 2
group.add(cube2)
// Sizes
const sizes = {
  width: 800,
  height: 800
}

// AxesHelper
const axesHelper = new THREE.AxesHelper(3)
scene.add(axesHelper)

// Camera
const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height)
camera.position.z = 5
camera.position.y = 1
camera.lookAt(group.position)

scene.add(camera)
// camera.lookAt(mesh.position)

// Render
const renderer = new THREE.WebGLRenderer({
  canvas
})
renderer.setSize(sizes.width, sizes.height)
renderer.render(scene, camera)
```

