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
docker run -d -p 9001:8080 -p 9002:50000 -v /var/jenkins_mount:/var/jenkins_home -v /etc/localtime:/etc/localtime --name myjenkins jenkins

# 配置镜像加速
vim /var/jenkins_home/hudson.model.updateCenter.xml

# 修改成 https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

> ​	/var/jenkins_home 为Jenkins的工作目录  /etc/localtime 同步服务器时间

