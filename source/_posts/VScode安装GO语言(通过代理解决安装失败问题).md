---
title: VScode安装GO语言
date: 2023-02-20 15:34:35
categories:
- Golang
---
## VScode安装GO插件
在VScode中安装GO插件后
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230220153743.png)

VScode会提示你安装插件
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230220153817.png)
点击install all 大概率会安装失败
```
Installing github.com/mdempsky/gocode FAILED  
Installing github.com/uudashr/gopkgs/v2/cmd/gopkgs FAILED  
Installing github.com/ramya-rao-a/go-outline FAILED  
Installing github.com/acroca/go-symbols FAILED  
Installing golang.org/x/tools/cmd/guru FAILED  
Installing golang.org/x/tools/cmd/gorename FAILED  
Installing github.com/cweill/gotests/… FAILED  
Installing github.com/fatih/gomodifytags FAILED
-------------------------------------------------------
```
这是由于go语言是由Google开发，国内很难连接
## 配置代理
可以通过官方的代理网站进行配置[goproxy](https://goproxy.io/)
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230220154346.png)
```
# 旧版，已废弃
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io,direct
```
```
# 新版改成如下链接
go env -w GO111MODULE=on
go env -w GOPROXY=https://proxy.golang.com.cn,direct
```
关闭vscode重新打开，再次点击install all
![image.png](https://cdn.staticaly.com/gh/K-Viior/blog-image@master/img/20230220154439.png)
成功安装


也可以进行手动安装
```
go get -u -v github.com/mdempsky/gocode
go get -u -v github.com/uudashr/gopkgs/v2/cmd/gopkgs
go get -u -v github.com/ramya-rao-a/go-outline
go get -u -v github.com/acroca/go-symbols
go get -u -v golang.org/x/tools/cmd/guru
go get -u -v golang.org/x/tools/cmd/gorename
go get -u -v github.com/cweill/gotests/...
go get -u -v github.com/fatih/gomodifytags
go get -u -v github.com/josharian/impl
go get -u -v github.com/davidrjenni/reftools/cmd/fillstruct
go get -u -v github.com/haya14busa/goplay/cmd/goplay
go get -u -v github.com/godoctor/godoctor
go get -u -v github.com/go-delve/delve/cmd/dlv
go get -u -v github.com/stamblerre/gocode
go get -u -v github.com/rogpeppe/godef
go get -u -v github.com/sqs/goreturns
go get -u -v golang.org/x/lint/golint

```