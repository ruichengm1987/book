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
* 包名
操作已经安装的软件包时,使用包名,是搜索/var/lib/rpm中的数据库

## RPM包安装
* rpm -ivh 包全名
* 选项:
  -i  (install) 安装
  -v  (verbose) 显示详细信息
  -h  (hash) 显示进度
  --nodeps 不检测依赖性
  
## RPM包升级
* rpm -Uvh  包全名
* 选项:
    -U (upgrade) 升级

## RPM包卸载
* rpm -e httpd 包名
* 选项: 
    -e （erase）卸载
    --nodeps 不检查依赖性 

## RPM包查询
### 1.查询是否安装
* rpm -q 包名   #查询包是否安装
* 选项:  
    -q  查询

* rpm -qa
* 选项:   
    -a  所有(all)  
    
rpm -qa|grep "test"  

### 2.查询软件包详细信息  
* rpm -qi 包名  
* 选项:  
    -i  查询软件信息  
    -p  查询未安装包信息  

### 3.查询包中文件安装位置
* rpm -ql 包名
* 选项:
    -l 列表  
    -p 查询未安装包信息  
    
默认安装位置  

|  路径   | 描述  |
|  ----  | ----  |
| /etc/  |配置文件安装目录 |
| /usr/bin/  |可执行的命令安装目录|
| /usr/lib/  |程序所使用的函数库保存位置|
| /usr/share/doc  |基本的软件使用手册保存位置|
| /usr/share/man  |帮助文件保存位置 |

### 4.查询系统文件属于哪个RPM包
* rpm -qf 系统文件名
* 选项:  
    -f  查询系统文件属于哪个软件包
    
### 5.查询软件包的依赖性
* rpm -qR 包名
* 选项: 
    -R  查询软件包的依赖性
    -p  查询未安装包的信息

## RPM包校验
### 1.rpm包校验
* rpm -V 已安装的包名
* 选项:  
    -V 检验指定RPM包中的文件
```aidl
# rpm -V httpd
S.5....T. c /etc/httpd/conf/httpd.conf
```
验证内容中的8个信息的具体内容如下:    
S  文件大小是否改变  
M  文件的类型或文件的权限(rwx)是否被改变  
5  文件MD5校验和是否改变(可以看成文件内容是否改变)  
D  设备的主从代码是否改变  
L  文件路径是否改变  
U  文件的属主(所有者)是否改变  
G  文件的属组是否改变    
T  文件的修改时间是否改变  

文件类型:  
c  配置文件  
d  普通文档  
g  "鬼"文件, 很少见,就是该文件不应该被这个RPM包包含  
L  授权文件  
r  描述文件  

### 2.RPM包中文件提取
* rpm2cpio 包全名| cpio -idv .文件绝对路径  
  -rpm2cpio 将rpm包转换为cpio格式的命令  
  -cpio  是一个标准工具,它用于创建软件档案文件和从档案文件中提取文件  
  
* cpio 选项 < [文件|设备]
* 选项: 
    -i copy-in模式, 还原   
    -d 还原时自动新建目录  
    -v 显示还原过程  
    
过程:  
* 查询ls命令属于哪个软件包 
 rpm -qf /bin/ls  
* 造成ls命令误删除假象
 mv /bin/ls/tmp
* 提取RPM包中ls命令到当前目录的/bin/ls下  
rpm2cpio /mnt/cdrom/PACKAGES/coreutils-8.4-19.el6.i686.rpm| cpio -idv ./bin/ls 
* 把ls命令复制到/bin/目录,修复文件丢失
cp  /root/bin/ls  /bin/
  