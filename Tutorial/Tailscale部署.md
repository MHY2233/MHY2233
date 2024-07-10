## Tailscale for OpenWrt安装：

### 第一步 安装Tailscale
```
opkg update

opkg install tailscale

opkg install iptables-nft
```
### 第二步 启动Tailscale，并关联账户
```
service tailscale restart && tailscale up
```
> 复制屏幕连接到浏览器打开并登录自己的tailscale账户。

### 第三步 设置Tailscale自启动

> 在OpenWrt→系统→启动项→本地启动脚本中，将原有的Tailscale命令删除（若有），并添加一行：
```
tailscale up --netfilter-mode=off
```
### 第四步 网络及防火墙设置

> 1.创建一个新接口，路径：OpenWrt→网络→接口→添加新接口
```
名称：tailscale
协议：不配置协议
设备：tailscale0
```
> 2.创建一个新的防火墙“区域”，路径：OpenWrt→网络→防火墙→区域→添加
```
名称：tailscale
入站数据：接受（默认状态）
出站数据：接受（默认状态）
转发：接受
IP动态伪装：开启
MSS钳制：开启
涵盖的网络：tailscale
允许转发到目标区域：选择你的LAN（和/或其他内部区域，或者如果你计划将此设备用作出口节点，则选择WAN）
允许来自源区域的转发：选择你的LAN（和/或其他内部区域，如果你不想将LAN流量路由到其他tailscale主机，则留空）
点击“保存并应用”。
```

### 第五步 开启子网路由和出口节点

> 1.开启ip转发

如果您的Linux系统有一个/etc/sysctl.d目录，请使用：
```
echo 'net.ipv4.ip_forward = 1' |  tee -a /etc/sysctl.d/99-tailscale.conf
echo 'net.ipv6.conf.all.forwarding = 1' |  tee -a /etc/sysctl.d/99-tailscale.conf
sysctl -p /etc/sysctl.d/99-tailscale.conf
```
否则，请使用：
```
echo 'net.ipv4.ip_forward = 1' |  tee -a /etc/sysctl.conf
echo 'net.ipv6.conf.all.forwarding = 1' | tee -a /etc/sysctl.conf
sysctl -p /etc/sysctl.conf
```

> 2.将设备宣传为出口节点和开启子网路由
- **openwrt输入以下命令**
```
tailscale set --advertise-routes=192.168.10.0/24 --advertise-exit-node --accept-dns=false
```

重启tailscale至此，就完成安装Tailscale，并完成了基础配置。