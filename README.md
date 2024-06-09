
<details><summary>Snell节点搭建</summary>
<p>

# [Snell 官方手册](https://manual.nssurge.com/others/snell.html)

- **1. 下载 Snell Server 安装包**
```
wget https://dl.nssurge.com/snell/snell-server-v4.0.1-linux-amd64.zip
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
</p>
</details>


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

<details><summary>Tuic节点搭建</summary>
<p>

# [查看最新发行版](https://github.com/EAimTY/tuic/releases)

- **下载程序**
```
curl -Lo tuic https://github.com/EAimTY/tuic/releases/latest/download/tuic-server-1.0.0-x86_64-unknown-linux-gnu && chmod +x tuic && mv -f tuic /usr/local/bin/
```
- **创建配置文件**
```
mkdir /usr/local/etc/tuic && vim /usr/local/etc/tuic/config.json
```
写入以下内容
```
{
    "server": "[::]:443",
    "users": {
"018c9516-8a12-7a17-95ac-4945db4fc9eb": "Gh6ZFxX6R8dyyJ$M"
    },
    "certificate": "/etc/nginx/ssl/fullchain.pem",
    "private_key": "/etc/nginx/ssl/private.key",
    "congestion_control": "bbr",
    "alpn": ["h3"],
    "udp_relay_ipv6": false,
    "zero_rtt_handshake": true,
    "dual_stack": true,
    "auth_timeout": "3s",
    "task_negotiation_timeout": "3s",
    "max_idle_time": "10s",
    "max_external_packet_size": 1500,
    "send_window": 16777216,
    "receive_window": 8388608,
    "gc_interval": "3s",
    "gc_lifetime": "15s",
    "log_level": "info"
}
```
- **创建service文件**
```
vim /etc/systemd/system/tuic.service
```
写入以下内容
```
[Unit]
Description=tuic service
Documentation=https://github.com/EAimTY/tuic
After=network.target nss-lookup.target

[Service]
User=root
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE CAP_NET_RAW
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE CAP_NET_RAW
ExecStart=/usr/local/bin/tuic --config /usr/local/etc/tuic/config.json
Restart=on-failure
RestartSec=10
LimitNOFILE=infinity

[Install]
WantedBy=multi-user.target
```
- **启动tuic**
```
systemctl daemon-reload && systemctl enable --now tuic
```
- **重启tuic**
```
systemctl restart tuic
```
- **查看tuic状态**
```
systemctl status tuic
```
- **查看日志**
```
journalctl -u tuic -o cat -e
```
- **实时日志**
```
journalctl -u trojan-go -o cat -f
```
</p>
</details>





