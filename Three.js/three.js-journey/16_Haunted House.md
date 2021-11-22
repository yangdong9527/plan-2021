#### 添加幽灵

#### 阴影

#### 阴影优化

### House

创建房子相关的元素

```js
// house
const house = new THREE.Group()
scene.add(house)

// wall
const walls = new THREE.Mesh(
  new THREE.BoxBufferGeometry(4, 2.5, 4),
  new THREE.MeshStandardMaterial({ color: '#ac8e82' })
)
walls.position.y = 1.25
house.add(walls)

// Roof
const roof = new THREE.Mesh(
  new THREE.ConeBufferGeometry(3.5, 1, 4),
  new THREE.MeshStandardMaterial({ color: '#b35f45' })
)
roof.position.y = 3
roof.rotation.y = Math.PI * 0.25
house.add(roof)

// door
const door = new THREE.Mesh(
  new THREE.PlaneBufferGeometry(2, 2),
  new THREE.MeshStandardMaterial({ color: '#b35f45' })
)
door.position.y = 1
door.position.z = 2 + 0.01
house.add(door)

// Bushes
const bushGeometry = new THREE.SphereBufferGeometry(1,16,16)
const bushMaterial = new THREE.MeshStandardMaterial({ color: '#89c854' })

const bush1 = new THREE.Mesh(bushGeometry, bushMaterial)
bush1.scale.set(0.5, 0.5, 0.5)
bush1.position.set(0.8,0.2,2.2)

const bush2 = new THREE.Mesh(bushGeometry, bushMaterial)
bush2.scale.set(0.25, 0.25, 0.25)
bush2.position.set(1.4,0.1,2.1)

const bush3 = new THREE.Mesh(bushGeometry, bushMaterial)
bush3.scale.set(0.4, 0.4, 0.4)
bush3.position.set(-0.8,0.1,2.2)
const bush4 = new THREE.Mesh(bushGeometry, bushMaterial)
bush4.scale.set(0.15, 0.15, 0.15)
bush4.position.set(-1,0.05,2.6)
house.add(bush1, bush2, bush3, bush4)
```



### 创建石头

#### 创建一个随机的圆形位置

```js
const graves = new THREE.Group()
scene.add(graves)

const graveGeometry = new THREE.BoxBufferGeometry(0.6,0.8,0.2)
const graveMaterial = new THREE.MeshStandardMaterial({ color: '#b2b6b1' })

for(var i = 0; i < 30; i++) {
  const angle = (Math.random() - 0.5) * Math.PI * 2
  const radius = (Math.random() * 4) + 5
  const x = Math.sin(angle) * radius
  const z = Math.cos(angle) * radius

  const grave = new THREE.Mesh(graveGeometry, graveMaterial)
  grave.position.set(x, 0.32, z)
  grave.rotation.y = (Math.random() - 0.5) * Math.PI * 2
  grave.rotation.z = (Math.random() - 0.5) * 0.4
  graves.add(grave)
}
```



### 创建灯光

```js
const ambientLight = new THREE.AmbientLight('#b9d5ff', 0.12)
scene.add(ambientLight)

const moonLight = new THREE.DirectionalLight('#b9d5ff', 0.12)
moonLight.position.set( 4, 5, -2)
scene.add(moonLight)

const doorLight = new THREE.PointLight('#ff7d46', 1, 7)
doorLight.position.set(0, 2.2,2.7)
scene.add(doorLight)
```



### 创建雾

```js
const fog = new THREE.Fog('#262837', 1, 15)
scen.fog = fog
```

参数说明 以相机为中心点 能看清楚的范围

#### 清除外边界

```js
renderer.setClearColor('#262837')
```



### 添加纹理

#### door texture

```js
// door texture
const doorColorTexture = textureLoader.load('/textures/door/color.jpg')
const doorAmbientOcclusionTexture = textureLoader.load('/textures/door/ambientOcclusion.jpg')
const doorAlphaTexture = textureLoader.load('/textures/door/alpha.jpg')
const doorHeightTexture = textureLoader.load('/textures/door/height.jpg')
const doorNormalTexture = textureLoader.load('/textures/door/normal.jpg')
const doorMetalnessTexture = textureLoader.load('/textures/door/metalness.jpg')
const doorRoughnessTexture = textureLoader.load('/textures/door/roughness.jpg')

const door = new THREE.Mesh(
  new THREE.PlaneBufferGeometry(2.2, 2.2, 100, 100),
  new THREE.MeshStandardMaterial({
    map: doorColorTexture,
    transparent: true,
    alphaMap: doorAlphaTexture,
    aoMap: doorAmbientOcclusionTexture,
    aoMapIntensity: 2,
    displacementMap: doorHeightTexture,
    displacementScale: 0.1,
    normalMap: doorNormalTexture,
    metalnessMap: doorMetalnessTexture,
    roughnessMap: doorRoughnessTexture
  })
)
// 内阴影 添加 UV
door.geometry.setAttribute(
  'uv2',
  new THREE.Float32BufferAttribute(door.geometry.attributes.uv.array, 2)
)
```

注意 `aoMap` 内阴影几何体提供额外的 UV 坐标 , `displacementMap`置换纹理需要 提供多的 顶点 才能生效

#### floor texture

```js
// grass texture
const grassColorTexture = textureLoader.load('/textures/grass/color.jpg')
const grassAmbientOcclusionTexture = textureLoader.load('/textures/grass/ambientOcclusion.jpg')
const grassNormalTexture = textureLoader.load('/textures/grass/normal.jpg')
const grassRoughnessTexture = textureLoader.load('/textures/grass/roughness.jpg')

const floor = new THREE.Mesh(
  new THREE.PlaneBufferGeometry(20,20),
  new THREE.MeshStandardMaterial({ 
    map: grassColorTexture,
    aoMap: grassAmbientOcclusionTexture,
    normalMap: grassNormalTexture,
    roughnessMap: grassRoughnessTexture
  })
)
floor.geometry.setAttribute(
  'u2v',
  new THREE.Float32BufferAttribute(floor.geometry.attributes.uv.array, 2)
)
```

### 添加移动的光