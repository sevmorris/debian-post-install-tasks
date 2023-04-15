
### Debian Post-Install Tasks & Miscellany
<br>

```
sudo apt update
sudo apt upgrade
```
<br>

<details>
  <summary>Install some things</summary>
<br>

```
sudo apt -y install curl openssh-server ii git figlet tldr neofetch deborphan aptitude htop
```
<br>

</details>

<details>
  <summary>Set up ssh</summary>

<br>
Enable and start sshd at boot time:
<br>

```
sudo systemctl enable ssh.service
```
<br>
Confirm sshd is enabled at boot time:
<br>

```
sudo systemctl is-enabled ssh.service
```
<br>
Check server status:
<br>

```
sudo service ssh status
```
<br>
Start sshd:
<br>

```
sudo systemctl start ssh.service
```
<br>
Restart the server:
<br>

```
sudo systemctl restart ssh.service
```
<br>
Show ip address:
<br>

```
ip a | grep "inet "﻿
```

</details>
<br>

<details>
  <summary>Install Oh My Bash</summary>
<br>

```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```

</details>

<details>
  <summary>Edit .bashrc</summary>
<br>

Change the theme to `Zork`

Add the following alias near the bottom:

```
alias update='sudo apt update && sudo apt -o Dpkg::Options::="--force-confdef" dist-upgrade -y && sudo apt autoremove -y && if sudo test -f /var/run/reboot-required; then read -p "A reboot is required to finish installing updates. Press [ENTER] to reboot now, or [CTRL+C] to cancel and reboot later." && sudo reboot; else echo "A reboot is not required. Exiting..."; fi'
```
<br>

Add the following near the bottom, replacing \<TEXT> with whatever you would like FIGlet to display

```
echo "$(tput bold)$(tput setaf 3)"
figlet <TEXT>
```
<br>

Add `neofetch` at the bottom
<br>

Reload `.bashrc`:

`source .bashrc`
<br>

</details>
<br>

<details>
  <summary>Boot to console</summary>
<br>

Backup the configuration file:

```
sudo cp -n /etc/default/grub /etc/default/grub.backup
```
<br>

Edit the configuration file:

```
sudo nano /etc/default/grub
```

Comment out: `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`

Change GRUB\_CMDLINE\_LINUX "" to:** `GRUB_CMDLINE_LINUX="text"`

Uncomment: `GRUB_TERMINAL="console"`

Save the file and apply changes:

```
sudo update-grub
```
<br>

And finally:

```
sudo systemctl set-default multi-user.target
```
<br>

</details>
<br>

<details>
  <summary>Add root password</summary>
<br>

Switch to root and add a password:

```
sudo -i
passwd
```
<br>

To switch to the root shell

 `su -`
<br>

</details>
<br>

<details>
  <summary>Run Deborphan</summary>
<br>

Deborphan finds "orphaned" packages on your system. It determines which packages have no other packages depending on their installation and shows you a list of these packages. It is most useful when finding libraries, but it can be used on packages in all sections.
<br>

Start out with a dry run:

```
deborphan --guess-all
```
<br>

Remove unnecessary data packages:

```
sudo deborphan --guess-data | xargs sudo aptitude -y purge
```
<br>

Delete unnecessary libraries:

```
sudo deborphan | xargs sudo apt-get -y remove --purge
```
<br>

</details>
<br>

### Remove x11 and everything that uses it, including all configuration
When I don't need a GUI...
<br>

```
sudo apt-get purge libx11.* libqt.*
```

```
sudo apt autoremove
```
<br>

### Remove legacy services

```
sudo apt-get --purge remove xinetd nis tftpd tftpd-hpa telnetd rsh-server rsh-redone-server
```
<br>

---
