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

### MeshToonMaterial

可使用该卡通材质 实现渐变效果

```js
const material = new THREE.MeshToonMaterial()

matterial.gradientMap = gradentTexture
```

注意 对于较小的纹理 图片, three.js 会进行拉伸模糊 来处理 记得添加以下处理

```js
gradientTexture.minFilter = THREE.NearestFilter
gradientTexture.magFilter = THREE.NearestFilter
gradientTexture.generateMipmaps = false // 为了性能关闭
```



### MeshStandardMaterial

一个支持光 , 并且支持 `roughness 和 metalness`  粗糙光泽 和 金属

```js
const material = new THREE.MeshStandardMaterial()
material.metalness = 0.45
material.roughness = 0.45

// 添加一个纹理
material.map = doorClorTexture
```

#### 添加一个 阴影效果

```js
// 要想添加 阴影效果 , 需要额外 提供一组 UV 数据给几何体 来对于 阴影材质渲染的 UV坐标
const plane = new THREE.Mesh(
	new THREE.PlaneBufferGeometry(1,1),
    material
)
plane.geometry.setAttribute(
	'uv2',
    new THREE.BufferAttribute(plane.geometry.attributes.uv.array, 2)
)

// 然后添加 纹理
material.aoMap = doorAmbientOcclusionTexture
material.aoMapIntensity = 10 // aoMap 强度
```



#### displacementMap

可以用来实现一种  `突出` 来的感觉,  纹理 看上去 真实,  

当你添加了 `这种效果没有展示出来 可能是你的 几何体 没有足够的顶点来支持 `

这种材质的 特点 height.png 为例,  白色 会 上升 黑色 下降, 当处于 灰色  则不动

```js
material.displacementMap = doorHeightTexture
material.displacementScale  = 0.05 // 位移强度
//  添加顶点
new THREE.PlaneBufferGeometry(1,1,100,100)

```

#### 金属 和 粗糙/光泽 效果

```js
material.metalnessMap = doorMetalnessTexture // 金属效果
material.roughnessMap = doorToughnessTexture  // 粗糙 光泽 就是是否反光的效果

// 这两个 不要一起使用 下面的会影响 贴图中的效果 或者设置成 0 和 1
material.metalness = 0
material.roughness = 1
```



#### normalMap

这个贴图可以 给物体添加 很多细节点 , 比如 门上 雕刻的花纹 凹槽 这种都可以展示出来

```js
material.normalMap = doorNormalTexture
material.normalScale.set(0.5, 0.5) // 细节 强度
```

#### alphaMap

可以控制显示和隐藏

```js
material.transparent = true
material.alphaMap = doorAlphaTexture
```



### MeshPhysicalMaterial

它可 MeshStandardMaterial 一样, 但是 会在外部添加一层 透明的薄膜的效果



### 实现 环境贴图

```js
const material = new THREE.MeshStandardMaterial()
material.metalness = 0.7
material.roughness = 0.2

// 加载环境贴图
const cubeTextureLoader = new THREE.CubeTextureLoader()

const environmentMapTexture = cubeTextureLoader.load([
    // 六张图片 顺序  +x -x +y -y  +z -z
])

material.envMap = environmentMapTexture

// 可通过 调整 金属 和 粗糙程度 来控制 清楚与否
```

HDRIHaven  可以获取环境贴图

