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

