### 准备工作

+ 添加定向光 和 gui
+ 开启 自身真实的光属性 效果, 如果你用的blander的模型开启这个就能得到你在blander中的灯光效果一样
+ 加载模型
+ 添加环境贴图  p 正数  n 负数,  px, nx, py, ny, pz, nz
+ 将环境贴图 应用到模型上  筛选只应用到 Mesh 和 MeshStandardMaterial 材质上, 添加调试
+ 更简单的方法
+ 优化
  + 修改输出编码 线性编码  srgb 编码 gamma编码  的区别
  + 记得也将环境贴图的输出编码  也修改成 srgb编码
  + 怎么选择输出编码?
+  色调映射 toneMapping 如何调试 下拉框
+ 设置曝光度 toneMappingExposure
+ 如何调整边缘的像素 更加细化
  + 开启 multi sampling 多重采样,   安提antialias: true
+ 开启 shadow, 优化 灯光远近, 阴影分辨率
+ 调试 blander, 阴影bug ,  设置阴影偏移量 



#### 开启物理光照模式

```js
renderer.physicallyCorrectLights = true
```

开启物理光照的的模式, 模拟物理光照照射到模型上的效果

#### 加载模型

```js
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'

const glTFLoader = new GLTFLoader()
glTFLoader.load('models/FlightHelmet/glTF/FlightHelmet.gltf', gitf => {
  gltf.scene.scale.set(10,10,10)
  scene.add(gltf.scene)
})
```

#### 添加场景环境贴图

`p`正方向, `n`负方向, 正确的顺序是, `px, nx,py,ny,pz,nz`

```js
// 使用 cubeTextureloader 加载
const cubeTextureLoader = new THREE.CubeTextureLoader()
const environmentMapTexyure = cubeTextureLoader.load([
  'textures/environmentMaps/0/px.jpg',
  'textures/environmentMaps/0/nx.jpg',
  'textures/environmentMaps/0/py.jpg',
  'textures/environmentMaps/0/ny.jpg',
  'textures/environmentMaps/0/pz.jpg',
  'textures/environmentMaps/0/nz.jpg'
])

scene.background = environmentMapTexyture
```



#### 给所有的材质添加环境贴图

给材质 添加环境贴图的两种方式

可以使用`material.envMap` 属性来单独给每一个材质添加他们的环境贴图

```js
const updateAllMaterial = () => {
  scene.traverse(child => {
    if (child instanceof THREE.Mesh && child.material instanceof THREE.MeshStandardMaterial) {
      child.material.envMap = environmentMap
      child.material.envMapIntensity = 5
    }
  })
}
```

也可以通过设置`scene.environment`统一设置所有支持环境贴图的材质

```js
scene.environment = environmentMap
```

#### 设置解码格式

可选的有三种格式 `THREE.LinearEncoding THREE.sRGBEncoding` 这三种格式 可以给 `renderer` 和 `环境贴图`设置, 一般如果存在 normal 等纹理的时候 不要使用 sRGB

```js
environmentMap.encoding = THREE.sRGBEncoding
renderer.outputEncoding = THREE.sRGBEncoding
```

就视觉效果来说 `sRGB` 色彩感觉更加的细腻和好看



#### 设置渲染器的色调

`renderer.toneMapping` 的几种色调, 可在 render 常量中找到

+ `THREE.NoToneMapping`
+ `THREE.LinearToneMapping`
+ `THREE.ReinhardToneMapping`
+ `THREE.CineonToneMapping`
+ `THREE.ACESFilmicToneMapping`

```js
renderer.toneMapping = THREE.ACESFilmicToneMapping
```

一般设置 `THREE.ACESFilmicToneMapping` 比较好看

调试这个属性的时候 存在一个小bug 调试效果

```js
gui.add(renderer, 'toneMapping', {
  No: THREE.NoToneMapping,
  Linear: THREE.LinearToneMapping,
  Reinhard: THREE.ReinhardToneMapping,
  Cineon: THREE.CineonToneMapping,
  ACESFilmic: THREE.ACESFilmicToneMapping
}).onFinishChange(() => {
  renderer.toneMapping = Number(renderer.toneMapping)
})
```

##### needsUpdate

**如果直接**添加上面的属性 , 然后当我们`改变色调`的时候你会发现 色调效果 只作用到了 环境贴图上, 模型上则并没有应用上效果, 我们需要设置`mesh.material.needUpdate = true`

`常见问题 : 当我们改变了某些全局变量, 但是 模型上并无效果, 可能需要设置模型的 材质 自动更新`

```js
// gui onFinishChange
gui.add(renderer, 'toneMapping', {
  No: THREE.NoToneMapping,
  Linear: THREE.LinearToneMapping,
  Reinhard: THREE.ReinhardToneMapping,
  Cineon: THREE.CineonToneMapping,
  ACESFilmic: THREE.ACESFilmicToneMapping
}).onFinishChange(() => {
  renderer.toneMapping = Number(renderer.toneMapping)
  updateAllMaterial()
})
// updateAllMaterials
const updateAllMaterial = () => {
  scene.traverse(child => {
    if (child instanceof THREE.Mesh && child.material instanceof THREE.MeshStandardMaterial) {
      // child.material.envMap = environmentMap
      child.material.envMapIntensity = debugObj.envMapIntensity
      child.material.needsUpdate = true
    }
  })
}
```

##### 设置色调曝光度

```js
renderer.toneMappingExposure = 2

gui.add(renderer, 'toneMappingExposure').min(0).max(10).step(0.1)
```



#### 抗锯齿

```js
const renderer = new THREE.WebGLRenderer({
  canvas,
  antialias: true
})
```



#### 添加阴影效果

**步骤一 :** 开启阴影, 设置阴影类型

```js
// 开启阴影
renderer.shadowMap.enabled = true
renderer.shadowMap.type = THREE.PCFSoftShadowMap
```

**步骤二 : ** 光源激活阴影, 并优化光源

```js
directionLight.castShadow = true
directionLight.shadow.camera.far = 15
directionLight.shadow.mapSize.set(1024,1024)  // 设置阴影分辨路

const directionHelper = new THREE.CameraHelper(directionLight.shadow.camera)
scene.add(directionHelper)
```

**步骤三 :**  给激活模型产生阴影 和 接收阴影

```js
child.castShadow = true
child.receiveShadow = true
```

##### 如果阴影参数BUG

可以调试

```js
directionLight.shadow.normalBias = 0.05
```

