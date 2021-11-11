### 什么是Geometry

+ Geometry 是由顶点组成的



### 内置几何体

```js
const geometry = new THREE.BoxGeometry(1,1,1,2,2,2)
const material = new THREE.MeshBasicMaterial({
    color: 0xff0000,
    wireframe: true
})
const mesh = new THREE.Mesh(geometry, material)
```

### 如何创建自己的几何体

先添加顶点 , 然后 创建 Face 根据添加进去的顶点的索引, 然后添加 Face

```js
const geometry = new THREE.geometry()
const vertex1 = new THREE.Vector3(0,0,0)
geometry.vertices.push(vertex1)
const vertex2 = new THREE.Vector3(0,1,0)
geometry.vertices.push(vertex2)
const vertex3 = new THREE.Vector3(1,0,0)
geometry.vertices.push(vertex3)
const face = new THREE.Face3(0,1,2)
geometry.faces.push(face)
```

### BoxBufferGeometry

性能非常的好 所以要使用这个

```
const geomatry = new THREE.BufferGeometry()
const vertices = new Float32Array([
	0,1,0,
	0,0,1,
	1,0,0
])
// 创建BufferAttribute
const attribute = new THREE.BufferAttribute(vertices, 3)
geomatry.setAttribute('position', attribute)
```

+ 几何体的position 和 Mesh 网格的 positon 不同
+ 几何体都是由 vertices 顶点 ， 组成的， 没 3 个顶点可以确定三角形