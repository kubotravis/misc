LVM CHEAT sheet
---------------
##### Creating LVM with a new Hard Disk

$ fdisk /dev/sdb

:n - new
:p - primary
:1 - <its dev partition num>
<Enter> <Enter> to accept the 1st & last cylinder size

p -> to print current 

:t - to change the type
:8e - for LVM
:w - write

$ pvcreate /dev/sdb1 <pvname>

$ vgcreate <vgname> /dev/sdb1

$ lvcreate -L <desire_size> -n <lvname> <vgname>

$ mkfs -t ext3 /dev/vgname/lvname

$ mount -t ext3 /dev/vgname/lvname /mountpoint

$ df -hT

##### Exteding the existing the LVM

$ lvextend -L +3G /dev/vgname/lvname

$ resize2fs /dev/vgname/lvname


##### Reducing the exisiting LVM

$ umount /mountpoint

$ e2fsck -ff /dev/vvgname/lvname || checking for the error , should pass all 5 levels

$ resize2fs /dev/vgname/lvname 1G || resizing by  filesystem 

$ lvreduce -L -1G /dev/vgname/llvname || resizing by GB size

$ lvreduce -l -256 /dev/vgname/lvname || resizing by PE count (SIZE/4=PE count) 1024MB/4(PEsize)=256 

$ resize2fs /dev/vgname/lvname

$ mount /dev/mapper/vgname-lvname /mountpoint

$ df -hT

##### Extending the LVM with new HDD with existing vgname

Create LVM partiton by following above (with new hard disk steps)

Imagine new hard disk /dev/sdc1

$ pvcreate /dev/sdc1

$ vgextend vgname /dev/sdc1

run vgdisplay and the  free PE size of it 254 here <<Free  PE / Size       254 / 1016.00 MiB>>

$ lvextend -l 254 /dev/vgname/lvname

$ resize2fs /dev/vgname/lvname

$ df -h


Disclaimer: May or may not work. This is absolutely for my reference, use or refer it your own RISK
