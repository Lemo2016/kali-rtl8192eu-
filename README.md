# kali-rtl8192eu
VBox安装kali，无线网卡tplinkwdn5200

原文参考：

https://blog.csdn.net/GX_1_11_real/article/details/90597457

https://www.jianshu.com/p/32e19a365534

https://github.com/Mange/rtl8192eu-linux-driver

平台

MAC 10.15

VirtualBox

Kali Linux VirtualBox 64-Bit

tplink wdn5200 （MTK MT7610U, USB ID = 0bda:c811, Driver = rtl8192eu）


wdn5200drive: https://github.com/Mange/rtl8192eu-linux-driver

Kali.dvi: https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/



VBox中安装kali

直接用官网的Kali Linux VirtualBox 64-Bit导入

VBox中端口设置-选择USB3.0。为了识别网卡

kali 2020.1和2019.3，在VBox安装都遇到了软件安装错误，重复很多遍还失败，不如直接用装好的


kali中文设置 

https://blog.csdn.net/GX_1_11_real/article/details/90597457

下载字体

$ apt-get install ttf-wqy-microhei ttf-wqy-zenhei xfonts-wqy locales

配置中文界面

空格选中en_US.UTF-8、zh_CN.GBK、zh_CN.UTF-8，先按方向键右，:zh_CN.UTF-8. 回车OK

$ dpkg-reconfigure locales

重启

$ systemctl reboot -i;


网卡设置（跟紧大神脚步）

https://github.com/Mange/rtl8192eu-linux-driver

$ git clone https://github.com/Mange/rtl8192eu-linux-driver;

$ cd rtl8192eu-linux-driver;

$ sudo dkms add .;

$ sudo dkms install rtl8192eu/1.0;

$ echo "blacklist rtl8xxxu" | sudo tee /etc/modprobe.d/rtl8xxxu.conf;

$ echo -e "8192eu\n\nloop" | sudo tee /etc/modules;

$ echo "options 8192eu rtw_power_mgnt=0 rtw_enusbss=0" | sudo tee /etc/modprobe.d/8192eu.conf;

$ sudo update-grub; sudo update-initramfs -u;

$ systemctl reboot -i;

$ sudo lshw -c network;


如果有意外出现

https://unix.stackexchange.com/questions/358075/realtek-rlt8812au-cant-start-monitor-mode

"Newly created monitor mode interface wlan0mon is *NOT* in monitor

 mode. Removing non-monitor wlan0mon interface...
 
 WARNING: unable to start monitor mode, please run "airmon-ng check kill""
 
#wlan0对应无线网卡名称替换

$ ip link set wlan0 down

$ iw dev wlan0 set type monitor

$ ip link set wlan0 up

查看网卡工作

$ airodump-ng wlan0


欢迎开始你的kali之旅

USB ID和驱动的部分对应关系

https://blog.csdn.net/opipa/article/details/51919847

查看其它网卡驱动和芯片组

https://www.drvsky.com/sort/300_1.htm




