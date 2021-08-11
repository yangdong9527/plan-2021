## 如何发布组件

### 基础知识

#### NPM账号

想要发布包,在你运行`npm publish`前需要注意以下:

+ 注册一个账号
+ 查看`.npmrc`设置, 确保你的registry是www.npmjs.com
+ `npm login --registry http://registry.npmjs.org`

#### package.json字段

先登录好, 然后完善以下 `package.json`中的字段

| 字段        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| name        | 发布的包名, 报名应该全部小写, 中划线分割                     |
| version     | 版本`magor.minor.path`, 如果`major`变动表示不兼容修改, 如果`minor`变动 表示添加向后兼容的新功能, 如果`patch`修改,意味着bug修复 |
| description | 包的描述,在官网搜索时有助于筛选                              |
| keywords    | 关键字 同样帮助用户筛选                                      |
| author      | 作者信息                                                     |
| license     | Vue官方项目写的是MIT                                         |
| repository  | 配置完成 npm页面会有一个github入口 **注释1**                 |
| main        | 定义包的入口文件, 当你使用 import xxx from 包名 的时候, 就是导入`main`定义的文件 |
| engines     | 告诉用户需要的 Node 版本                                     |

**注释1**: repository 格式

```json
"repository": {
  "type": "git",
  "url": "https://github.com/...git"
}
```

#### 忽略文件

`.gitignore`文件和`.npmignore`文件都是忽略某些文件夹, 如果`.gitignore`忽略了dist文件夹, 但是发包时想保留就在package.json中设置`files`属性

```json
"files":["dist"]
```

#### 发布

一个版本只能发布一次, 为避免手动修改package.json,可以使用`npm version 1.0.0`

#### 打标签

假设你的包最新版本是`1.0.0`, 这时候当用户直接安装不携带版本时,就会按照你的最新的包

```shell
npm install my-package
npm install my-package@1.0.0
npm install my-package@latest
```

这三个代码是等价的,假如你开发了`2.0.0`但是不稳定,想发布测试一下,又不能直接发布变成`latest`版本,这时候就可以打标签

```shell
npm publish --tag beta
```

则用户`npm install my-package`安装的还是`1.0.0`,而`npm install my-package@beta`才是`2.0.0`

`npm dist-tag ls`查看npm包一共有几个标签



