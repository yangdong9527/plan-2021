实现HTML和3D模型的混入, 可以将3D模型上面的点, 给映射到2D的屏幕上的HTML元素

主要用到的其实就是 `v3.project(camera)` 能获取到元素2D屏幕的法向量, 注意是已中心点作为其实点的





判断 该点是否可见了 

首先判断 该点 和 模型射线之间有没有相交, 如果没有, 那么肯定可以看见,  如果有相交, 则判断 相交点和 该点距离相机的位置, 如果 相交点的距离 大于 该点与相机的距离, 则表示可以看见



### 准备工作

创建HTML元素, 定位到视图的`中心点`, 然后准备好 3D 中的一个定位点 作为HTML相映射的点位

```js
const points = [
  {
    position: new THREE.Vector3(1.55,0.3, -0.6),
    element
  }
]
```



### 建立HTML和作表点的位置关系

``` js
// tick
const tick = () => {
  
  for (const point of points) {
    // 获取v3 在 屏幕上的 向量, 和我们之前求 鼠标在屏幕中的向量一样, 正中心为 0 点, 
    const screenPosition = point.position.clone()
    screenPosition.project(camera) // 返回 v2
    
    // 然后计算偏移量
    const translateX = screenPosition.x * (sizes.width / 2)
    const translateY = - screenPosition.y * (sizes.height / 2)  // 记得 取反
    point.element.style.transform = `translate(${translateX}px, ${translateY}px)`
  }
}
```



### 判断点位是否显示隐藏

原理是, 使用 `Raycaster` , 接收 `点位的二位坐标`, 判断是否存在相交点

如果不存在相交点, 那么 该点是没有被遮挡的

如果存在相交点, 判断 相交点 到相机的距离 A , 和 真实的v3坐标点到相机的就 B

如果 A > B, 则没遮挡, 如果 B > A 表示被遮挡了

```js
const raycaster = new THREE.Raycaster()

// tick 
const tick = () => {
  // ...
  
  raycaster.setFromCamera(screenPosition, camera)
  const intersects = raycaster.intersectObjects(scene.children, true)
  
  if (intersects.length === 0) {
    point.element.classList.add('visible')
  } else {
    const intersectionDistance = intersects[0].distance
    const pointDistance = point.position.distanceTo(camera.position)
    
    if (intersectionDistance > pointDistance) {
      point.element.classList.add('visible')
    } else {
      point.element.classList.remove('visible')
    }
  }
  
}
```

