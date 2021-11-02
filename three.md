## 03 - Basic scene

创建一个基础的场景我们需要四个东西

+ scene
+ mesh 网格
+ canera
+ render

首先我们创建一个场景

```js
const scene = new THREE.Scene()
```

然后我们场景一个立方体, 创建一个几何体 和 材料, 然后生成一个网格 添加到场景中

```js
// red cube
const geometry = new THREE.BoxGeometry(1,1,1)
const material = new THREE.MeshBasicMaterial({ color: 'red' })
const mesh = new THREE.Mesh(geometry, material)
scene.add(mesh)
```

然后添加一个相机, 调整摄像机位置 x 右 y 上 z 向你的方向

```js
const sizes = {
    width: 800,
    height: 600
}
// 一般最大55 
const camera = new THREE.PrespectiveCamera(75, sizes.width / sizes.height)
camera.position.z = 3
scene.add(camera)
```

创建一个render, 带上你的场景和相机

```js
const canvas = document.querySelector('.webgl')
const renderer = new THREE.WebGLRenderer({
    canvas
})
renderer.setSize(sizes.width, sizes.height)
renderer.render(scene,camera)
```

