
### Debian Post-Install Tasks
<br>

<details>
  <summary>Add root password/add new user/add user to sudoers</summary>
<br>

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

```
# Enable and start sshd at boot time
sudo systemctl enable ssh.service

# Confirm sshd is enabled at boot time
sudo systemctl is-enabled ssh.service

# Check server status
sudo service ssh status

# Start sshd
sudo systemctl start ssh.service

# Restart the server
sudo systemctl restart ssh.service

# Show ip address
ip a | grep "inet "﻿
```

---

</details>

<details>
  <summary>Configure key-based SSH authentication</summary>

```
# Generate keys on the local machine (skip if you already have a key pair):
ssh-keygen -t rsa

# Copy contents of public key file to remote authorized_keys file, changing {user} & {ip} as needed:
scp ~/.ssh/id_rsa.pub {user}@{ip}:

# Log into remote machine

# Create (or append to) authorized_keys file, copy pub key to it, then clean up:
mkdir -p .ssh
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
rm ~/id_rsa.pub
```

---

</details>

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

# The text after "figlet" is displayed, so change it as desired...
echo "$(tput bold)$(tput setaf 3)"
figlet Debian!

neofetch
```

```
# Reload .bashrc
source ~/.bashrc
```

---

</details>

<details>
  <summary>Boot to console/skip GRUB boot menu</summary>

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
