

<details><summary>CloudDrive-2服务部署部署</summary>
<p>
  
# [查看最新发行版](https://github.com/cloud-fs/cloud-fs.github.io/releases) 
<details><summary>VPS服务器部署</summary>
<p>

- **1.安装程序**

```
wget https://github.com/cloud-fs/cloud-fs.github.io/releases/download/v0.7.3/clouddrive-2-linux-aarch64-0.7.3.tgz && tar -zxvf clouddrive-2-linux-aarch64-0.6.14.tgz && mv clouddrive-2-linux-aarch64-0.6.14 /usr/local/bin/clouddrive-2 && rm ~/clouddrive-2-linux-aarch64-0.7.3.tgz
```

- **2.配置开机启动**
```
vim /etc/systemd/system/clouddrive.service
```
写入以下内容
```
[Unit]
Description=clouddrive service
Wants=network.target
After=network.target network.service

[Service]
Type=simple
WorkingDirectory=/usr/local/bin/clouddrive-2
ExecStart=/usr/local/bin/clouddrive-2/clouddrive server
KillMode=process

[Install]
WantedBy=multi-user.target
```
- **3.启动clouddrive**

```
systemctl enable --now clouddrive
```
- **4.查看运行状态**

```
systemctl status clouddrive
```
</p>
</details>

<details><summary>OPENWRT软路由部署</summary>
<p>

- **1.下载安装程序**
```
wget https://linkroad.io/proxy-tools/package/-/raw/main/clouddrive-2-linux-aarch64-0.7.3.tgz && tar -zxvf clouddrive-2-linux-aarch64-0.7.3.tgz && mv clouddrive-2-linux-aarch64-0.7.3 /usr/bin && rm clouddrive-2-linux-aarch64-0.7.3.tgz 
```
- **2.配置开机启动**
```
vim /etc/init.d/clouddrive-2_service
```
输入以下内容
```
#!/bin/sh /etc/rc.common
START=15
USE_PROCD=1

start_service() {
  procd_open_instance "clouddrive-2_service"
  procd_set_param command "/usr/bin/clouddrive-2/clouddrive"
  procd_close_instance
}
```


- **3.赋予可执行权限并启动**
```
chmod +x /etc/init.d/clouddrive-2_service && /etc/init.d/clouddrive-2_service start
```
- **4.设置开机启动**
```
/etc/init.d/my_service enable
```
- **4.查看是否生效
```
ls -l /etc/rc.d
```
</p>
</details>

</p>
</details>

<details><summary>ShadowSocks节点搭建</summary>
<p>

# [查看最新发行版](https://github.com/shadowsocks/shadowsocks-rust/releases)

- **安装程序**
```
wget https://github.com/shadowsocks/shadowsocks-rust/releases/download/v1.18.2/shadowsocks-v1.18.2.x86_64-unknown-linux-gnu.tar.xz && tar -xf shadowsocks-v1.18.2.x86_64-unknown-linux-gnu.tar.xz -C /usr/local/bin/ && chmod +x /usr/local/bin/ssserver && rm shadowsocks-v1.18.2.x86_64-unknown-linux-gnu.tar.xz
```
- **创建配置文件**
```
mkdir /etc/shadowsocks/ && vim /etc/shadowsocks/config.json
```
写入以下内容
```
{
    "server": "0.0.0.0",
    "server_port": 4143,
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "mode":"tcp_and_udp",
    "password": "eNlo@1g@%6gq7H9Y",
    "timeout": 300,
    "method": "chacha20-ietf-poly1305"
}
```
- **创建 service 文件**
```
vim /etc/systemd/system/shadowsocks.service .service 
```
写入以下内容
```
[Unit]
Description=Shadowsocks Server
After=network.target

[Service]
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks/config.json

Restart=on-abort

[Install]
WantedBy=multi-user.target
```
- **启动 shadowsocks**
```
systemctl daemon-reload && systemctl enable --now shadowsocks
```
- **重启 shadowsocks**
```
systemctl restart shadowsocks 
```
- **查看运行状态**
```
systemctl status shadowsocks 
```
- **查看日志**
```
journalctl -u shadowsocks -o cat -e
```
- **查看实时日志**
```
journalctl -u shadowsocks -o cat -f
```
- **卸载 shadowsocks**
```
systemctl disable --now shadowsocks && rm -rf /usr/local/bin/ssserver /etc/shadowsocks /etc/systemd/system/shadowsocks.service
```
</p>
</details>



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
