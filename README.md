# PortableWiFi-Web
随身WiFi 后台管理界面    
影腾Y1 中芯微 版本

## 打包过程

## 在`full.bin` 所在目录下载 py脚本
wget https://raw.githubusercontent.com/anysoft/mifi-tools/main/squashfs_extract.py

## 执行提取脚本 得到 full_498000-74eccc.bin 之类的squashfs文件
python3 squashfs_extract.py output full.bin

## 安装squafs-tools 工具
apt install squashfs-tools

## 对 full_498000-74eccc.bin 进行解压
## 解压后文件在 squashfs-root/目录下，可以进行去控修改啦
unsquashfs full_498000-74eccc.bin

# 去控修改
## 一般删除bin 和sbin目录下的一些流控程序、脚本
## 同时编辑 etc/rc 文件开启 adb、去掉远控代码，加上nv set *** 等开机自动对nv修改的命令
## 修改 etc_ro/default/ 目录下几个配置文件的默认配置
## 修改替换 etc_ro/web web.zip 后台(并不是所有棒子、机都支持全功能后台，由于版本不一样，很多机子是不支持全功能后台的)
## 对于不支持全功能后台的机器只能修改 etc_ro/web/js/set.js 内的一些开关来开启一些隐藏菜单，或者编辑com.js修改一些逻辑

# 重新打包，得到 new.squashfs
mksquashfs squashfs-root/ new.squashfs -comp xz

# 用 new.squashfs 替换full.bin中的 rootfs分区，得到 full.bin.new的新编程器固件
python3 squashfs_extract.py input full.bin new.squashfs 0
