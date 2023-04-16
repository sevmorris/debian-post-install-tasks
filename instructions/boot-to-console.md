```
# Backup the configuration file
sudo cp -n /etc/default/grub /etc/default/grub.backup

# Edit the configuration file
sudo nano /etc/default/grub

# Change GRUB_TIMEOUT to 0 (skips boot options):
GRUB_TIMEOUT=0

# Comment out (disable) GRUB_CMDLINE_LINUX_DEFAULT:
# GRUB_CMDLINE_LINUX_DEFAULT="quiet"

# Change GRUB_CMDLINE_LINUX "" to:
GRUB_CMDLINE_LINUX="text"

# Uncomment (enable) GRUB_TERMINAL:
GRUB_TERMINAL="console"

# Save the file and apply changes
sudo update-grub

# And finally, to change boot target to text mode:
sudo systemctl set-default multi-user.target
```
