## R4S软路由上搭建

- **下载安装程序**
```
cd /usr/bin && wget https://github.com/alist-org/alist/releases/download/v3.35.0/alist-linux-musl-arm64.tar.gz && tar -zxvf alist-linux-musl-arm64.tar.gz && chmond +x alist && rm alist-linux-musl-arm64.tar.gz -y
```
- **创建开机启动脚本**
```
vim /etc/init.d/alist
```
写入以下内容
```
#!/bin/sh /etc/rc.common
START=99
start() {
    cd /usr/bin #alist的路径
    ./alist server #启动alist
}
stop() {
    killall alist
}
```
- **赋予脚本执行权限**
```
chmod +x /etc/init.d/alist
```
- **启动脚本**
```
/etc/init.d/alist enable --now



