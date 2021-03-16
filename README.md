# go的坑爹之路
## 1. go get无法安装beego
原因：国内的牛逼网络环境
```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

## 2. 添加 GOPATH 到环境变量
原因：找不到 bee命令
```
vim ~/.bash_profile
增加：
  export GOPATH="$HOME/go
  export PATH="$GOPATH/bin:$PATH
source ~/.bash_profile
```
## 3. 安装 beego 和 beego 工具：
```
go get -u github.com/beego/beego/v2

go get -u github.com/beego/bee/v2
```

# docker部署beego应用
目前的网络应用都使用容器化部署，趁着在公司摸鱼的时间摸索了一下如何使用docker部署一个beego helloword 程序

## 1. docker 的安装
见官网

## 2. beego 的安装
见官网

## 3. 创建 helloworld 程序

```bash
# 格式 bee new <name>
$ bee new hello
# 使用 tree 命令，看看创建了些什么
$ tree hello
# 关于目录的具体解释建议看 beego 官网的文档
hello
├── conf
│   └── app.conf
├── controllers
│   └── default.go
├── go.mod
├── go.sum
├── hello
├── lastupdate.tmp
├── main.go
├── models
├── routers
│   └── router.go
├── static
│   ├── css
│   ├── img
│   └── js
│       └── reload.min.js
├── tests
│   └── default_test.go
└── views
    └── index.tpl

```

## 4. 制作 docker image
```bash
$ cd hello
$ mkdir build
$ cd build
$ vim build.sh

#!/bin/bash
export GOOS=linux
go build hello

$ vim Dockerfile

FROM golang:latest # 基础镜像，因为beego基于golang的环境，这里使用latest，但是建议根据具体的需求选择适当的TAG。
RUN mkdir -p /hello # 新建文件夹用于存放构建脚本和Dockerfile
COPY hello/ /hello # 将对应文件夹下的内容拷贝到容器的目录下
WORKDIR /hello/doc # 切换工作目录
CMD ["/hello/hello"] # 运行hello程序

$ cd .. # 很重要

$ docker build -f ./hello/build/Dockerfile . -t justzyzhang:latest

# 看看是否成功
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED             SIZE
justzyzhang   latest    d41db69b23ab   About an hour ago   881MB
# 
```
## 6. 使用制作完成的 docker image 部署应用
```
$ docker run --name test2 -p 8085:8080 -d justzyzhang:latest
```
## 7. 将 docker image push 到远程镜像仓库
```
$ docker login
$ docker tag justzyzhang:latest slimhappy/justzyzhang:v1.0
$ docker push slimhappy/justzyzhang:v1.0
```