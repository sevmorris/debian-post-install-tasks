### MY Debian Post-Install Tasks
---

1. This is my Debian post-install task list. There are many like it, but this one is mine.

2. My Debian post-install task list is my best friend. It is my life. I must master it as I must master my life.

3. My Debian post-install task list, without me, is useless. Without my Debian post-install task list, I am useless.

---
<br>

_The steps are to be performed in the order shown._
<br>

Log into the new Debian machine using the default credentials, open a terminal, then proceed...

<br>

<details>
  <summary> Add root password/add new user/add user to sudoers</summary>

---

```console
# Switch to root
su -

# Add (or change from default) root password:
passwd

# Create new user (changing {username} to desired username)
adduser {username}

# Exit root
exit

# Switch to the new user
su {username}

# Add user account to the sudo group (enter root password when prompted)
su - -c 'gpasswd -a '$USER' sudo'

# Test sudo by listing the contents of the /root directory
exit
su {username}
sudo ls -la /root
```

---

</details>

<details>
  <summary>Set up ssh</summary>

---

```console
# Enable and start sshd at boot
sudo systemctl enable ssh.service

# Confirm sshd is enabled at boot
sudo systemctl is-enabled ssh.service

# Check server status
sudo service ssh status

# Start sshd
sudo systemctl start ssh.service

# Show ip address
hostname -I﻿
```

---

</details>

<details>
  <summary>Configure key-based ssh authentication</summary>

---

```console
# Generate keys on the LOCAL machine (skip if you already have a key pair):
ssh-keygen -t rsa

# It is strongly advised that you give the key a strong passphrase...

# Copy contents of public key to remote authorized_keys
# file (change <user> and <ip> as needed):
scp ~/.ssh/id_rsa.pub <user>@<ip>:

# Log into remote machine

# Create .ssh directory (if it doesn't already exist), then copy
# public key to authorized_keys file (will be created if it doesn't exist):
mkdir -p .ssh && cat ~/id_rsa.pub >> ~/.ssh/authorized_keys

# Clean up:
rm ~/id_rsa.pub
```

---

</details>

<details>
  <summary>Install some things</summary>

---

```console
# Run each line separately
sudo apt update && sudo apt -y upgrade
sudo apt -y install cmatrix curl dkms figlet git htop lolcat neofetch net-tools nmon openssh-server ii tldr

# Install Github CLI (run the following all at once)

type -p curl >/dev/null || (sudo apt update && sudo apt install curl -y)
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
&& sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
```

Install Oh My Bash

```console
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```

---

</details>

<details>
  <summary>.bashrc & .hushlogin</summary>

 ---

Change Oh My Bash theme to my preference (Zork) and append some things to the bottom of .bashrc
<br>
(paste and execute the entire code block below):
<br><br>
_Due credit to [Mako-Wish](https://askubuntu.com/users/885743/mako-wish) for the [update alias](https://askubuntu.com/questions/118025/bypass-the-yes-no-prompt-in-apt-get-upgrade/1305901#1305901)_


```console
sed -i 's/font/zork/g' ~/.bashrc

cat >> ~/.bashrc << EOL

# Remove duplicate $PATH entries (https://unix.stackexchange.com/a/149054)
PATH="$(perl -e 'print join(":", grep { not $seen{$_}++ } split(/:/, $ENV{PATH}))')"

# A rather comprehensive update alias:
alias update='sudo apt update && sudo apt -o Dpkg::Options::="--force-confdef" dist-upgrade -y && sudo apt autoremove -y && if sudo test -f /var/run/reboot-required; then read -p "A reboot is required to finish installing updates. Press [ENTER] to reboot now, or [CTRL+C] to cancel and reboot later." && sudo reboot; else echo "A reboot is not required. Exiting..."; fi'

# Show line numbers in nano
alias nano="nano --linenumbers "

# The text after figlet is displayed as an ASCII text banner, so change it as desired...
figlet Your Message Here!
echo "" # Just to add a little space bellow the banner

neofetch
EOL

# Create an empty file to suppress the boot message/warning:
touch ~/.hushlogin

source ~/.bashrc
```

<br>
Change the Figlet message (changing "your NEW message" to whatever you want):
<br><br>

```console
sed -i 's/Your Message Here!/your NEW message/g' ~/.bashrc

source ~/.bashrc
```

---

</details>

<details>
  <summary>Boot to console/skip GRUB boot menu</summary>

---

```console
# Change boot target to console mode
sudo systemctl set-default multi-user.target

# Skip boot options
sudo sed -i 's/GRUB_TIMEOUT=.*/GRUB_TIMEOUT=0/' /etc/default/grub
sudo update-grub

# Reboot the system
sudo reboot

```

---

</details>

<details>
  <summary>Now take a break!</summary>

```console
cmatrix | lolcat
```

---

</details>
