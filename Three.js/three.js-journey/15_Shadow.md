如果在软件中制作的光线追踪 前端每秒钟渲染 那么多次 不合适, 所以还是自己做比较好



### 实现原理

首先 阴影就是一种 纹理, three.js 会根据 光和物体 生成一个 阴影的纹理, 然后在每次渲染的时候, 拿到这个纹理, 将它特殊处理



### 基础

```js
// 是否生成阴影
render.shadowMap.enabled = true

// 集合体是否产生 阴影， 是否 接受阴影
Sheree.castShadow = true

plane.receiveShadow = true

// 激活 光 的阴影 只有 点光源 定向光 聚光灯 支持
directionLight.castShadow = true
```

### 优化

```js
// 大小
directionLight.shadow.mapSize.width = 1024

// 远近 控制 光能找到的范围  
// 添加复制工具
const directionalLightCameraHelper = new THREE.CameraHelper(directionalLight.show.camera)
scene.add(directionalLightCameraHelper)

directionalLight.shadow.camera.far = 6
directionalLight.shadow.camera.near = 1

//控制光源产生阴影的 覆盖范围  越小  这个阴影看起来就 约清楚 不会出现马赛克
directionalLight.shasow.camera.top = 1
directionalLight.shadow.camera.bottom = 1
directionalLight.shadow.camera.left = 1
directionalLight.shadow.camera.right = 1

// 调整完可以隐藏helper
directionalLightCamera.visible = false

// 产生模糊
directionalLight.show.radius = 10
```

产生阴影的 类型 有四种方式

```js
renderer.shadowMap.type = THREE.PCFSoftShadowMap

```



### 添加一个聚光灯

```js
const spotLight = new THREE.SpotLight(0xffffff, 0.4, 10 Math.PI * 0.3)
spotLight.castShadow = true

spotLight.position.set(0,2,2)
scene.add(spotLight)
scene.add(spotLight.target)

const spotLightCamerHelper = new THREE.CameraHepler(spotLight.shadow.camera)
scene.add(spotLightCamerHelper)

spotLight.shadow.camera.fov = 30  // 视角 圆锥光源的大小
```



### 添加一个点光源

 



### 使用阴影贴图

#### 基础贴图

```JS
const textureLoader = new THREE.TextureLoadr()
const bakedShadow = textureLoader.load('./js')

// 投影在 平面上
const plane = new THREE.Mesh(
	new THREE.PlaneBufferGeometry(5,5),
    new THREE.MeshBasicMaterial({
        map: bakedShadow
    })
)
```



#### 动态的阴影 让球上下移动

使用另一种贴图  然后通过 调整透明度来实现

```js
const simpleShadow = textureLoader.load('./simpleShow.jpg')

// 创建一个 阴影几何体 屏幕
const sphereShadow = new THREE.Mesh(
	new THREE.PlaneBufferGeometry(1.5,1.5),
    new THRE.MeshBasicMaterial({
        color: 0x000000,
        transparent: true,
        alphaMap: simpleShadow
    })
)
sphereShadow.rotation.x = Math.PI * 0.5
sphereShadow.position.y = Plane.position.y + 0.01
scene.add(sphereShadow)

// 让球动起来
const tick = () => {
    const elapsedTime = clock.getElapsedTime()
    
    sphere.position.x = Math.cos(elapsedTime) * 1.5
    sphere.position.z = Math.sin(elapsedTime) * 1.5
    sphere.position.y = Math.abs(Math.sin(elapsedTime * 3))
    
    // update shadow
    shpereShadow.position.x = sphere.position.x
    shpereShadow.psoition.y = sphere.position.y
    sphereShasow.material.opacity = (1 - Math.abs(sphere.position.y)) * 0.3
}
```

