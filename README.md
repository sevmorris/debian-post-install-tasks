1. This is my Debian post-install task list. There are many like it, but this one is mine.

2. My Debian post-install task list is my best friend. It is my life. I must master it as I must master my life.

3. My Debian post-install task list, without me, is useless. Without my Debian post-install task list, I am useless.

<br>

_The steps are to be performed in the order shown._
<br>

Log into the new Debian machine using the default credentials, open a terminal, then proceed...

<details>
  <summary>Add root password/add new user/add user to sudoers</summary>

---

```
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

```
# Enable and start sshd at boot time
sudo systemctl enable ssh.service

# Confirm sshd is enabled at boot time
sudo systemctl is-enabled ssh.service

# Check server status
sudo service ssh status

# Start sshd
sudo systemctl start ssh.service

# Show ip address
ip a | grep "inet "﻿
```

---

</details>

<details>
  <summary>Configure key-based ssh authentication</summary>

---

```
# Generate keys on the local machine (skip if you already have a key pair):
ssh-keygen -t rsa

# It is strongly advised that you give the key a strong passphrase...

# Copy contents of public key to remote authorized_keys
# file (change {user} & {ip} as needed):
scp ~/.ssh/id_rsa.pub {user}@{ip}:

# Log into remote machine

# Create .ssh directory (if it doesn't already exist):
mkdir -p .ssh

# Copy public key to authorized_keys file (will be created if it doesn't exist):
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys

# Clean up:
rm ~/id_rsa.pub
```

---

</details>

<details>
  <summary>Install some things</summary>

---

```
# Run each line separately
sudo apt update
sudo apt upgrade
sudo apt -y install curl openssh-server ii git figlet tldr neofetch aptitude htop
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
  <summary>.bashrc</summary>

 ---

Edit .bashrc:

`nano ~/.bashrc`

<br>
Change the theme to `Zork` then paste the following at the bottom of .bashrc:
<br><br>

```
alias update='sudo apt update && sudo apt -o Dpkg::Options::="--force-confdef" dist-upgrade -y && sudo apt autoremove -y && if sudo test -f /var/run/reboot-required; then read -p "A reboot is required to finish installing updates. Press [ENTER] to reboot now, or [CTRL+C] to cancel and reboot later." && sudo reboot; else echo "A reboot is not required. Exiting..."; fi'

# The text after "figlet" is displayed, so change it as desired...
echo "$(tput bold)$(tput setaf 3)"
figlet Debian!

neofetch
```


Reload .bashrc

`source ~/.bashrc`

---

</details>

<details>
  <summary>Boot to console/skip GRUB boot menu</summary>

---

```
# Change boot target to console mode
sudo systemctl set-default multi-user.target

# Skip boot options: Change GRUB_TIMEOUT=0
sudo nano /etc/default/grub

# Update grub
sudo update-grub

# Reboot the system
sudo reboot

```

---

</details>
