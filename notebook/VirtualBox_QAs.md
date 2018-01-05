### How to create a shared folder? (Ubuntu 16.04 running in VirtualBox on Windows 10)
* Create share folder in the settings of VB and don't select `auto mount`.
* Check out the sharedfoler list with `sudo VBoxControl sharedfolder list`.
* Open a terminal in ubuntu and type `sudo mount -t vboxsf <shared_folder_name> <shared_folder_in_ubuntu>`.
* `shared_folder_name` and `shared_folder_in_ubuntu` shoud be different or you'll get protocol error.
* eg. 
```
sudo mount -t vboxsf vb_share ~/vb_share/
```
* Done!

-------
*references*
* https://www.virtualbox.org/manual/
