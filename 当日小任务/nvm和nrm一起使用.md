## nvm

先将安装的 node 卸载掉

### 安装

去`github`仓库 下载 `nvm 安装包` 直接按照

### 命令

+ `nvm list available` : 查询可安装的版本
+ `nvm install 14.4.0` : 安装指定版本
+ `nvm uninstall 14.4.0` : 卸载指定版本
+ `nvm list` : 查看已安装版本
+ `nvm use 14.4.0` : 使用指定版本



## nrm

### 安装

```shell
npm install nrm -g
```



### 命令

+ `nrm ls` : 查看已保存配置
+ `nrm add taobao http://` : 添加名为 `taobao`的源
+ `nrm del taobao` : 删除源
+ `nrm use taobao` : 使用某个源



### 报错

`at Object.<anonymous> (C:\Users\Y_D\AppData\Roaming\npm\node_modules\nrm\cli.js:17:20)`

修改文件

```js
//const NRMRC = path.join(process.env.HOME, '.nrmrc');(注掉)
const NRMRC = path.join(process.env[(process.platform == 'win32') ? 'USERPROFILE' : 'HOME'], '.nrmrc');
```

