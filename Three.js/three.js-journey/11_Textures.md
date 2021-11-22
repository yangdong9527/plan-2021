### 直接加载图片纹理

```js
const image = new Image()
const texture = new THREE.Texture(image)
image.onload = () => {
    texture.needsUpdate = true
}
image.src = '../img.png'

const material = new THREE.MeshBasicMaterial({ map: texture })
```

### 使用TextureLoader

```js
const textureLoader = new THREE.TextureLoader()
const texture = textureLoader.load('../img.png')
```

### 使用LoadingManager

```js
const loadingManager = new THREE.LoadingManager()
loadingManager.onStart = () => {
    console.log('start')
}
// progress
// error
// ...

// 添加到 TextureLoader中
const textureLoader = new THREE.TextureLoader(loadingManager)
```

### UA

```js
const geometry = new THREE.BoxBufferGeometry(1,1,1)
console.log(geometry.attributes.uv)
```

### textures transform

```js
const colorTexture = textureLoader.load('./a.png')

// repeat 设置渲染面积 ,  wrapS x 轴未渲染的应该怎么渲染, wrapT 表示Y轴怎么渲染
colorTexture.repeat.x = 2
colorTexture.repeat.y = 3
colorTexture.wrapS = THREE.RepeatWrapping
colorTexture.wrapT = THREE.MirroredRepeatWrapping

// 从加载的图片 那个位置开始渲染, 最大为1
colorTextrue.offset.x = 0.5
colorTexture.offset.y = 0.5

colorTexture.rotation = Math.PI / 4
colorTexture.center.x = 0.5
colorTexture.center.y = 0.5
```

#### repeat

默认值为 1,1  表示, 一整个页面 直接渲染,   当设置为 4,4 , 表示 将页面 x 分成 4 份 y 轴分成4 份, 然后 最小的一个页面进行渲染

### minFilter

当物体像素小于纹理像素, 边界会被模糊 , 使用这个 可以看的更清楚

```js
 colorTexture.minFilter = THERR.NearestFilter

// THREE.LinearFilter
// THREE.NearestMipmapNearestFilter
// ....
```

### magFilter

当纹理像素小于物体像素

```js
colorTexture.generateMipmaps = false
colorTexture.minFilter = THREE.NearestFilter
colorTexture.magFilter = THREE.NearestFilter
```



