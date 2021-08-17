## 安装Node

使用 `nvm`来安装 Node 

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash
# 或者
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash
```

安装完以后可以需要 退出当前窗口 重新连接以下 nvm的命令才有显示

其他的一些操作命令

```shell
# 查看所有的Node.js版本
nvm ls-remote
# 安装最新的Node
nvm install node
# 安装指定版本Node
nvm install v16.0.0
# 查看安装了哪些版本
nvm ls
# 切换Node
nvm use 14.0.0
# 卸载Node
nvm uninstall v14.0.0

```

**安装完成后报错**

```shell
# command not found
cd ~/.nvm
vim .bash_profile
# 输入以下内容保存

export NVM_DIR="/root/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"

# 执行环境变量
source .bash_profile
```

