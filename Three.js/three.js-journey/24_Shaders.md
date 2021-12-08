学习目标

+ 什么是 着色器, 为啥使用
+ 创建自己的简单的 着色器
+ 学习语法
+ 做练习



### lesson01

**什么是着色器**

着色器材质是用GLSL编写的小程序, 运行在GPU上,能够提供materials之外的效果

**两种着色器**

`顶点着色器(vertexShader)`  首先运行,接收**attribute**,计算每个单独顶点的位置, 并将其他数据(**varyings**)传递给片元着色器

`片元着色器(fragmentShader)`后运行, 它设置渲染到每个单独片元(像素)的颜色

三种类型的变量: `uniforms, attributes, varyings`

+ `Uniforms`是所有顶点都具有相同的值, 比如, 灯光 雾, 阴影贴图等, 被存储在 uniforms 中, 可以通过两个着色器来访问这些
+ `Attributes`是与每个顶点关联的变量, 例如, 顶点位置, 法线 和 顶点颜色都是存储在 attributes 中的数据, attributes 只能在顶点着色器中访问
+ `Varyings`是从顶点着色器传递给片元着色器的变量, 对于每个片元, 每个 varying 的值将是相邻顶点值的平滑插值

**为什么使用着色器**

+ three.js 中的 materials 存在限制, 比如: 像实现波浪的效果
+ 更好的优化
+ 可以做后期处理



### lesson02

**创建着色器材质**

有两种方式创建, `ShaderMaterial` 和 `RawShaderMaterial` , 两者的区别是, 前者会被添加一些内置的`uniforms 和 attributes`



**RawShaderMaterial使用**

```js
const material = new THREE.RawShaderMaterial({
  vertexShader: ``,
  fragmentShader: ``
})
```

**GLSL**语法

+ 变量 `float, int, vec2, vec3, vec4`

**小点总结**

+ `vertexShader`控制顶点,  可以获取几何体中的 `attribute`属性, 还可以定义 `varyings`属性传递给`fragmentShader`
+ `fragmentShader`渲染可视的每个像素, 能接收到`varyings`



**使用Uniforms**

传递 `Uniforms`到着色器中





### 本章学习总结

three.js 提供了两种  **着色器材质: **  `RawShaderMaterial` 和 `ShaderMaterial`, 两者的唯一区别是, 后者自动的添加了一些内置的 `uniform 和 attribute`, 其他的使用起来是一样的, 这里就以`RawShaderMaterial`来举例子

#### 着色器的分类和常量

提供了两种着色器, 一种是`顶点着色器(vertexShader)` 一种是 `片元着色器(fragmentShader)`, 分别用来控制当前像素点位置坐标, 和当前像素点的渲染方式

在着色器之间流窜这三个常量: `uniform , attribute, varying`

+ `uniform` 是可以在两个着色器中都可以访问到的, 有点全局的意思
+ `attribute`是只能在`顶点着色器`中访问到, 它获取的是几何体中的`attributes`属性中的值
+ `varying`是`顶点着色器`中定义的值, 可以在`片元着色器`中进行接收

#### 代码

创建一个着色器材质

```js
const material = new THREE.RawShaderMaterial({
  vertexShader: testVertexShader,
  fragmentShader: testFragmentShader
})
```

提供`vertexShader 和 fragmentShader`, 两者都需要使用 GLSL 语法进行书写

```glsl
// testVertexShader
uniform mat4 modelMatrix;
uniform mat4 viewMatrix;
uniform mat4 projectionMatrix;

attribute vec3 position;

void main()
{
  vec4 modelPosition = modelMatrix * vec4(position, 1.0);
  vec4 viewPosition = viewMatrix * modelPosition;
  vec4 projectedPosition = projectionMatrix * viewPosition;
  
  gl_Position = projectedPosition;
}
```

```glsl
// testFragmentShader
precision mediump float;

void main()
{
  gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);
}
```

参数的传递

```js
// 通过 uniforms 可以传递到全局
const material = new THREE.RawMaterial({
  ...
  uniforms: {
    uTime: { value: 0 }
  }
})

// testVertexShader 中
uniform float uTime;

void main () {
  ...
  modelPosition.z += sin(modelPosition.x * 10.0 - uTime)
}
```

渲染纹理

加载纹理

```js
const textureLoader = new THREE.TextureLoader();
const flagTexture = textureLoader.load('/textures/flag-china.jpg')
const material = new THREE.RawShaderMaterial({
  uniforms: {
    uTexture: {
      value: flagTexture
    }
  }
})
```

传递UV

```glsl
// testVertexShader
attribute vec2 uv;

varying vec2 vUv; // 传递给 fragment
void main()
{
  vUv = uv;
}
```

渲染

```glsl
uniform sampler2D uTexture;
varying vec2 vUv;

void main()
{
  vec4 textureColor = texture2D(uTexture, vUv);
  gl_FragColor = textureColor;
}
```



`ShaderMaterial`和`RawShaderMaterial`的区别就是 不用写 `attribute 和 uniform`来接收一些值

