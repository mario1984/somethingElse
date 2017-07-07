# How VLC get video stream from RPI 2gen & 3gen via ethernet. | 树莓派2代和3代使用以太网传输cam视频流方法
---
## basic info

|   Raspbian Jessie  | VNC       | SSH    | VLC stream |
|------------------- |:---------:|:------:|:----------:|
|RPI 3 Jessie LITE   | √         | ×      | not tested |
|RPI 3 Jessie        | √         | ×      | √          |
|RPI 2 Jessie        | √         | ×      | √          |

|ubuntu 14.04  | VNC       | SSH    | VLC stream |
|------------- |:---------:|:------:|:----------:|
|RPI 2         |not tested | ×      | √          |

---
## 1. raspberry 2 (Raspbian Jessie)
1. Add ip address `eg:192.168.2.2` to /boot/cmdline.txt. Change the PC eth0     address to `eg:192.168.2.3`.
2. Connect RPI and PC with ethernet and startup RPI.
3. ssh into RPI and enable camera with `sudo raspi-config` then reboot.
4. install VNC with`sudo apt-get install tightvncserver`and set password with`vncpasswd`,view-only nevermind usually not useful :)
5. Create a file in startup folder `sudo vim /etc/init.d/tightvncserver`
```
#!/bin/sh
### BEGIN INIT INFO
# Provides:          tightvncserver
# Required-Start:    $local_fs
# Required-Stop:     $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start/stop tightvncserver
### END INIT INFO
 
# More details see:
# http://www.penguintutor.com/linux/tightvnc
 
### Customize this entry
# Set the USER variable to the name of the user to start tightvncserver under
export USER='pi'
### End customization required
 
eval cd ~$USER
 
case "$1" in
  start)
    # 启动命令行。此处自定义分辨率、控制台号码或其它参数。
    su $USER -c '/usr/bin/tightvncserver -depth 16 -geometry 800x600 :1'
    echo "Starting TightVNC server for $USER "
    ;;
  stop)
    # 终止命令行。此处控制台号码与启动一致。
    su $USER -c '/usr/bin/tightvncserver -kill :1'
    echo "Tightvncserver stopped"
    ;;
  *)
    echo "Usage: /etc/init.d/tightvncserver {start|stop}"
    exit 1
    ;;
esac
exit 0
```
```
sudo chmod 755 /etc/init.d/tightvncserver
sudo update-rc.d tightvncserver defaults
```
*continue...*<br>
6. restart<br>
7. install VLC
> There are several options you can choose between. At my work we are using VLC to stream video captured by Raspberry Pi Camera from our server-rooms to the office. One downside of this is that there are about 5 seconds delay and I haven't found a solution to this. The following is our setup:

    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install vlc
```
sudo vim myscript.sh
```
myscript.sh:
    
```
raspivid -o - -t 0 -hf -w 640 -h 360 -fps 25 | cvlc -vvv stream:///dev/stdin --sout '#rtp{sdp=rtsp://:8554}' :demux=h264
```
then:
```
sudo vim myscript2.sh
```
myscript2.sh:
```
#!/bin/bash
/path/to/myscript.sh
```
next:
```
sudo chmod +x myscript.sh
sudo chmod +x myscript2.sh
crontab -e
@reboot /path/to/myscript2.sh
```
> To watch the videostream, open VLC on a computer on the same network as the raspberry pi you are using for streaming. Press Media -> Open Networkstream and paste the following in the field:
```
rtsp://192.168.2.2:8554/
```

---

## 2. raspberry 3 (Raspbian Jessie)
*1~6 are the same with RPI 2.*<br>
7. install VLC
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install vlc
```
```
mkdir -p ~/streamWorks/runvlc.sh
``` 
runvlc.sh:
```
#!/bin/sh
raspivid -o - -t 0 -w 640 -h 360 -fps 25|cvlc stream:///dev/stdin --sout '#standard{access=http,mux=ts,dst=:8090}' :demux=h264
```
    sudo chmod +x runvlc.sh
*continue...*<br>
8. `http://192.168.2.2:8090/`on the PC VLC

## 3. raspberry 2 (Ubuntu 14.04)
1. add a line "start_x=1" at the bottom of the file `/boot/firmware/config.txt`, then reboot.
2. install vlc by `sudo apt-get install vlc`.
3. touch a script.sh file and add `raspivid -o - -t 0 -hf -w 640 -h 360 -fps 25 | cvlc -vvv stream:///dev/stdin --sout '#rtp{sdp=rtsp://:8554}' :demux=h264`, chmod +x to the script.sh file.
4. run the script to see the stream from rpi screen.
5. use `rtsp://192.168.1.115:8554/` to see VLC stream from other devices.

---
## references:
* [Camera Module - Raspberry Pi Documentation](https://www.raspberrypi.org/documentation/usage/camera/)
* [Camera board comparisons: Pi NoIR v1 vs Pi NoIR v2 - Raspberry Pi](https://www.raspberrypi.org/blog/camera-board-comparisons-pi-noir-v1-vs-pi-noir-v2/)
* [Raspberry Pi • View topic - Can't force wlan0: to use static IP](https://www.raspberrypi.org/forums/viewtopic.php?f=91&t=22660)
* [Use your desktop or laptop screen and keyboard with your Pi - Raspberry Pi](https://www.raspberrypi.org/blog/use-your-desktop-or-laptop-screen-and-keyboard-with-your-pi/)
* [Raspberry Pi Home CCTV System - 3](http://www.instructables.com/id/Raspberry-Pi-Home-CCTV-System/step3/Step-3-Configuring-motion-and-starting-the-softwar/)
* [raspbian - How to stream video from raspberry Pi camera and watch it live - Raspberry Pi Stack Exchange](http://raspberrypi.stackexchange.com/questions/23182/how-to-stream-video-from-raspberry-pi-camera-and-watch-it-live)
* [Raspberry Pi Camera Module - Raspberry Pi Documentation](https://www.raspberrypi.org/documentation/raspbian/applications/camera.md)
* [玩转树莓派－RaspBerry，安装VNC远程桌面服务 - openthings - 开源中国社区](https://my.oschina.net/u/2306127/blog/388798)
* [Raspberry PI (Ver 2.0) + PI Cam as a stable RTSP Server - Synology Forum](https://forum.synology.com/enu/viewtopic.php?t=98870)
* [networking - How can I connect my Pi directly to my PC and share the internet connection? - Raspberry Pi Stack Exchange](http://raspberrypi.stackexchange.com/questions/11684/how-can-i-connect-my-pi-directly-to-my-pc-and-share-the-internet-connection)
* [linux - Capture RTSP stream from IP Camera and store - Super User](http://superuser.com/questions/766437/capture-rtsp-stream-from-ip-camera-and-store)
* https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=113019
