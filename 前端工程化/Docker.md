## Docker 安装

### Linux系统安装Docker

Docker已经为我们准备好了各个系统的安装包,直接安装即可

#### CentOS

```
$ sudo yum install yum-utils device-mapper-persistent-data lvm2
$
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
$ sudo yum install docker-ce
$
$ sudo systemctl enable docker
$ sudo systemctl start docker
```



#### 配置国内镜像

在Linux环境下, 修改`/etc/docker/daemon.json`(如果没有这个文件, 可以直接创建)

```json
{
    "registry-mirrors": [
        "https://registry.docker-cn.com"
    ]
}
```

