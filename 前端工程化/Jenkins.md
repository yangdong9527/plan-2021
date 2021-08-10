## 安装

### Docker安装Jenkins

准备工作: 我们创建一个Jenkins的挂载目录,然后给与权限

```shell
mkdir -p /var/jenkins_mount
chmod 777 /var/jenkins_mount
```

安装容器

```shell
docker pull jenkins/jenkins
docker run -d -p 9001:8080 -p 9002:50000 -v /var/jenkins_mount:/var/jenkins_home -v /etc/localtime:/etc/localtime --name myjenkins jenkins/jenkins

# 配置镜像加速
vim /var/jenkins_home/hudson.model.updateCenter.xml

# 修改成 https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

> ​	/var/jenkins_home 为Jenkins的工作目录  /etc/localtime 同步服务器时间



### Jenkins配置GitHub

**第一步** : 获取配置GitHub token

获取步骤 :  github官网 => setting => Developer settings => Personal access tokens => Generate new token => 勾选 repo 和 admin:repo_hook => 复制token

配置步骤 :  系统管理 => 系统配置 =>GitHub => Add

**第二部** : 配置项目的webHook

配置项目的webHook : GitHub项目 => setting => Webhooks => add => 填入jenkins地址 => 勾选push或自定义

这一步的目的比如 当 github 接收到 push 等操作时, 会触发一个 hook, 然后 根据地址回调Jenkins, 然后Jenkins完成自动构建

