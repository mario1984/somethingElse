# rpi2 ROS network establishment and enable node upstart.
## 1. description:
* rpi2 
* ubuntu 14.04
* ros indigo desktop full
## 2. rpi2 ROS network establishment.
* follow ROS network setup [page](http://wiki.ros.org/ROS/NetworkSetup#Configuring_.2BAC8-etc.2BAC8-hosts).

## 3. chmod ttyACM* while upstart.
* if device cannot be found while started, then use following method.
* make a rules file in `/etc/udev/rules.d/` directory.  for example:
```
$ cd /etc/udev/rules.d
$ sudo touch 50-myusb.rules
$ sudo vim 50-myusb.rules
```
in the file `50-myusb.rules`:
```
KERNEL=="ttyACM[0-9]*",MODE="0666"
```
method from [askubuntu.com](https://askubuntu.com/questions/58119/changing-permissions-on-serial-port).
>Note that this will give any device connected to ttyACM socket read/write permissions. If you need only specific device to get read/write permissions you must also check `idVendor` and `idProduct`. You can find those by running `lsusb` command twice, once without your device connected and once when it is connected, then observe the additional line in the output. There you will see something like `Bus 003 Device 005: ID ffff:0005`. In this case `idVendor = ffff` and `idProduct = 0005`. Yours will be different. Than you modify the rules file to:

```
ACTION=="add", KERNEL=="ttyACM[0-9]*", ATTRS{idVendor}=="ffff", ATTRS{idProduct}=="0005", MODE="0666"
```
>Now only this device gets the permissions. Read [this](http://reactivated.net/writing_udev_rules.html) to know more about writing udev rules.

## 4. rpi2 enable node upstart.
* robot-upstart installation [page](http://docs.ros.org/indigo/api/robot_upstart/html/).
* some [arguments](http://docs.ros.org/indigo/api/robot_upstart/html/install.html) are needed while installing scripts.
* example:
```
$ rosrun robot_upstart install auto_start/launch/auto.launch \
> --interface wlan0\
> --user ubuntu\
> --rosdistro indigo\
> --master http://192.168.0.115:11311
```
## 5. rpi2 uninstall upstart scripts.
* to uninstall upstart scripts:
```
$ rosrun robot_upstart uninstall <scripts-package-name>
```
* if it doesn't work, delete following files.
```
/etc/init/upstart.conf
/etc/ros/indigo/upstart.d/.installed_files
/etc/ros/indigo/upstart.d/<scripts-name>
/usr/sbin/upstart-start
/usr/sbin/upstart-stop
```
## reference:
- http://docs.ros.org/indigo/api/robot_upstart/html/
- http://docs.ros.org/indigo/api/robot_upstart/html/install.html
- http://wiki.ros.org/ROS/NetworkSetup#Configuring_.2BAC8-etc.2BAC8-hosts
- http://wiki.ros.org/ROS/EnvironmentVariables#ROS_IP.2BAC8-ROS_HOSTNAME