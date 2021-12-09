渲染小技巧



### 工具

安装

```shell
npm i stats.js 
```

配置

```js
import Stats from 'stats.js'

const stats = new Stats()
stats.showPanel(0)
document.body.appendChild(stats.dom)


// tick
const tick = () => {
  stats.begin()
  
  
  stats.end()
}
```



### 小点

**移除不需要的模型**

```js
scene.remove(cube)
cube.geometry.dispose()
cube.material.dispose()
```

**避免使用 光 阴影**



**优化阴影**

如果使用了阴影 优化



**关闭阴影自动更新**

如果场景是固定的 就是 光源不会发生变化,  关闭阴影自动更新 

```js
renderer.shadowMap.autoUpdate = false  // 停止自动更新
renderer.shadowMap.needsUpdate = true  // 只在必要时更新
```



**渲染多个相同的网格模型**

不要循环创建每个几何体, 只需要创建一个几何体 , 然后多次调用就行



**合并多个几何体, 实现上一步**

可以创建多个几何体 然后 通过 `BufferGeometryUtils` 合并成一个几何体, 然后渲染成一个 网格模型



**相同的材质创建一次**



合并多个几何体变成了一个整体, 无法单独对某个进行操作



#### 模型





