Disable Suspend and Hibernation in Linux

`sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target`

Reboot

Verify

`sudo systemctl status sleep.target suspend.target hibernate.target hybrid-sleep.target`
