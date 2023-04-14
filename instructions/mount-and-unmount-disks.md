List attached disks.

`sudo fdisk -l`
<br><br>

Make a mnt directory (or whatever name you'd like).

`mkdir /mnt`
<br><br>

Mount a disk (x is partition number).

`sudo mount /dev/sdb(x) /mnt`
<br><br>

Unmount disk.

`umount /dev/sdb(x)`
