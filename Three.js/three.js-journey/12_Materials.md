### material 属性

```js
const material = new THREE.MeshBasicMaterial()

meterial.map = doorColorTexture
material.color = new THREE.Color('red')
material.wireframe = trhe

// 如果要使用到 透明度 都要设置 transparent
material.transparent = true
material.opacity = 0.5
material.alphaMap = doorAlphaTexture

//设置平面那面渲染
material.size = THREE.FrontSide
// THREE.FrontSide  default
// THREE.BackSide
// THREE.DoubleSide

```

### MeshNormalMaterial 网格法向量材质



### MeshMatcapMaterial

自动生成图片的材质



### MeshDepthMaterial 网格深度材质



### MeshLamberMaterial



### MeshPhongMaterial



### MeshToonMaterial



### MeshStandarMaterial



### aoMap

设置材质突起的产生的阴影的感觉



### displacementMap

可以控制几何体纹理的 突起 凹陷





### 环境贴图

给物体添加周围环境贴图



如何获取环境 贴图



### 准备工作

#### 创建3个集合体

```js
const material = new THREE.MeshBasicMaterial()
const sphere = new THREE.Mesh(
	new THREE.SphereBufferGeometry(0.5, 16, 16),
  material
)
sphere.position.x = -1.5

const plane = new THREE.Mesh(
	new THREE.PlaneBufferGeometry(1,1)
  material
)

const torus = new THREE.Mesh(
	new THREE.TorusBufferGeometry(0.3,0.2,16,32),
  material
)
torus.position.x = 1.5
scene.add(sphere, plane, tours)
```

#### 添加一个动画

```js
const elapsedTime = new THREE.Clock()
const tick = () => {
  sphere.rotation.y = 0.1 * elapsedTime
  plane.rotation.y = 0.1 * elapsedTime
  tours.rotation.y = 0.1 * elapsedTime
  
  sphere.rotation.x = 0.15 * elapsedTime
  plane.rotation.x = 0.15 * elapsedTime
  tours.rotation.x = 0.15 * elapsedTime
  ...
}
```

#### 加载纹理

```js
const textureLoader = new THREE.TextureLoader()

const doorColorTexture = textureLoader.load('/textures/door/color.jpg')
const doorAlphaTexture = textureLoader.load('/textures/door/alpha.jpg') // 透明
const doorAmbientOcclusionTexture = textureLoader.load('/textures/door/ambientOcclusion.jpg') // 环境光遮蔽
const doorHeightTexture = textureLoader.load('/textures/door/height.jpg')
const doorNormalTexture = textureLoader.load('/textures/door/normal.jpg')
const doorMetalnessTexture = textureLoader.load('/textures/door/metalness.jpg') // 金属
const doorRoughnessTexture = textureLoader.load('/textures/door/roughness.jpg') // 粗糙感

const matcapTexture = textureLoader.load('/textures/matcaps/1.png')
const gradientTexture = textureLoader.load('/textures/gradients/3.png') // 渐变
```



### 有哪些Material

### MeshBasicMaterial

##### 常见的属性

```js
const material = new THREE.MeshBasicMaterial()
material.map = doorColorTexture
material.color = new THREE.Color('red')
material.wireframe = true
```

##### 透明度

```js
// 透明度
material.transparent = true
material.opacity = 0.5
```

当需要用到 透明度 时, 需要 开启 `transparent`属性才会生效, 当然 你也可以使用 `alphaMap`使用 `alpha`纹理来实现效果, alpha 纹理 会隐藏黑色部分

```js
material.transparent = true
material.alphaMap = doorAlphaTexture
```

##### Side 控制渲染

```js
material.side = THREE.DoubleSide

// THREE.FrontSide default
// THREE.BackSide 
// THREE.DoubleSide
```

### MeshNormalMaterial

使用这个材质可以得到  几何体的  法向量  , 可以帮助你判断  比如 几何体是否现在正处于 光照下

该材质拥有Basic 材质一样的属性 , 比如 `wireframe, transparent, opacity, side` 等

它有一个 特殊属性 `flatShading`  可以看到几何体是否平滑



该材质通常用来 调试 几何体



### MeshMatcapMaterial

该材质 可以 自动捕捉 (MatCap, 或者 光照球(Lit Sphere)) 这种纹理,  来自动生成材质的颜色 等

```js
const material = new THREE.MeshMatcapMaterial()
material.matcap = matcapTexture
```

该材质 可以 通过 matcap 图像文件 生成阴影 , 而且不会对灯光做出反应

[如何获取matcap图像](https://github.com/nidorx/matcaps)



### MeshDepthMaterial

一种按照 深度绘制 几何体的材质,  深度 基于相机的远近,  越近越白  越远越黑

```js
const material = new THREE.MeshDepthMaterial()
```



### MeshLamberMaterial

一种 对光有 反射的材质  漫反射

```js
const material = new THREE.MeshLamberMaterial()

// 记得添加光 不然 比什么也看不见
const ambientLight = new THREE.AmbientLight(0xffffff, 0.5)
scene.add(ambientLight)

const pointLight = new THREE.PointLight(0xffffff, 0.5)
pointLight.position.x = 2
pointLight.position.y = 3
pointLight.position.z = 4
scene.add(pointLight)
```



### MeshPhongMaterial

和 上面的一样,  但是 上面的反射为漫反射 可以看到一些 模糊,  而这个为 强反射

```js
const material = new THREE.MeshPhongMaterial()
```



#### 光泽度的效果

对光有反应的材质 可以设置  光泽度的效果

```js
material.shininess = 100  // 光泽度
material.speculat = new THREE.Color(px1188ff)  // 设置 反光的颜色

```

