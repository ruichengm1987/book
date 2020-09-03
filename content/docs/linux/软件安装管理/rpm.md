---
title: RPM
weight: 2
---
# RPM
## RPM包在系统光盘中

## RPM包命名规则
httpd-2.2.15-15.el6.CENTOS.1.i686.rpm  
-httpd 软件包名  
-2.2.15 软件版本 
-15 软件包发布的次数  
-el6.centos 适合的Linux平台
-i686 适合的硬件平台
-rpm rpm包扩展名  

## RPM包依赖性
* 树形依赖: a -> b -> c
* 环形依赖: a -> b -> c -> a
* 模块依赖: 模块依赖, 查询网站: www.rpmfind.net 

## 包全名与报名
* 包全名
操作的包是没有安装的软件包时,使用包全名,而且要注意路径  
* 报名
操作已经安装的软件包时,使用包名,是搜索/var/lib/rpm中的数据库

## RPM安装
* rpm -ivh 包全名
* 选项:
  -i  (install) 安装
  -v  (verbose) 显示详细信息
  -h  (hash) 显示进度
  --nodeps 不检测依赖性
  
## RPM升级与卸载
* rpm -Uvh  包全名
* 选项:
    -U (upgrade) 升级



## RPM