# raspberry pi 2 wifi configeration.
##树莓派2，realtek网卡无线上网设置。
![](imgs/20170619-203008.png)

### 1.安装前准备：
```
sudo apt-get install wireless-tools wpasupplicant
```
### 2.通过`dmesg | grep usb`查看网卡生产厂家信息。
```
$ dmesg | grep usb
...
usb 1-1.3: Manufacturer: Realtek
...
```
### 3.搜索驱动：
```
$ apt-cache search realtek
firmware-realtek - Binary firmware for Realtek wired and wireless network adapters
```
### 4.使用 apt-get 安装驱动：
```
$ sudo apt-get install firmware-realtek
```
### 5.如果不能通过 apt-get 安装，可以下载[deb文件](http://mirrors.aliyun.com/raspbian/raspbian/pool/non-free/f/firmware-nonfree/)安装：`firmware-realtek......deb`
```
$ sudo dpkg -i /{PATH}/firmware-realtek_xxxxxx_all.deb
```
### 6.配置无线网络：
```
$ sudo vim /etc/network/interfaces
```
改为以下内容：
```
auto lo

iface lo inet loopback
iface eth0 inet dhcp

auto wlan0
#allow-hotplug wlan0
#iface wlan0 inet manual
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
#wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
#原文中 wpa-roam 改为 wpa-conf !
face default inet dhcp
```
### 7.创建 wpa_supplicant.conf 文件：
```
$ sudo vim /etc/wpa_supplicant/wpa_suplicant.conf
```
写入内容：
```
network={
  ssid="NETWORK_NAME"
  #key_mgmt=NONE
  key_mgmt=WPA-PSK
  psk="PASSCODE"
}
```
### 8.使用`sudo ifdown wlan0`和`sudo ifup wlan0`重新启动无线网络 。


#reference:
* http://www.jianshu.com/p/b42e8d3df449
* http://www.cnblogs.com/imapla/p/5532993.html
* http://mirrors.aliyun.com/raspbian/raspbian/pool/non-free/f/firmware-nonfree/