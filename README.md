# go的坑爹之路
## 1. go get无法安装beego
原因：国内的牛逼网络环境
```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```
