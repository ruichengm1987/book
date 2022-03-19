---
title: 五、依赖管理go mod
bookCollapseSection: true
weight: 5
---

# 五、依赖管理go mod  
* 由go命令统一的管理,用户不必关心目录结构
* 初始化: go mod init
* 增加依赖: go get
* 更新依赖: go get [@v...], go mod tidy
* 将旧项目迁移到go mod: go mod init, go build ./...

## Go mod下如何引用本项目的包呢？

大家只要记住这个公式即可：
```$xslt
import的路径 = go.mod下的module name + 包相对于go.mod的相对目录

## go编译多个
go build ./... 编译当前和子目录下的main文件，不产生二机制文件
go install ./... 编译当前和子目录下的main文件，产生二机制文件。在 GOPATH目录的bin目录下
```

