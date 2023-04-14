To make changes permanent (will not be overwritten by updates):

`sudo nano /etc/motd.tail`
<br><br>

To change (or remove) the other messages already displayed:

`cd /etc/update-motd.d`
<br><br>

This folder contains numbered shell scripts which are run in order. You can
modify or remove them, or add your own.
