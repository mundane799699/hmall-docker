# docker compose部署黑马商城

### 资料下载

https://pan.baidu.com/s/1inZog6YV0f3_Qqx2WDNJag&pwd=9988

密码：9988

### 安装docker

https://www.runoob.com/docker/centos-docker-install.html

https://www.bilibili.com/video/BV1gr4y1U7CY/?p=11

### 关闭防火墙

查看防火墙状态

```
systemctl status firewalld.service
```

关闭防火墙

```
systemctl stop firewalld.service
```

禁止防火墙开机启动

```
systemctl disable firewalld.service
```

启动防火墙

```
systemctl start firewalld.service
```

### docker run

#### 第1步，准备文件

在当前目录下准备好docker-compose.yml、Dockerfile、hm-service.jar、mysql目录、nginx目录

#### 第2步，创建网络

```
docker network create hmall
```

#### 第3步，构建java镜像

```
docker build -t hmall .
```

#### 第4步，创建容器并加入网络

```
docker run -d \
--name hmall \
-p 8080:8080 \
--network hmall \
hmall
```

```
docker run -d \
--name mysql \
-p 3306:3306 \
-e TZ=Asia/Shanghai \
-e MYSQL_ROOT_PASSWORD=123 \
-v /root/mysql/data:/var/lib/mysql \
-v /root/mysql/init:/docker-entrypoint-initdb.d \
-v /root/mysql/conf:/etc/mysql/conf.d \
--network hmall \
mysql
```

```
docker run -d \
--name nginx \
-p 18080:18080 \
-p 18081:18081 \
-v /root/nginx/html:/etc/nginx/html \
-v /root/nginx/nginx.conf:/etc/nginx/nginx.conf \
--network hmall \:
nginx
```

### docker compose

在当前目录下准备好docker-compose.yml、Dockerfile、hm-service.jar、mysql目录、nginx目录

然后执行命令

```
docker compose up -d
```

### 常用命令

删除容器(强制)

```
docker rm -f hmall nginx mysql
```

删除网络

```
docker network rm hmall
```

查看网络

```
docker network ls
```

查看容器信息

```
docker inspect nginx
```

### 重装时要做的事

清空mysql的data目录

```
docker rm -f hmall nginx mysql
docker network rm hmall
docker rmi xxx
```

### 参考

[黑马程序员Docker快速入门到项目部署](https://www.bilibili.com/video/BV1HP4118797)

[Docker run轻松转Docker Compose](https://www.bilibili.com/video/BV1DN411p7Wr)