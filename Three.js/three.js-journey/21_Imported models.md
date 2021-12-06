### 模型

#### 获取模型

github上的 `gltf-sample-models`



### GLTF 格式

+ glTF
+ glTF-Binary
+ glTF-Draco

前两者大小差不多, 前者优势可以访问然后修改, 后者不可修改的二进制但是 读取时会更快一些, 第三个文件大小被压缩



### 加载模型

```js
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'

const gltfLoader = new GLTFLoader()

gltfLoader.load('/models/FlightHelmet/glTF/FlightHelmet.gltf',
  (gltf) => {
    console.log(gltf)
    // const children = [...gltf.scene.children]
    // for (let item of children) {
    //   scene.add(item)
    // }
    scene.add(gltf.scene)
  }
)
```

### Draco加载模型

```js
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
import { DRACOLoader } from 'three/examples/jsm/loaders/DRACOLoader.js'

const gltfLoader = new GLTFLoader()
const dracoLoader = new DRACOLoader()
dracoLoader.setDecoderPath('/draco/')
gltfLoader.setDRACOLoader(dracoLoader)
gltfLoader.load('/models/Duck/glTF-Draco/Duck.gltf',
  (gltf) => {
    console.log(gltf)
    // const children = [...gltf.scene.children]
    // for (let item of children) {
    //   scene.add(item)
    // }
    scene.add(gltf.scene)
  }
)
```

### animation

```js 
let mixer = null

gltfLoader.load('/models/Fox/glTF-Binary/Fox.glb',
  (gltf) => {
    mixer = new THREE.AnimationMixer(gltf.scene) // 生成动画控制器
    const action = mixer.clipAction(gltf.animations[2]) // 加载动画
    action.play()
    gltf.scene.scale.set(0.025,0.025,0.025)
    scene.add(gltf.scene)
  }
)


// tick
const clock = new THREE.Clock()
let previousTime = 0
const tick = () => {
	const elapsedTime = clock.getElapsedTime()
    const deltaTime = elapsedTime - previousTime
    previousTime = elapsedTime
    if (mixer) {
      mixer.update(deltaTime)
    }
}
```

