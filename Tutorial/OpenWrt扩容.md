安装前直接扩容镜像：

扩容镜像需要linux系统：可以是openwrt、debian、ubuntu等

openwrt中执行确认是否具备所需依赖：

```
which gzip
which dd
which parted
```

第一步：把镜像上传至openwrt根目录（上传位置随意）

第二步：按顺序执行以下命令(命令中的镜像名字根据自己实际填写)

```
# 切换到根目录
cd /
 
# 解压缩镜像文件
gzip -kd ImmortalWrt.img.gz
 
# 解压成功后删除压缩包，方便后面选择文件
 
# 扩展镜像文件的大小 (count=500 表示增加 500MB 的空间)
dd if=/dev/zero bs=1M count=500 >> ImmortalWrt.img
 
# 使用分区工具操作镜像
parted ImmortalWrt.img
 
# 查看分区情况
print
 
# 调整分区大小 (将第 2 个分区扩展至镜像文件的 100%)
resizepart 2 100%
 
# 查看是否扩展成功
print
 
# 退出分区工具
quit
```
EFI固件在执行扩容过程中会弹出额外选项：

选择ok
选择Fix
