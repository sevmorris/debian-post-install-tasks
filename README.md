
### Debian Post-Install Tasks & Miscellany
<br>

<details>
  <summary>Install some things</summary>

```
# Run each line separately
sudo apt update
sudo apt upgrade
sudo apt -y install curl openssh-server ii git figlet tldr neofetch deborphan aptitude htop
sudo apt install build-essential dkms linux-headers-$(uname -r)

# Install Oh My Bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"

# Install Github CLI (run the following all at once)

type -p curl >/dev/null || (sudo apt update && sudo apt install curl -y)
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
&& sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
```

---

</details>

<details>
  <summary>Edit .bashrc</summary>
<br>

_This assumes I've installed everything above_
<br>

Change the theme to `Zork`
<br>

Paste the following at the bottom of .bashrc

```
alias update='sudo apt update && sudo apt -o Dpkg::Options::="--force-confdef" dist-upgrade -y && sudo apt autoremove -y && if sudo test -f /var/run/reboot-required; then read -p "A reboot is required to finish installing updates. Press [ENTER] to reboot now, or [CTRL+C] to cancel and reboot later." && sudo reboot; else echo "A reboot is not required. Exiting..."; fi'

echo "$(tput bold)$(tput setaf 3)"
figlet Debian!

neofetch
```

<br>
Reload .bashrc

```
source ~/.bashrc
```

---

</details>

<details>
  <summary>Set up ssh</summary>

```
# Enable and start sshd at boot time:
sudo systemctl enable ssh.service

# Confirm sshd is enabled at boot time:
sudo systemctl is-enabled ssh.service

# Check server status:
sudo service ssh status

# Start sshd:
sudo systemctl start ssh.service

# Restart the server:
sudo systemctl restart ssh.service

# Show ip address:
ip a | grep "inet "﻿
```

---

</details>

<details>
  <summary>Boot to console (for hosts I'll only ssh into)</summary>

```
#Backup the configuration file:
sudo cp -n /etc/default/grub /etc/default/grub.backup

# Edit the configuration file:
sudo nano /etc/default/grub

# Comment out (disable) GRUB_CMDLINE_LINUX_DEFAULT:
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`

# Change GRUB_CMDLINE_LINUX "" to:
GRUB_CMDLINE_LINUX="text"

# Uncomment (enable) GRUB_TERMINAL:

GRUB_TERMINAL="console"

# Save the file and apply changes:
sudo update-grub

# And finally:
sudo systemctl set-default multi-user.target
```

---

</details>

<details>
  <summary>Boot straight into Debian</summary>

```
# Open the GRUB config file and set GRUB_TIMEOUT to 0
sudo nano /etc/default/grub

# Update GRUB
sudo update-grub
```

---

</details>
<br>

### Miscellany

<details>
  <summary>Add root password</summary>
<br>

```
# Switch to root and add a password:
sudo -i
passwd

# To switch to the root shell
 su -
```

---

</details>

<details>
  <summary>Run Deborphan</summary>

<br>
Deborphan finds "orphaned" packages on your system. It determines which packages have no other packages depending on their installation and shows you a list of these packages. It is most useful when finding libraries, but it can be used on packages in all sections.
<br><br>

```
# Start out with a dry run:
deborphan --guess-all

# Remove unnecessary data packages:
sudo deborphan --guess-data | xargs sudo aptitude -y purge

# Delete unnecessary libraries:
sudo deborphan | xargs sudo apt-get -y remove --purge
```

---

</details>

<details>
  <summary>Add a user to sudoers</summary>

<br>


```
# Switch to root
su - root

# Add user (change <user> to correct username)
usermod -aG sudo <user>
```

---
