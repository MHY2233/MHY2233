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
