最近，我有机会检修一台老笔记本，并重新安装了一个 Atlas OS 的 Win10 系统。这次，我决定尝试在 Windows 上部署 Immich 服务器，以前都是在 Linux 系统中完成类似的操作。结果发现，虽然在 Windows 上部署可行，但确实需要耐心和细心。不同系统的文件编码和存储位置有所差异，不能简单地用“docker-compose up -d”就能一键搞定。下面我会详细分享在 Windows 上部署 Immich 服务器的完整过程和遇到的难点。

一、安装 Docker Desktop
第一步是安装 Docker Desktop，这部分相对简单。你只需要确保有一个稳定的网络环境即可。我连接了 Zero-Trust 网络，很快就安装好了 Docker Desktop。接下来，面临的重大选择是 Docker Desktop 的虚拟机方式：WSL（Windows Subsystem for Linux）或 Hyper-V。

关于虚拟机的选择：

WSL 模式：我尝试过 WSL 虚拟机模式，但它在重启自启动方面问题较多，每次重启都需要手动启动 WSL 服务，这让使用过程显得非常繁琐。而且，WSL 会在系统底层架设一个同步于 Win10 的操作系统，两边共享文件操作，增加了部署难度。
Hyper-V 模式：最终我切换回 Hyper-V 模式。Hyper-V 更像是一个 VM 客户端，设置为开机自动启动后，可以直接运行设定好的 Docker 容器，免去了重启手动操作的麻烦。
二、部署 Immich 服务器的挑战
接下来就是 Immich 服务器的部署。这里最大的挑战在于镜像拉取，因为 Immich 服务器需要的 4 个 Docker 镜像总计约 3.9GB，这对网络环境要求很高。用 Zero-Trust 显然不够，速度慢且容易中断。于是，我尝试借助第三方镜像站下载，但过程依然充满坎坷，经常碰到下载到一半被拒绝（“401”错误）的情况，真的令人抓狂。

经过多次尝试，我终于成功拉取了所有需要的镜像。按照 Immich 官网的 docker-compose 步骤，顺利部署并启动了大部分容器，但问题随之而来。

问题：immich-postgres 容器不断重启

这个容器负责存储账户信息等重要数据。没有它，整个服务器就无法正常运行。我查看日志发现错误提示是关于数据文件存储路径的问题。根据 docker-compose 的默认配置，镜像尝试将数据存储在“./postgres”路径下，但由于 Windows 没有 Linux 系统的 root 目录，这就导致了问题。
解决方案：修改配置文件

在查阅大量资料后，我找到了解决方法：编辑 docker-compose 文件夹中的 .env 配置文件，将 DB_DATA_LOCATION=./postgres 修改为 DB_DATA_LOCATION=/mnt/cache/appdata/immich/postgres。这样就将数据存储位置改为 Windows 可识别的路径。
完成修改后，重新运行命令 docker-compose up -d，耐心等待 1 分钟，成功打开了 localhost:2283，服务器终于运行起来了！

三、总结与心得
整个部署过程并不容易，尤其是在跨系统环境下实施。每一个细小的问题都可能影响最终的结果，但这些挫折也是宝贵的经验。希望通过我的经历，能为其他人提供参考，少走弯路。

记录每一次折腾的点滴，标记每一次微小的成长。如果你也遇到类似的挑战，不要放弃，问题总有解决的办法。