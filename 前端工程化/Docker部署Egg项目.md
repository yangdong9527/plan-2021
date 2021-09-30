项目根目录下创建`Dockerfile`文件

```dockerfile
FROM node:16-alpine
# 在容器中新建目录文件夹 egg
RUN mkdir -p /egg
# 将 /egg 设置为默认的工作目录
WORKDIR /egg
# 将 package.json 复制到默认工作目录
COPY package.json /egg/package.json
# 安装依赖
RUN npm config set register http://registry.npm.taobao.org
RUN npm install
# 再 copy 代码到容器
COPY ./ /egg
#7001 端口
EXPOSE 7001
# 等容器启动后执行脚本
CMD npm run prod
```

然后在服务器运行

```shell
# 创建镜像
docker build -t egg:latest .
docker run -d -p 7001:7001 --name egg egg:latest
```

搞定

