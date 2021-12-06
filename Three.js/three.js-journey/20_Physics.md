### 物理库

3D物理库

+ Ammo.js
+ Cannon.js
+ Oimo.js

2D物理库

+ Matter.js
+ P2.js
+ Planck.js
+ Box2D.js

### World

```js
import CANNON from 'Cannon'

const world = new CANNON.World()
world.gravity.set(0, -9.82, 0)
```

### 使用

#### 创建物理几何体

创建一个物理的球几何体, 然后球的运动 效果添加到 three的 球的效果上

``` =>
const sphereShape = new CANNON.Sphere(0.5) // 形状
const sphereBody = new CANNON.Body({ // body
  mass: 1, //质量
  position: new CANNON.Vec3(0,3,0),
  shape: sphereShape // 形状
})

world.addBody(sphereBody)

// 记得更新
tick = () => {
	// 帧数, 上次用的时间, 
	world.step( 1/60, deltaTime, 3)
	// console.log(sphereBody.position)
	// 然后我们就可以物理的模型的值 给到我们的 three中的模型
	sphere.position.copy(sphereBody.position)
}
```

#### 创建一个地板

```js
const floorShape = new CANNON.Plane()
const floorBody = new CANNON.Body()
floorBody.mass = 0 // 物体会被锁死 
floorbody.addShape(floorShape)
floorbody.quaternion.setFromAxisAngle( // 旋转平面, 注意物理的plane 是无限的 没有边界
	new CANNON.Vec3(-1,0,0),
  Math.PI * 0.5
)
world.addBody(floorBody)

```

#### 设置物理材质

首先创建两个材质, 名字可以是任何名字不重要

```js
const concreteMaterial = new CANNON.Material('concrete')
const plasticMaterial = new CANNON.Material('plastic')
```

然后我们需要定义一个关联 两个材质碰撞到一起后会 发生的事情 比如 摩擦 是否反弹等等

```js
const concretePlasticContactMaterial = new CANNON.ContactMaterial(
	concreteMaterial,
  plasticMaterial,
  {
  	friction: 0.1, // 摩擦度
    restitution: 0.7 // 恢复程度
  }
)
// 将这种材质添加进去
world.addContactMaterial(concretePlasiticContactMaterial)

// 然后记得 给之前的 body 添加 材质
const sphereBody = new CANNON.Body({
  mass: 1,
  position: new CANNON.Vec3(0,3,0),
  sphere: sphereShape,
  material, plasticMaterial
})
```



#### 如何简化上一步的操作

可能我们只需要一种材质碰撞, 整个世界使用 同一种材质

```js
const defaultMaterial = new CANNON.Material('default')
const defaultContactMaterial = new CANNON.ContactMaterial(
	defaultMaterial,
  defaultMaterial,
  {
    friction: 0.1,
    restitution: 0.7
  }
)
world.addContactMaterial(defaultContactMaterial)
```

然后有两种方式可以设置 统一材质, 一种方式 给每个模型 应用这个材质  一种方式 直接设置默认材质

```js
const sphereBody = new CANNON.Body({
  ...
  material: defaultMaterial
})
```

或者

```js
world.defaultContactMaterial = defaultContactMaterial
```



#### 添加一个力

使用`applyLocalForce`产生一个力, 这个力是从`自身`内产生的

```js
const sphereBody = new CANNON.Body({
  ...
})
// 传入,  力的方向  力的偏移位置 , 0,0,0 表示 物体的中心点产生力
sphereBody.applyLocalForce(new CANNON.Vec3(150, 0,0), new CANNON.Vec3(0,0,0))
```

如何模仿一个风的力, 在 `tick` 中 添加 `world`之前

```js
const tick = () => {
  sphereBody.applyForce(new CANNON.Vec3(-0.5,0,0), sphereBody.position)
}
```



### 实际操作

创建模型和物理模型函数

优化

物理立方体模型的创建 和 three.js 创建方式不一样

解决碰撞后的旋转问题

