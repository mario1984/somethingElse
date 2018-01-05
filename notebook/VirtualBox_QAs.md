### How to create a shared folder? (Ubuntu 16.04 running in VirtualBox on Windows 10)
* Create share folder in the settings of VB and don't select `auto mount`.
* Open a terminal in ubuntu and type `sudo mount -t vboxsf <shared_folder_name> <shared_folder_in_ubuntu>`.
eg. 
```
sudo mount -t vboxsf vb_share ~/vb_share/
```
* Done!
