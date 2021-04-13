---
bookCollapseSection: true
weight: 84
---

# 8.4.Go工程实践
## 工程项目结构
### Standard Go Project Layout  
[https://github.com/golang-standards/project-layout/blob/master/README_zh.md](https://github.com/golang-standards/project-layout/blob/master/README_zh.md)
* /cmd 本项目的主干
每个应用的程序的目录名应该与你想要的可执行文件的文件名相匹配(例如:/cmd/myapp),不要放太多代码
* /internal  私有应用程序和库代码, 不希望其它人在其他应用程序或库中导入代码  
/internal/app   
/internal/app/myapp   
/internal/pkg  
* /pkg
```
pkg
|-- cache
|   | -- mecache
|   | -- redis
|-- conf
|   |-- dsn
|   |-- env
```
* Kit Project Layout
每个公司都应该为不同的微服务建立一个统一的kit工具包  
kit项目必须具备的特点
    * 统一
    * 标准库方式布局
    * 高度抽象
    * 支持插件
    
    
### Service Application Project Layout  
* /api  
api协议定义目录.xxapi.proto protobuf文件,以及生成的go文件.我们通常把api文档直接在proto文件中描述

* /configs
配置文件模板或默认配置

* /test
额外的外部测试应用程序和测试数据
## API设计
## 配置管理
## 包管理
## 测试
## References

观看到: Go进阶训练营-1.工程化实践(上) 40:20

