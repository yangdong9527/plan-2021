### 准备

创建粒子

```js
const particlesGeometry = new THREE.SphereBufferGeometry(1,32,32)

const particlesMaterial = new THREE.PointsMaterial() 
particlesMaterial.size = 0.02 // 设置顶点 大小
particlesMaterial.sizeAttenuation = true // 会随着相机的远近大小不一样

const particles = new THREE.Points(particlesGeometry, particlesMaterial)
scene.add(particles)
```



### 创建自己的几何体

```js
const particlesGeometry = new THREE.BufferGeometry()
const count = 500
const position = new Float32Array(count * 3)
for (var i = 0; i < count * 3; i++) {
  position[i] = (Math.random() - 0.5) * 10
}
particlesGeometry.setAttribute(
	'position',
  new THREE.BufferAttribute(position, 3)
)
```



### 粒子使用材质

```js
const particleMaterial = new THREE.PointsMaterial()
particleMaterial.size = 0.1
particleMaterial.sizeAttenuation = true
particlesMaterial.transparent = true
particlesMaterial.alphaMap = particleTexture
```

### 材质

### alphaText 修复边缘

```js
particlesMaterial.alphaTest = 0.001
```

#### depthTest 修复

```js
particlesMaterial.depthTest = false
```

如果有其他的几何体或者有其他的颜色 都会出问题

#### depthWrite 修复

```js
particlesMaterial.depthWrite = false
```

#### blending  混合

```js
particleMaterial.blending = THREE.AdditiveBlending
```

#### color

设置几何体各个顶点的颜色

```js
const colors = new Float32Array(count * 3)
for (var i = 0; i < count * 3; i++) {
  colors[i] = Math.random()
}
geometry.setAttribute('color',new THREE.BufferAttribute(colors, 3) )
```

记得需要添加属性才能生效

```js
particlesMaterial.vertexColors = true
```



