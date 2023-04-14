## Scan using badblocks

List attached disks.

`sudo fdisk -l`
<br><br>

Run badblocks, replacing (x) with partition number or remove for whole disk.

`sudo badblocks -v /dev/sda(x) > badsectors.txt`
<br><br>

## If bad sectors found

For ext2/ext3/ext4 file-systems.

`sudo e2fsck -l badsectors.txt /dev/sda(x)`
<br><br>

for other file-systems.

`sudo fsck -l badsectors.txt /dev/sda(x)`
<br><br>

## Scan using Smartmontools

Install smartmontools.

`sudo apt install smartmontools`
<br><br>

Check drive's SMART results.

`sudo smartctl -Ha /dev/sda(x)`
<br><br>

Check SMART status. Look for "Reallocated_Sector_Ct" attribute.

`sudo smartctl -a /dev/sd(x)`
