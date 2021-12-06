### 创建一个星系

```js
const parameters = {
  count: 100000,
  radius: 5,
  size: 0.01,
  branches: 3,
  spin: 1,
  randomness: 0.2,
  randomnessPower: 2.294,
  insideColor: '#ff6030',
  outsideColor: '#1b3984'
}
let geometry,material,points
const generateGalaxy = () => {

  if (points) {
    geometry.dispose()
    material.dispose()
    scene.remove(points)
  }
  
  geometry = new BufferGeometry()
  const colorInside = new THREE.Color(parameters.insideColor)
  const colorOutside = new THREE.Color(parameters.outsideColor)

  const positions = new Float32Array(parameters.count * 3)
  const colors = new Float32Array(parameters.count * 3)
  for (var i = 0; i < parameters.count; i++) {
    const i3 = i * 3

    const radius = Math.random() * parameters.radius
    // 分支
    const branchAngle = (i % parameters.branches) * (Math.PI * 2 / parameters.branches)
    // 自旋
    const spinAngle = radius * parameters.spin
    // 添加随机偏移值
    const randomX = Math.pow(Math.random(), parameters.randomnessPower)
    const randomY = Math.pow(Math.random(), parameters.randomnessPower)
    const randomZ = Math.pow(Math.random(), parameters.randomnessPower)

    positions[i3] = Math.cos(branchAngle + spinAngle) * radius + randomX
    positions[i3 + 1] = randomY
    positions[i3 + 2] = Math.sin(branchAngle + spinAngle) * radius + randomZ

    // colors
    const mixerColor = colorInside.clone()
    mixerColor.lerp(colorOutside, radius / parameters.radius)

    colors[i3] = mixerColor.r
    colors[i3 + 1] = mixerColor.g
    colors[i3 + 2] = mixerColor.b

  }
  geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3))
  geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3))
  // material
  material = new THREE.PointsMaterial({
    size: parameters.size,
    sizeAttenuation: true,
    depthWrite: false,
    blending: THREE.AdditiveBlending,
    vertexColors: true
  })
  points = new THREE.Points(geometry, material)
  
  scene.add(points)
}
generateGalaxy()
```

主要功能

+ 如何自定义模型, 生成粒子
+ 如何生成多个分支
+ 如何添加自旋角度
+ 如何添加随机值
+ 如何添加颜色, 记得开启 顶点着色器( vertexColors )