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
