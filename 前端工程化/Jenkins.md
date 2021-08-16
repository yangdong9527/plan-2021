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

# 还可以有其他的挂载
-v /usr/bin/mv:/usr/bin/mv
-v /usr/bin/docker:/usr/bin/docker
-v /var/run/docker.sock:/var/run/docker.sock

# 配置镜像加速
vim /var/jenkins_home/hudson.model.updateCenter.xml

# 修改成 https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

> ​	/var/jenkins_home 为Jenkins的工作目录  /etc/localtime 同步服务器时间

> ​	特别注意安装完Jenkins后, 安装的Node的插件版本和项目版本匹配

### Jenkins配置GitHub

**获取配置GitHub token**

获取步骤 :  github官网 => setting => Developer settings => Personal access tokens => Generate new token => 勾选 repo 和 admin:repo_hook => 复制token

配置步骤 :  系统管理 => 系统配置 =>GitHub => Add

**配置项目的webHook**

配置项目的webHook : GitHub项目 => setting => Webhooks => add => 填入jenkins地址 => 勾选push或自定义

这一步的目的比如 当 github 接收到 push 等操作时, 会触发一个 hook, 然后 根据地址回调Jenkins, 然后Jenkins完成自动构建

**配置ssh**

```
ssh-keygen -t rsa -C "18855238097@163.com"
```



### Centos安装Jenkins

```shell
# 安装JDK
yum install -y java
# 安装jenkins 两种方法
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
yum install -y jenkins

# 配置jenkins端口
vi /etc/sysconfig/jenkins

#启动
service jenkins start
```



