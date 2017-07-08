# install ubuntu core 16.04 on raspberry pi 3.
## keywords: 
- ubuntu core 16.04
- rpi 3
- macOS
## 1. create an account on [Ubuntu One](https://login.ubuntu.com).
* setup ssh keys by `key-gen` command in `~/.ssh/` of laptop terminal.
* copy all code in `~/.ssh/id_rsa.pub`, paste into the *Public SSH Key* box and click "Import SSH key" button.
## 2. burn [the img file](https://developer.ubuntu.com/core/get-started/raspberry-pi-2-3) of ubuntu core 16.04 to SD card.
* Etcher for Mac was tested.
* insert the SD card and boot with moniter, ethernet cable and keyboard connected.
* use `Tab` button to change selection. 
* type in the ubuntu one account email address and select DONE.
* to login from the macOS terminal:
```
$ ssh <SSO username>@<ip address>
```
* no password needed.
## reference:
- https://insights.ubuntu.com/2017/01/27/ros-on-arm64-with-ubuntu-core/ (turn snap to apt-get)
