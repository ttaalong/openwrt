### 刷机方法（Kirkwood 88F6281 任子行net110专用固件）
---

首先，把电脑有线网卡的IP地址更改为：192.168.1.2，子网掩码：255.255.255.0，网关：192.168.1.1
用网线连接net110的LAN口和电脑
串口线连接net110的console口和电脑的USB口，设备管理器看是哪个串口，打开putty或xshell，建立一个SERIAL连接，设置好对应的串口和波特率（波特率115200）。
然后运行tftpd软件（如果是64位电脑，则运行tftpd64）

任子行插电开机，观察putty或xshell的连接，TTL按9后中断uboot，进入uboot命令行模式

##### 一.环境设置（可以先printenv看看是否一致，如果一致这一步可省略）
> setenv mainlineLinux yes <br/>
> setenv bootargs_root 'root=/dev/mtdblock2 rootfstype=squashfs' <br/>
> setenv bootcmd 'nand read.e 0x2000000 0x100000 0x400000; setenv bootargs $(console) $(bootargs_root);bootm 0x2000000' <br/>
> setenv bootargs 'console=ttyS0,115200 root=/dev/mtdblock2 ro' <br/>
> setenv arcNumber 1682 <br/>
> setenv ipaddr 192.168.1.1 <br/>
> setenv serverip 192.168.1.2 <br/>
> saveenv <br/>

##### 二.烧录uboot
> tftp 0x2000000 lce-kirkwood-butong_butongwifi-uImage> <br/>
> nand erase 0x00100000 0x00400000> <br/>
> nand write 0x2000000 0x00100000 0x00400000 <br/>

##### 三.烧录系统
> tftp 0x2000000 lce-kirkwood-butong_butongwifi-squashfs-factory.bin <br/>
> nand erase 0x00500000 0x03000000（如果遇到烧录完成reset启动时不能进入系统提示ubi0 error scan_peb bad image sequence number之类的错误时，请在此处：nand erase 0x00500000 0x7a60000） <br/>
> nand write 0x2000000 0x00500000 0x03000000 <br/>
> nand erase 0x3500000 0x06b00000 <br/>
> reset <br/>

##### 四.完成
> 路由器会自动重启，重启完成后进入系统，然后就可以体验了，记得把电脑网络设置的IP地址从192.168.1.2改成自动获取。 <br/>

##### 五.作者
> https://github.com/aboutboy/openwrt <br/>
