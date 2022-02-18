---
title: docker
bookCollapseSection: true
weight: 10
---

# Docker
## 1、概念
* 镜像：可以理解为软件安装包，可以方便的进行传播和安装。
* 容器：软件安装后的状态，每个软件运行环境都是独立的、隔离的，称之为容器。

### 1.1、安装
https://docs.docker.com/engine/install/#server
### 1.2、镜像加速源
| 镜像加速器 | 镜像加速器地址 |
| ----- | ---- |
|Docker 中国官方镜像	| https://registry.docker-cn.com |
|DaoCloud 镜像站	|http://f1361db2.m.daocloud.io |
|Azure 中国镜像	|https://dockerhub.azk8s.cn |
|科大镜像站	|https://docker.mirrors.ustc.edu.cn |
|阿里云	|https://<your_code>.mirror.aliyuncs.com |
|七牛云	|https://reg-mirror.qiniu.com|
|网易云	|https://hub-mirror.c.163.com|
|腾讯云	|https://mirror.ccs.tencentyun.com|

通过:registry-mirrors参数配置

## 2、Docker 快速安装软件
### 2.1、演示 Docker 安装 Redis
Docker 官方镜像仓库查找 Redis ：https://hub.docker.com/  
#### 安装: docker run -d -p 6379:6379 --name redis redis:latest

## 3、命令参考
https://docs.docker.com/engine/reference/run/

### 3.1、仓库相关
| 命令 | 描述 |
| ---- | ----|
|**docker search $KEY_WORD**             |**查找镜像** |
|**docker pull $REGISTRY:$TAG**         |**获取镜像** |
|**docker push $IMAGE_NAME:$IMAGE_TAG**  |**推送镜像到仓库，需要先登录**|
|**docker login $REGISTRY_URL**          |**登录仓库**|
|**docker logout $REGISTRY_URL**         |**退出仓库**|
|**docker info**                         |**显示Docker详细的系统信息，可查看仓库地址**|
|**docker version**                      |**显示Docker信息**|
|**docker --help**                       |**显示Docker的帮助信息**|

### 3.2、容器相关
| 命令 | 描述 |
| ---- | ----|
|docker attach $CONTAINER_ID |启动一个已存在的docker容器
|**docker stop $CONTAINER_ID**  |**停止docker容器**
|**docker start $CONTAINER_ID**  |**启动docker容器**
|**docker restart $CONTAINER_ID**|**重启docker容器**
|**docker kill $CONTAINER_ID**   |**强制关闭docker容器**
|docker pause $CONTAINER_ID  |暂停容器
|docker unpause $CONTAINER_ID|恢复暂停的容器
|docker rename $CONTAINER_ID |重新命名docker容器
|**docker rm $CONTAINER_ID**     |**删除容器**
|**docker logs $CONTAINER_ID**   |**查看docker容器运行日志，确保正常运行**
|**docker inspect $CONTAINER_ID**|**查看container的容器属性，比如ip等等**
|**docker port $CONTAINER_ID**   |**查看container的端口映射**
|**docker top $CONTAINER_ID**    |**查看容器中正在运行的进程**
|**docker commit $CONTAINER_ID** $NEW_IMAGE_NAME:$NEW_IMAGE_TAG|**将容器保存为镜像**
|**docker ps -a**                |**查看所有容器**
|**docker stats**                |**查看容器的资源使用情况**

### 3.3、镜像相关
| 命令 | 描述 |
| ---- | ----|
|**docker images**                    |**查看本地镜像**
|**docker rmi $IMAGE_ID**              |**删除本地镜像**
|**docker rmi -f $(docker images -qa)**   |**删除本地所有镜像**
|**docker inspect $IMAGE_ID**          |**查看镜像详情**
|docker save $IMAGE_ID > 文件路径  |保存镜像为离线文件
|docker save -o 文件路径 $IMAGE_ID |保存镜像为离线文件
|docker load < 文件路径            |加载文件为docker镜像
|docker load -i 文件路径           |加载文件为docker镜像
|**docker tag $IMAGE_ID $NEW_IMAGE_NAME:$NEW_IMAGE_TAG** |**修改镜像TAG**
|**docker run 参数 $IMAGE_ID $CMD**    |**运行一个镜像**
|**docker history $IMAGE_ID**          |**显示镜像每层的变更内容**

### 3.4、run命令相关
| 参数 | 描述 |
| ---- | ----|
| **-d** |**后台运行容器, 并返回容器ID；不指定时, 启动后开始打印日志, Ctrl+C退出命令同时会关闭容器**
| **-i** |**以交互模式运行容器, 通常与-t同时使用**
| **-t** |**为容器重新分配一个伪输入终端, 通常与-i同时使用**
| **--name container_name** |**设置容器名称, 不指定时随机生成**
| -h container_hostname |设置容器的主机名, 默认随机生成
| --dns 8.8.8.8 |指定容器使用的DNS服务器, 默认和宿主机一致
| -e docker_host=172.17.0.1 |设置环境变量
| --cpuset="0-2" or --cpuset="0,1,2" |绑定容器到指定CPU运行
| -m 100M |设置容器使用内存最大值
| --net bridge |指定容器的网络连接类型, 支持bridge/host/none/container四种类型
| --ip 172.18.0.13 |为容器指定固定IP（需要使用自定义网络none）
| --expose 8081 --expose 8082 |开放一个端口或一组端口，会覆盖镜像设置中开放的端口
| **-p [宿主机端口]:[容器内端口]** |**宿主机到容器的端口映射，可指定宿主机的要监听的IP，默认为0.0.0.0**
| -P |注意是大写的, 宿主机随机指定一组可用的端口映射容器expose的所有端口
| -v [宿主机目录路径]:[容器内目录路径] | 挂载宿主机的指定目录（或文件）到容器内的指定目录（或文件）
| --add-host [主机名]:[IP] |为容器hosts文件追加host, 默认会在hosts文件最后追加[主机名]:[容器IP]
| --volumes-from [其他容器名] |将其他容器的数据卷添加到此容器
| --link [其他容器名]:[在该容器中的别名] |添加链接到另一个容器，在本容器hosts文件中加入关联容器的记录，效果类似于--add-host

