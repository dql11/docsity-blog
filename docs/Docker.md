[TOC]



# Docker

## Docker环境安装

- 安装yum-utils：

```bash
yum install -y yum -utils device-mapper-persistent-data lvm2
```

- 为yum源添加docker仓库位置：

```bash
yum-config-manager --add-repo https://download.com/linux/centos/docker-ce.repo
```

- 安装docker：

```bash
yum install docker-ce
```

- 启动docker：

```bash
systemctl start docker
```

## Docker镜像常用命令

### 搜索镜像

```bash
docker search java
```

### 下载镜像

```bash
docker pull java:8
```

### 如何查找镜像支持

> 通过docker hub

- 进入docker hub官网 https://hub.docker.com
- 搜索需要的镜像：

```bash
docker pull nginx:1.17.0
```

### 列出镜像

```bash
docker images
```

### 删除镜像

- 指定名称删除镜像

```bash
docker rmi java:8
```

- 指定名称删除镜像（强制）

```bash
docker rmi -f java:8
```

- 删除所有没有引用的镜像

```bash
docker rmi 'docker images | grep none | awk '{print $3}''
```

- 强制删除所有镜像

```bash
docker rmi -f $(docker images)
```

## Docker 容器常用命令

### 新建并启动容器

```bash
docker run -p 80:80 --name nginx -d nginx:1.17.0
```

- -d :表示后台运行
- --name：指定运行后容器的名字为nginx，以后可以用名字来操作容器
- -p ：指定端口映射，格式为：hostPort：containerPort

### 列出容器

- 列出运行中的容器

```bash
docker ps
```

- 列出所有容器

```bash
docker ps -a
```

### 停止容器

```bash
docker stop $ContainerName(或者$ContainerId)可用docker ps查出来
```

### 强制停止容器

```bash
docker kill $ContainerName($ContainnerId)
```

### 启动已停止的容器

```bash
docker start $ContainerName
```

### 进入容器

- 先查出容器的pid:

```bash
docker inspect --format "{{.State.Pid}}" $ContainerName
```

- 根据容器的pid进入容器：

```bash
nsenter --target "$pid" --mount --uts --ipc --net --pid
```

### 删除容器

- 删除指定容器

```bash
docker rm $ContainerName
```

- 按名称删除容器

```bash
docker rm 'docker ps -a | grep mall-* | awk '{print $1}''
```

- 强制删除所有容器

```bash
docker rm -f $(docker ps -a -q)
```

### 查看容器的日志

- 查看当前全部日志

```bash
docker logs $ContainerName
```

- 动态查看日志

```bash
docker logs $ContainerName -f
```

### 查看容器的IP地址

```bash
docker insperct --format '{{.NetworkSettings.IPAddress}}' $ContainerName
```

修改容器的启动方式

```bash
docker container update --restart=always $ContainerName
```

同步宿主机时间到容器

```bash
docker cp /etc/localtime $ContainerName:/etc/
```

### 在宿主机查看docker使用cpu、内存、网络、io情况

- 查看指定容器情况：

```bash
docker stats $ContainerName
```

- 查看所有容器情况

```bash
docker stats -a
```

### 查看Docker磁盘使用情况

```bash
docker system df
```

### 进入Docker容器内部的bash

```bash
docker exec -it $ContainerName /bin/bash
```

### 修改Docker镜像的存放位置

- 查看Docker镜像的存放位置：

```bash
docker info | grep "Docker Root Dir"
```

- 关闭Docker服务：

```bash
systemctl stop docker
```

- 移动目录到目标路径

```bash
mv /var/lib/docker /mydata/docker
```

- 建立软连接：

```bash
ln -s /mydata/docker /var/lib/docker
```

