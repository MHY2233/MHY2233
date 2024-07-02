## Snell节点搭建

# [Snell 官方手册](https://manual.nssurge.com/others/snell.html)

- **1. 下载 Snell Server 安装包**
X86_AMD64
```
wget https://dl.nssurge.com/snell/snell-server-v4.0.1-linux-amd64.zip
```
ARM 
```
wget https://dl.nssurge.com/snell/snell-server-v4.0.1-linux-aarch64.zip
```
- **2. 解压 Snell Server 到指定目录**
```
unzip snell-server-v4.0.1-linux-amd64.zip -d /usr/local/bin/ && rm ~/snell-server-v4.0.1-linux-amd64.zip
```
- **3. 赋予服务器权限**
```
chmod +x /usr/local/bin/snell-server
```
- **4. 创建配置文件**
```
mkdir /etc/snell && vim /etc/snell/snell-server.conf
```
**写入下面内容**
```bash
[snell-server]
listen = 0.0.0.0:12321
psk = a1T48yGmETVZytQGBoec
ipv6 = false
```
- **5. 配置systemctl 文件**
```
vim /etc/systemd/system/snell-server.service
```
**写入下面内容**
```bash
[Unit]
Description=Snell Proxy Service
After=network.target

[Service]
Type=simple
User=root
Group=nogroup
LimitNOFILE=32768
ExecStart=/usr/local/bin/snell-server -c /etc/snell/snell-server.conf
AmbientCapabilities=CAP_NET_BIND_SERVICE
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=snell-server

[Install]
WantedBy=multi-user.target
```
- **6. 开启 snell 服务**
```
systemctl enable --now snell-server
```
- **7. 查看 Snell 运行状态**
```
systemctl status snell-server
```

<deta## Shadow-Tls v3节点搭建

- **1.下载Shadow-Tls**

X86_AMD64
```
wget https://github.com/ihciah/shadow-tls/releases/download/v0.2.23/shadow-tls-x86_64-unknown-linux-musl -O /usr/local/bin/shadow-tls
```

ARM
```
wget https://github.com/ihciah/shadow-tls/releases/download/v0.2.23/shadow-tls-aarch64-unknown-linux-musl -O /usr/local/bin/shadow-tls
```
- **2.赋予程序执行权限**
```
chmod +x /usr/local/bin/shadow-tls
```
- **3.创建service文件**
```
vim /etc/systemd/system/shadow-tls.service
```
写入以下内容
```
[Unit]
Description=Shadow-TLS Server Service
Documentation=man:sstls-server
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
ExecStart=/usr/local/bin/shadow-tls --fastopen --v3 server --listen [::]:8443 --server 127.0.0.1:12321 --tls gateway.icloud.com  --password JsJeWtjiUyVgh0ooqQ
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=shadow-tls
Environment=MONOIO_FORCE_LEGACY_DRIVER=1

[Install]
WantedBy=multi-user.target
```
- **4.重载&启动服务**
```
systemctl daemon-reload && systemctl enable --now shadow-tls.service
```
- 5.**查看服务状态**
```
systemctl status shadow-tls.service
```


### Surge节点配置写法
```
Snell+TLS = snell, vps的ip或域名, 8443, psk=GLk1ff4wuQNCDSqr97WwsHwe8KBjy3S, version=4, shadow-tls-password=JsJeWtjiUyVgh0ooqQ, shadow-tls-sni=gateway.icloud.com, shadow-tls-version=3

```













