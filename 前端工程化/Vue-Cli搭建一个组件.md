## 搭建一个Vue组件库

[参考链接, 这人真牛逼](https://juejin.cn/post/6844904085808742407)

### 准备工作

首先通过 Vue-cli 常见一个初始项目, 然后修改下目录结构, 将`src`修改为`examples`目录, 然后根目录下创建一个`packages` 文件夹存放组件



+ `examples`修改下配置,里面还是和以前一样写业务代码, 比如用来测试组件什么的
+  `packages`中用来存放我们的组件代码



### 修改配置

修改`vue.config.js`文件, 配置`examples`

```js
module.exports = {
    page: {
        entry: 'examples/main.js',
        template: 'public/index.html',
        filename: 'index.html'
    },
    chainWebpack (config) {
        config.module
        	.rule('js')
        	.include.add('/packages')
            .end()
        	.use('babel')
        	.loader('bable-loader')
    }
}
```



修改`package.json`, 打包`packages`目录下的所有组件

```json
{
    "script": {
        "lib": "vue-cli-service build --target lib --name yui --dest lib packages/index.js"
    }
}
```

**解释 : ** 从某个主文件, 以包的形式打包 到根目录下的 lib 文件中



### 编写packages中的组件

每个组件都要有一个`install`方法, 方便按需引入时使用

```js
// packages/Button/index.js
import YButton from './src/index.vue'

YButton.install((Vue) => {
    Vue.component(YButton.name, YButton)
})

export default YButton
```

然后有一个全局的`install`方法, 方便全局引入

```js
// packages/index.js
import YButton from './Button/index.js'

const components = [ YButton ]

function install (Vue) {
   	components.map(component => Vue.component(component.name, component))
}

export default {
    install,
    YButton
}
```



### 配置package.json

可以查看**NPM基础**



### 上传npm

```
// 首先打包
npm run lib
// 配置 package.json
npm login
npm publish
```



### 使用

```js
// 全局引入
import yui from 'yui'
import 'yui/lib/yui.css'
Vue.use(yui)
```

这个只能说是一个简易版本的