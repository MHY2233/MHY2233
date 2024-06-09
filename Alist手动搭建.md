# [最新版本](https://github.com/alist-org/alist/releases)


## 在VPS服务器上部署

- **下载安装程序**
```
cd /usr/local/bin && wget https://github.com/alist-org/alist/releases/download/v3.35.0/alist-linux-musl-amd64.tar.gz && tar -zxvf alist-linux-musl-amd64.tar.gz && chmod +x alist && rm alist-linux-musl-amd64.tar.gz
```
- **设置密码**
```
./alist admin set password
```
- **创建service文件**
```
vim /usr/lib/systemd/system/alist.service
```
写入以下内容
```
[Unit]
Description=alist
After=network.target
 
[Service]
Type=simple
WorkingDirectory=path_alist
ExecStart=/usr/local/bin/alist server
Restart=on-failure
 
[Install]
WantedBy=multi-user.target
```
- **开启程序**
```
systemctl daemon-reload && systemctl enable --now alist
```

## 在R4S软路由上部署

- **下载安装程序**
```
cd /usr/bin && wget https://github.com/alist-org/alist/releases/download/v3.35.0/alist-linux-musl-arm64.tar.gz && tar -zxvf alist-linux-musl-arm64.tar.gz && chmond +x alist && rm alist-linux-musl-arm64.tar.gz -y
```
- **设置密码**
```
./alist admin set password
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
```


