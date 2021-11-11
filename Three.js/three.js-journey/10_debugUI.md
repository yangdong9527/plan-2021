创建的调试工具有很多

+ dat.gui



### dat.gui

```powershell
npm i dat.gui -S
```

使用

```js
import * as dat from 'dat.gui'

const gui = new dat({ closed: true })

//debug
// 调整数值
gui.add(mesh.position, 'y', -3, 3, 0.01).name('geometry y')
// 调整布尔值
gui.add(material, 'wireframe')
// 控制颜色
const parameters = {
    color: 0xff0000，
    spin: () => {
        gasp.to(mesh.rotation, { duration: 1, y: mesh.rotation.y + 10 })
    }
}
gui.addColor(parameters, 'color').onChange(() => {
    material.color.set(parameters.color)
})
// 按钮点击事件

```

