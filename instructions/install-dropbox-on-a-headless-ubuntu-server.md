
## How to Install Dropbox on a Headless Ubuntu Server

64 bit

`wget https://www.dropbox.com/download?plat=lnx.x86_64 -O dropbox-linux.tar.gz`
<br><br>

32 bit

`wget https://www.dropbox.com/download?plat=lnx.x86 -O dropbox-linux.tar.gz`
<br><br>

Create a directory in /opt and extract Dropbox to /opt/dropbox/.

`sudo mkdir /opt/dropbox/`
<br>
`sudo tar xvf dropbox-linux.tar.gz --strip 1 -C /opt/dropbox`
<br><br>

Install dependency packages for Dropbox.

`sudo apt install libc6 libglapi-mesa libxdamage1 libxfixes3 libxcb-glx0 libxcb-dri2-0 libxcb-dri3-0 libxcb-present0 libxcb-sync1 libxshmfence1 libxxf86vm1`
<br><br>

Run the Dropbox daemon.

`/opt/dropbox/dropboxd`
<br><br>

It will ask you to visit a web address in order to link your Ubuntu server with your Dropbox account.

Copy and paste the link in your web browser, log into your Dropbox account and click the Connect button to link your Ubuntu server with your Dropbox account.

Once the link is complete, return to the terminal window and press Ctrl+C to temporarily stop Dropbox daemon because it by default runs in the foreground and you can’t run other commands. The Dropbox sync folder appears in your home directory.
<br><br>

## Enable Auto-Start

Create a systemd service unit.

`sudo nano /etc/systemd/system/dropbox.service`
<br><br>

Put the following lines into the file. We want Dropbox to run as a standard user instead of root, so replace username with your real username. If it runs as root user, then files in ~/Dropbox will not sync properly.

```
[Unit]
Description=Dropbox Daemon
After=network.target

[Service]
Type=simple
User=username
ExecStart=/opt/dropbox/dropboxd
ExecStop=/bin/kill -HUP $MAINPID
Restart=always

[Install]
WantedBy=multi-user.target
```
<br><br>

Save and close the file, then start the service.

`sudo systemctl start dropbox`
<br><br>

Enable auto-start at system boot time.

`sudo systemctl enable dropbox`
<br><br>

Check its status.

`systemctl status dropbox`
<br><br>

[source](https://www.linuxbabe.com/ubuntu/install-dropbox-headless-ubuntu-server)
