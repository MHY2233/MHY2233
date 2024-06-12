# OpenWrt 22.03 安装 Tailscale 方法：

> ## 第一步 安装Tailscale
```
opkg update
opkg install tailscale
opkg install iptables-nft
```
> ## 第二步 启动Tailscale，并关联账户
```
service tailscale restart
tailscale up --netfilter-mode=off
```
在OpenWrt→状态→系统日志中，找到Tailscale关联账户的链接，将设备添加至自己的Tailscale账户。

第三步 设置Tailscale自启动

在OpenWrt→系统→启动项→本地启动脚本中，将原有的Tailscale命令删除（若有），并添加一行：

tailscale up --netfilter-mode=off
第四步 网络及防火墙设置

1.创建一个新接口，路径：OpenWrt→网络→接口→添加新接口

名称：tailscale
协议：不配置协议
设备：tailscale0
2.创建一个新的防火墙“区域”，路径：OpenWrt→网络→防火墙→区域→添加

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

 

至此，就完成了在OpenWrt 22.03及更高版本安装Tailscale，并完成了基础配置。