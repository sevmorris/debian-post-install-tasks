
# Debian Post-Install Tasks

<br><br>

### Updates & upgrades...
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

I sometimes set up ssh and switch to it to finish the rest of the following steps.
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
ifconfig | grep "inet "﻿
```

</details>
<br>

### <a name="2"></a>Install [Oh My Bash](https://github.com/ohmybash/oh-my-bash)
<br>

```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```
<br>

### Using [FIGlet](https://github.com/cmatsuoka/figlet) & [neofetch](https://github.com/dylanaraps/neofetch/wiki/Installation#ubuntu)
<br>

Edit `.bashrc` and:

- Change the theme to <span style="color:red">Zork</span>
- Add the following, replacing \<TEXT> with whatever you would like FIGlet to display

```
echo "$(tput bold)$(tput setaf 3)"
figlet <TEXT>
```
<br>

- Add `neofetch` at the bottom
<br>

Reload `.bashrc` to see the changes immediately: `source .bashrc`
<br><br>


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
<br><br>

</details>
<br>

### Change motd
(message of the day)
<br>

**The folder** `/etc/update-motd.d` **contains numbered shell scripts which are run in order.<br>
You can modify or remove them, and/or add your own.**
<br><br><br>

### Comprehensive update & cleanup alias
<br>

- **Create a file called** .bash\_aliases **and add the following line:**

```
alias update='sudo apt update && sudo apt -o Dpkg::Options::="--force-confdef" dist-upgrade -y && sudo apt autoremove -y && if sudo test -f /var/run/reboot-required; then read -p "A reboot is required to finish installing updates. Press [ENTER] to reboot now, or [CTRL+C] to cancel and reboot later." && sudo reboot; else echo "A reboot is not required. Exiting..."; fi'
```
<br>

- **Add the following lines to** bashrc:

```
if [ -f ~/.bash_aliases ]; then
. ~/.bash_aliases
fi
```
<br>

- **Reload** .bashrc: `source .bashrc`
<br><br>

	**Alternatively, you can simply add the alias to** .bashrc **instead of creating a separate aliases file.**
<br><br>
[Source](https://askubuntu.com/a/1305901)
<br><br><br>


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
