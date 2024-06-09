###[1.先安装NGINX_官方安装教程](https://nginx.org/en/linux_packages.html#Debian)














<details><summary>Trojan-Go节点搭建</summary>
<p>

# [trojan-go版本查看](https://github.com/p4gefau1t/trojan-go) 

- **安装 Trojan-GO**

```
wget https://github.com/gfw-report/trojan-go/releases/download/v0.10.10/trojan-go-linux-amd64.zip && unzip trojan-go-linux-amd64.zip -d ./trojan-go && mv trojan-go/trojan-go /usr/local/bin && chmod +x /usr/local/bin/trojan-go && rm -rf trojan-go  trojan-go-linux-amd64.zip
```
    
- **创建配置文件**

```
mkdir /etc/trojan-go && vim /etc/trojan-go/config.json
```
写入以下内容
```
{
    "run_type": "server",
    "local_addr": "0.0.0.0",
    "local_port": 443,
    "remote_addr": "127.0.0.1",
    "remote_port": 80,
    "password": [
        "xHvse7n9wLy#aBB$"
    ],
    "ssl": {
        "cert": "/etc/nginx/ssl/fullchain.pem",
        "key": "/etc/nginx/ssl/private.key",
        "sni": "domain.com",
        "fallback_prt": 1234
    }
}
```
    
- **创建 service 文件**

```
vim /etc/systemd/system/trojan-go.service
```
写入以下内容
```
[Unit]
Description=Trojan-Go - An unidentifiable mechanism that helps you bypass GFW
Documentation=https://p4gefau1t.github.io/trojan-go/
After=network.target nss-lookup.target

[Service]
User=root
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
NoNewPrivileges=true
ExecStart=/usr/local/bin/trojan-go -config /etc/trojan-go/config.json
Restart=on-failure
RestartSec=10s
LimitNOFILE=infinity

[Install]
WantedBy=multi-user.target
```
    
- **开启 Trojan-Go**

```
systemctl daemon-reload && systemctl enable --now trojan-go
```    
- **查看运行状态**

```
systemctl status trojan-go
```
- **查看日志**

```
journalctl -u trojan-go -o cat -e
```
- **实时日志**

```
journalctl -u trojan-go -o cat -f
```
- **卸载 Trojan-Go**

```
systemctl disable --now trojan-go && rm -rf /usr/local/bin/trojan-go /usr/local/etc/trojan-go /etc/systemd/system/trojan-go.service
```
<p>
</details>
