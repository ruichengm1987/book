---
title: Linux如何关闭某个占用端口的进程
weight: 1
---

# Linux如何关闭某个占用端口的进程
## 1）查找被占用的端口:
```aidl
netstat -tln | grep 8000  
tcp        0      0 192.168.2.106:8000      0.0.0.0:*                 LISTEN  
```

## 2）查看被占用端口的PID：  
```aidl
sudo lsof -i:8000
COMMAND PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
nginx   850     root    6u  IPv4  15078      0t0  TCP 192.168.2.106:8000 (LISTEN)
nginx   851 www-data    6u  IPv4  15078      0t0  TCP 192.168.2.106:8000 (LISTEN)
nginx   852 www-data    6u  IPv4  15078      0t0  TCP 192.168.2.106:8000 (LISTEN)
```
3）kill掉该进程
```aidl
sudo kill -9 850
```