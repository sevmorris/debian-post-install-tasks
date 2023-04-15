
Place script in /etc/cron.daily
Generate a vnstati bandwidth usage png, append the date

`vnstati -s -i wlp4s0 -o /home/$USER/network-log-"`date +"%d-%m-%Y"`".png`
