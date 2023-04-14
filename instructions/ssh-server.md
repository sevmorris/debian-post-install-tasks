**Install and misc useful commands**

<br>

Enable and start sshd at boot time:

```
sudo systemctl enable ssh.service
```
<br>

Confirm sshd is enabled at boot time:

```
sudo systemctl is-enabled ssh.service
```
<br>

Check server status:

```
sudo service ssh status
```
<br>

Start sshd:

```
sudo systemctl start ssh.service
```
<br>

Restart the server:

```
sudo systemctl restart ssh.service
```
<br>

Show ip address:

```
ifconfig | grep "inet "﻿
```