### 3.5、进入当前正在运行的容器
docker exec -it 容器id /bin/bash  #进入容器开启新的终端
docker attach 容器id /bin/bash  #进入容器正在执行的终端,不会启动新的进程

### 3.6、从容器拷贝文件到主机
docker cp 容器id:容器内路径 目的的主机路径

## 4、容器数据卷
### 4.1、使用数据卷
#### 4.1.1、直接使用命令来挂载 -v
* docker run -it -v [宿主机目录路径]:[容器内目录路径] centos /bin/bash
* docker inspect 容器id   #查看Mounts
双向同步的,在容器中改，或者在宿主机改，都会互相同步的。

#### 4.1.2、匿名挂载
* -v 容器内路径:    
docker run -d -P -name nginx01 -v /etc/nginx nginx
* 查看所有卷的情况
docker volume ls

#### 4.1.3、具名挂载
* -v 卷名:容器内路径:    
docker run -d -P -name nginx02 -v juming-nginx:/etc/nginx nginx
* 查看一下这个卷
docker volume inspect 卷名

#### 4.1.4、指定路径挂载
* -v 宿主路径:容器内路径  

#### 4.1.5、扩展
docker run -d -P -name nginx02 -v juming-nginx:/etc/nginx:ro nginx  #容器不能修改
ro: 只读, rw:可读写

所有的docker容器内的卷,没有指定目录的情况下都是在 /var/lib/docker/volumes/xxxx/_data

### 4.2、通过Dockerfile
参考5、Dockerfile

### 4.3、数据卷容器
利用容器给别的容器共享数据  
docker run -it --name docker01 [容器名称/id]  
docker run -it --name docker02 --volumes-from docker01 [容器名称/id]

## 5、Dockerfile
Dockerfile是用来构建docker镜像文件
### 5.1、构建步骤
* 编写一个dockerfile文件
* docker build 构建成一个镜像
* docker run 运行镜像
* docker push 发布镜像(DockerHub、阿里云镜像仓库)

### 5.2、Dockerfile的指令
```aidl
FROM #基础镜像,一切从这里开始构建
MAINTAINER   #镜像是谁写的, 姓名+邮箱
RUN          #镜像构建的时候需要运行的命令
ADD          #步骤,tocat镜像；添加内容
WORKDIR      #镜像的工作目录
VOLUME       #挂载的目录位置
EXPOSE       #暴露端口
CMD          #指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT   #指定这个容器启动的时候要运行的命令,可以追加命令
ONBUILD      #当构建一个被继承DockerFile 这个时候就会运行ONBUILD的指令、触发指令
COPY         #类似ADD，将我们文件拷贝到镜像中
ENV          #构建的时候设置环境变量
```
### 5.3、实战
scratch 最基础镜像
#### 5.3.1、编写dockerfile文件
```
FROM centos
MAINTAINER mrc<mrc@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80
CMD echo $MYPATH
CMD echo "----end----"
CMD /bin/bash
```
#### 5.3.2、构建
docker build -f dockerfile文件路径 -t 镜像名:[tag] .

### 5.4、研究人家dockerfile如何做的
docker history 镜像id

### 5.5、CMD和ENTRYPOINT 区别
CMD          #指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT   #指定这个容器启动的时候要运行的命令,可以追加命令

### 5.6、发布自己的镜像
git push [容器名]:[版本号]

## 6、Docker网络
### 6.1、原理
我们每启动一个docker容器 docker就会给docker容器分配一个ip,我们只要安装了docker，就会有一个网卡docker0

桥接模式，使用的技术是evth-pair技术

```aidl
我们发现这个容器带来的网卡，都是一对对的  
evth-pair 就是一对虚拟设备接口，他们都是成对出现的，一段俩这协议，一段彼此相连
正以为有这个特性，evth-pair充当了桥梁，连接各种虚拟网络设备的
openstac docker容器之间的连接，都是使用evth-pair技术
```

容器和容器之间是可以互相ping通的

### 6.2、--link
思考一个场景，我们编写了一个微服务,database url=ip 项目不重启，数据库ip换掉了，我们希望可以处理这个问题。可以名字来进行访问吗？

docker run -d -P -name tomcat03 --link tomcat02 tomcat   
docker exec -it tomcate ping tomcate02

### 6.3、自定义网络
#### 6.3.1、网络模式
* bridge: 桥接docker （默认,自己创建也使用bridge模式）
* none: 不配置网络
* host: 和宿主机共享网络
* container: 容器网络连通 (用的少,局限很大)

```aidl
#我们直接启动的命令 --net 吧 ridge， 而这个就是我们的docker0
docker  run -d -P --name tomcat01 --net bridge tomcat

# docker0特点, 默认，域名不能访问, --link可以打通连接

# 我们可以自定义一个网络
# --driver bridge
# --subnet 192.168.0.0/16 192.168.0.0/24
# --gateway 192.168.0.1
docker network create  --driver bridge --subnet 192.168.0.0、16 --gateway --gateway 192.168.0.1 mynet
```
我们自定义的网络docker都已经帮我们维护好了对应的关系，推荐我们平时这样使用网络

#### 6.3.2、网络连通
docker network connect mynet tomcat01

## 7、redis集群部署实战
// todo::

## 8、微服务打包docker镜像

## 9、Docker compose

## 10、Docker Swarm

## 11、CICD


