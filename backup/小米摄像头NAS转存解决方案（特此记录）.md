用废旧笔记本部署了台Docker的轻NAS, 装了小雅全家桶，Immich 照片源，做了Lucky版的内网穿越，最后终于把小米摄像头转存挂上了，这个Samba整了我两天

原因通过CSDN友人分享经验，终于成功，特此记录。

主要原因是新版的Samba 取消了对Smb1协议的支持

修改如下
1..vim /etc/samba/smb.conf
在[global] 中添加/修改
“min protocal = NT1”
让新版支持老的SMB1协议

2.在［share］中添加/修改
“guest ok = no”
拒绝访客模式

*这里岔开，我直接用了CasaOS中的文件管理做的共享文件夹，所以常规smb.conf文件中找不到[share],在CasaOS环境下也不能直接添加，会冲突。需要找到对应的smb.casa.conf文件去添加修改

3.然后执行重启Samba
"sudo smbd restart"

再用小米摄像头搜索NAS位置，登陆用户密码就找到共享文件夹了。

谢谢CSDN网友“qq_33130395”分享的经验

本人部署环境
Armbian_x86,运行docker,docker-compose,安装CasaOS，全部在网页端造成部署。