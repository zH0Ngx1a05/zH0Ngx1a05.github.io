自己用的是xiaomi r3,刷了uboot后，安装x-wrt9.0的老版本（不带网页安装插件的最新版本，机子太烂了，带网页装插件的太耗资源，自己用putty打命令靠谱点）
这里要注意，小白很多会没法继续，因为涉及到升级kernal和修改系统hush值（所谓的指纹）整体编译太烦了。请拿出以前打游戏金手指的作分，深度学习后继续。
然后把下载的passwall2软件包用winscp仍进内存中（/tmp文件夹）
执行opkg install *.ipk --force-depend
遇到问题再解决问题就好了

Passwall2就用2个内核，一个xray,一个 hysteria,X-ray內核要去openwrt镜像站拉取
Snapshots/packages/mipsel_24kc/packages/
但是越是更新，系统占用越大，我现在都是更新后仍到内存里运行，稍微好一点，但是麻烦就是断电或者卡重启后都要重新复制过去一遍
Hysteria内核占用少些，但是魔法大环境的问题，也觉得越来越卡，更新到直接在github上下载linux-mipsle-sf文件，删除后缀替换就好了，我一般先仍进内存，确认好用，再替换内核机的

