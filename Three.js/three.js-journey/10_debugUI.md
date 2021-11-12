创建的调试工具有很多

+ dat.gui



### dat.gui

```powershell
npm i dat.gui -S
```

使用

```js
import * as dat from 'dat.gui'

const params = {
  color: 0xff0000,
  spin: () => {
    gsap.to(mesh.rotation, { y: mesh.rotation.y + 10 })
  }
}
const gui = new dat.GUI({ closed: true })

const gui = new dat.GUI({ closed: true })
gui.add(mesh.position, 'y').min(-3).max(3).step(0.01).name('mesh.y')
gui.add(mesh, 'visible')
gui.add(material, 'wireframe')
gui.addColor(params, 'color').onChange(() => {
  material.color.set(params.color)
})
gui.add(params, 'spin')
```

