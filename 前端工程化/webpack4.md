# Webpack4

总结一下把, 总是忘记

`webpack`是什么? 它是一个用于现代`JavaScript`应用程序的静态模块打包工具

## 安装

```shell
# webpack 安装在项目中
npm install webpack webpack-cli -D
```



其实学习`webpack`就是学习常用的`loader`和`plugin`, 常见的一些有

+ **url-loader**
+ file-loader
+ 



## 常见的Loader

[官网资料](https://webpack.docschina.org/guides/asset-management/)

 ### 打包图片等资源

打包这种资源,主要使用的是`url-loader`和`file-loader`, 两者都是用来打包静态资源的, 其中`url-loader`比`file-loader`多了一个`limit`参数, 小于这个参数的文件会被转换成`base64`的资源, 大于这个参数的文件会被打包输出

```js
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(jpe?g|png|gif|webp)$/,
        use: {
          loader: 'url-loader',
          options: {
            limit: 2048,
            name: '[name].[hash:8].[ext]',
            outputPath: 'images/'
          }
        }
      }
    ]
  }
}
```

注意打包字体文件的时候,使用的就是`file-loader`

```
module.exports = {
	module: {
		rules: [
			{
				test: /\.(eot|ttf|svg)$/,
				use: ['file-loader']
			}
		]
	}
}
```



### 打包样式资源

使用的插件有

+ **style-loader** : 将打包的css挂载到页面中

+ **css-loader**: 打包css文件

+ **sass-loader**: 打包scss文件

+ **postcss-loader**

  这个插件是帮助我们给一些样式自动添加厂商前缀用的, 需要配置一个`postcss.config.js`文件,在文件内配置`plugin`使用`autoprefixer`插件

```js
// webpack.config.js
moduel.exports = {
    module: {
        rules: [
		  {
              test: /\.scss$/,
              use: [
                  'style-loader',
                  {
                      loader: 'css-loader',
                      options: {
                          importLoadaers: 2, // 注释1
                          modules: true // 注释2
                      }
                  },
                  'sass-loader',
                  'process-loader'
              ]
          }
        ]
    }
}
```

`注释1`: 当打包到的scss文件中又@import其他的文件的时候, 这个引入的文件也要从`process-loader`开始打包后才往下走

```shell
npm install --save-dev autoprefixer
```

创建`postcss.config.js` 文件在根目录

```js
module.exports = {
  plugins: [
      require('autoprefixer')
  ]
}
```

`注释2`: 是否开启css模块化,不开启,打包的css是全局的,不太好





## 常见插件

`plugin`的作用就是, 在webpack运行的某一个时刻,帮你做一些事情

### HtmlWebpackPlugin

会在打包结束后,生成html文件,并将打包的文件引入到html中

```js
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    plugins: [
        new HtmlWebpackPlugin({
            template: 'public/index.html',
            title: '标题'
        })
    ]
}
```

[指南 > 管理输出](https://webpack.docschina.org/guides/output-management/)

### Clean-webpack-plugin

用来每次打包之前,清除之前的dist文件下的东西, webpack4中可以在`output`中开启`clean`属性





## SourceMap

`sourcemap`是一个位置信息文件,记录的是打包后的文件和打包前的文件之间的位置映射关系

```js
// 推荐
{
    devtool: 'cheap-module-eval-source-map' // development
	devtool: 'cheap-module-source-map' // production
}
```

[指南 > 开发环境 > sourcemap](https://webpack.docschina.org/guides/development/#using-source-maps)

## WebpackDevServer使用

[指南 > 开发环境 > 开发工具](https://webpack.docschina.org/guides/development/#choosing-a-development-tool)

提高开发效率的三种做法

+ `webpack --watch` 添加命令, 代码变动自动打包
+ 使用`WebpackDevServer`, 配置config文件中的`devServe`
+ 使用`webpack-dev-middleware`, 这个是用来自己写一个开发环境,以前的`WebpackDevServer`不太好用的时候有人就自己写



## Hot Module Replacement(HMR)

使用的`HotModuleReplacement`插件, 当修改某个模块中的代码,只修改这个模块改变的地方不会影响其他的已经渲染的模块

在`devServe`中修改`hot`属性开启HMR

[指南 > 热模块替换](https://webpack.docschina.org/guides/hot-module-replacement/)

其他相关连接

[指南 > 模块热替换](https://webpack.docschina.org/concepts/hot-module-replacement/) HMR 原理