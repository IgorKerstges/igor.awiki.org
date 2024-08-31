# Mount the attached block volume

Configure server instance and then attach the block device. With this state, we need to take care of following steps:

- mount the blockvolume in fstab with sudo-user
- map the user home directories to the mounted loactions
- configure SE-Linux to work with the new home directories


### mount the blockvolume in fstab with sudo-user

`sudo nano /etc/fstab`

Add following line:

`UUID=0211c06a-afee-4543-98ef-d32f10112973 /mnt       ext4  defaults  0  2`

### map the user home directories to the mounted loactions

..

### configure SE-Linux to work with the new home directories

..
