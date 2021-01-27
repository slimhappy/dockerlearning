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
