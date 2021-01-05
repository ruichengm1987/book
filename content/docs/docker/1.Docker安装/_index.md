---
title: 1.Docker安装
bookCollapseSection: true
weight: 1
---

# 1.Docker安装

## 1.1 安装
* yum包更新到最新
```aidl
yum update
```
* 安装需要的软件包, yum-util提供yum-config-manager功能,另外两个是devicemapper驱动依赖的
```aidl
yum install -y yum-utils device-mapper-persistent-data lvm2
```
* 设置yum源为阿里云
```aidl
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
* 安装docker
```aidl
yum -y install docker-ce
```
* 安装后查看docker版本
```aidl
docker -v
```

## 1.2 设置ustc的镜像
ustc是老牌的linux镜像服务提供者了, ustc的docker镜像加速器速度很快。ustc docker mirror的优势之一就是
不需要注册,是真正的公共服务.

编辑该文件
```aidl
mkdir -p /etc/docker
vi /etc/docker/daemon.json
```
在该文件中输入如下内容
```aidl
{
"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```

## 1.3 Docker的启动与停止
启动docker:
```aidl
systemctl start docker
```

查看docker状态:
```aidl
systemctl status docker
```

查看docker信息
```aidl
docker info
```

停止docker
```aidl
systemctl stop docker
```

docker帮助信息
```aidl
docker --help
```

docker重启
```aidl
systemctl restart docker
```