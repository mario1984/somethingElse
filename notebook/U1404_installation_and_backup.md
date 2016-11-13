# Ubuntu 14.04 installation and backup | Ubuntu 安装与系统镜像笔记

###步骤：
1. Ubuntu下使用Disks分区：`Ubuntu下使用Disks对磁盘分区，保留一定大小的fat32或ntfs格式分区，剩余分区删除`
2. 使用[etcher](http://www.etcher.io)制作启动盘。
3. 重启安装ubuntu，将系统安装在磁盘上删除的空间里，`/swap`分区设置为内存大小，`/` 根目录分区ext4格式大小为剩下的空间。
4. 进入ubuntu桌面后更换镜像，update，安装常用软件。
5. 配置好适合的系统之后使用[remastersys](https://github.com/mutse/remastersys)制作backup镜像iso文件。
6. 使用windows的[ultraiso](http://cn.ultraiso.net/)制作启动盘(使用ubuntu的etcher制作的无法安装，纳尼-.-||) 。

---
###ubuntu常用软件:
    sudo apt-get install aptitude
**使用aptitude安装：**<br>
>vim , zsh , git , qt-creator , flash_plugin , terminator , etcher , remastersys , 

**install oh-my-zsh:**<br>
```
$ wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

$ chsh -s /bin/zsh
```
**install vim_plugin:**<br>
```
$ wget -qO- https://raw.github.com/ma6174/vim/master/setup.sh | sh -x
```