
# Debian Post-Install Tasks
<br>

```
sudo apt update
sudo apt upgrade
```
<br>

### Install some stuff
<br>

```
sudo apt -y install curl openssh-server ii git figlet tldr neofetch deborphan aptitude htop
```
<br>

I sometimes set up and connect via ssh at this point:
<br><br>

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

### <a name="2"></a>Install [Oh My Bash](https://github.com/ohmybash/oh-my-bash)
<br>

```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```
<br>

<details>
  <summary>Edit .bashrc</summary>
<br>

- Change the theme to `Zork`
- Add the following, replacing \<TEXT> with whatever you would like FIGlet to display

```
echo "$(tput bold)$(tput setaf 3)"
figlet <TEXT>
```

- Add `neofetch` at the bottom
- Add the following alias near the bottom:

```
alias update='sudo apt update && sudo apt -o Dpkg::Options::="--force-confdef" dist-upgrade -y && sudo apt autoremove -y && if sudo test -f /var/run/reboot-required; then read -p "A reboot is required to finish installing updates. Press [ENTER] to reboot now, or [CTRL+C] to cancel and reboot later." && sudo reboot; else echo "A reboot is not required. Exiting..."; fi'
```
<br>

Reload `.bashrc` to see the changes immediately: `source .bashrc`
<br><br>

</details>
<br>

<details>
  <summary>Boot to console</summary>

### Boot to console
<br>

- Backup the configuration file:
<br>

```
sudo cp -n /etc/default/grub /etc/default/grub.backup
```
<br>

Edit the configuration file:
<br>

```
sudo nano /etc/default/grub
```

- Comment out: `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`

- Change GRUB\_CMDLINE\_LINUX "" to:** `GRUB_CMDLINE_LINUX="text"`

- Uncomment: `GRUB_TERMINAL="console"`

- Save the file and apply changes:

```
sudo update-grub
```
<br>

- And finally:
<br>

```
sudo systemctl set-default multi-user.target
```
<br>

</details>
<br>

### Change motd
(message of the day)
<br>

The directory `/etc/update-motd.d` contains numbered shell scripts which are run in order.
<br>
You can modify or remove them, and/or add your own.
<br>

<details>
  <summary>Deborphan</summary>

### Using [Deborphan](https://manpages.ubuntu.com/manpages/bionic/man1/deborphan.1.html)
Deborphan finds "orphaned" packages on your system. It determines which packages have no other packages depending on their installation and shows you a list of these packages. It is most useful when finding libraries, but it can be used on packages in all sections.
<br><br>

- **Start out with a dry run:**

```
deborphan --guess-all
```
<br>

- **Remove unnecessary data packages:**

```
sudo deborphan --guess-data | xargs sudo aptitude -y purge
```
<br>

- **Delete unnecessary libraries:**

```
sudo deborphan | xargs sudo apt-get -y remove --purge
```
<br><br>

</details>
<br>

Another option is to edit `/etc/UPower/UPower.conf` and change `IgnoreLid=` to `true`
<br><br><br>

### Add root password
<br>

- **Switch to root and add a password:**

```
sudo -i
passwd
```
<br>

- **To switch to the root shell:** `su -`
<br><br><br>


### Remove x11 and everything that uses it, including all configuration
Because I don't need a GUI.
<br><br>

```
sudo apt-get purge libx11.* libqt.*
```

```
sudo apt autoremove
```
<br><br><br>

### Remove legacy services
<br><br>

```
sudo apt-get --purge remove xinetd nis tftpd tftpd-hpa telnetd rsh-server rsh-redone-server
```

<br><br><br>

---
