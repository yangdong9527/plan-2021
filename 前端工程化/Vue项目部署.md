# Vue项目部署的那些事

## Vue项目基础配置

### 模式和环境变量

首先在Vue CLI中需要搞清楚的几个概念 : 模式 , 环境变量

Vue CLI 默认添加了三种模式 : 分别是 `development production  test` , 可以通过 `--mode` 选项 修改命令行默认模式

```
vue-cli-service build --mode development
```

运行`vue-cli-service`时`环境变量`都从对应的环境文件中载入, 如果文件内没有`NODE_ENV`变量, 就取决于模式,

默认的`production`模式下环境变量就是`production`

简单来说, Vue CLI 提供了三种默认的模式,当你运行`vue-cli-service`命令时,`环境变量`会从对应的环境文件中载入,而`环境变量`会决定应用程序的运行那种模式, 不同的环境变量会生成不同的webpack配置



创建一个预发布环境, 实际操作如下:

在项目根目录下创建环境文件: `.env.production `

```
NODE_ENV="production"
VUE_APP_URL="http://www.abc.com"
```

然后在 package.json 文件中配置

```json
"script": {
    "build_preproduction": "vue-cli-service build --mode production"
}
```

## Nginx的基础配置

## Docker容器部署

