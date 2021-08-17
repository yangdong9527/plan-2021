创建容器

```
docker pull mysql
docker run -d -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 mysql
```

Docker 安装mysql后, node连接报错

```shell
# 进入mysql容器
docker exec -it mysql sh
# 进入mysql数据库 输入密码
mysql -uroot -p;

#远程连接授权
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;

#更改加密规则
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;

#更新root用户密码
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';

#刷新权限
FLUSH PRIVILEGES;
```

重启容器