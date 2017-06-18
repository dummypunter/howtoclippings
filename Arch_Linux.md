# Arch Linux

Arch Linux setup in a Pi for kodi media player

## Autologin

***Didn't worked!!!***

https://wiki.archlinux.org/index.php/LightDM#Enabling_autologin
 
username must be part of the autologin group to be able to login automatically without entering your password:
```
# groupadd -r autologin`
# gpasswd -a username autologin
```
https://wiki.archlinux.org/index.php/LightDM#Enabling_LightDM
 
 lightdm --test-mode --debug
 
## Start kodi
``` 
[alarm@alarmpi ~]$ /usr/bin/kodi-standalone &
which: no start-pulseaudio-x11 in (/usr/local/sbin:/usr/local/bin:/usr/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl)
which: no pulse-session in (/usr/local/sbin:/usr/local/bin:/usr/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl)
``` 
 
 
 
 

 
 
https://archlinuxarm.org/forum/viewtopic.php?f=64&t=8470
 
modify gpu_mem in config.txt (/boot/config.txt???) 
 
As the default in Arch is 64MB it wasn't enough..! It took me a few hours to understand my mistake so, what is your gpu_mem setting ?
 
https://raspberrypi.stackexchange.com/questions/673/what-is-the-optimum-split-of-main-versus-gpu-memory
 
New Firmware (after October 2012)
Edit /boot/config.txt and add or edit the following line:
gpu_mem=16


The value can be 16, 64, 128 or 256 and represents the amount of RAM available to the GPU.
 
https://wiki.archlinux.org/index.php/time#Time_zone
To check the current zone defined for the system:
$ timedatectl


To list available zones:
$ timedatectl list-timezones


To change your time zone: 
$ timedatectl set-timezone Europe/Athens
 
Install Transmission
# pacman -S transmission-cli
 
View system logs
$ journalctl -xn
 
Configure Static IP Address
https://wiki.archlinux.org/index.php/netctl
# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether b8:27:eb:53:2e:db brd ff:ff:ff:ff:ff:ff
 
# cp /etc/netctl/examples/ethernet-static /etc/netctl/eth0
# vi /etc/netctl/eth0
# cat /etc/netctl/eth0
Description='A basic static ethernet connection'
Interface=eth0
Connection=ethernet
IP=static
Address=('192.168.1.22/24')
#Routes=('192.168.0.0/24 via 192.168.1.2')
Gateway='192.168.1.1'
DNS=('192.168.1.1')
 
## For IPv6 autoconfiguration
#IP6=stateless
 
## For IPv6 static address configuration
#IP6=static
#Address6=('1234:5678:9abc:def::1/64' '1234:3456::123/96')
#Routes6=('abcd::1234')
#Gateway6='1234:0:123::abcd'
[root@alarmpi]# netctl enable eth0
 
Kodi plugins offline
http://mirrors.kodi.tv/addons/krypton
 
(krypton is the version of kodi in Pi)
 
Info for plugins: https://kodi.tv/addon/plugins-video-add-ons/youtube
Download the zip file with the plugin eg:
http://mirrors.kodi.tv/addons/krypton/plugin.video.youtube/plugin.video.youtube-5.3.12.zip
 
 
Removing packages
To remove a package and its dependencies which are not required by any other installed package:
# pacman -Rs package_name


Transmission
Mount external disk
Mount the external disk, givin access to the user “alarm” and the group “transmission”.
 
Find the userid of user “alarm” and the groupid of group “transmission”:
 
[root@alarmpi alarm]# grep alarm /etc/passwd
alarm:x:1000:1000::/home/alarm:/bin/bash
[root@alarmpi alarm]# grep trans /etc/group
transmission:x:169:
 
Edit /etc/fstab:
 
//192.168.1.1/Partition1 /mnt/FreeAgent cifs username=<username>,password=<password>,uid=1000,gid=169,iocharset=utf8 0 0
 
[root@alarmpi alarm]# umount /mnt/FreeAgent/
[root@alarmpi alarm]# mount -a
[root@alarmpi alarm]# ls -l /mnt/          
total 0
drwxrwxrwx 1 alarm transmission 0 Aug 13  2016 FreeAgent
Set-up transmission
https://github.com/transmission/transmission/wiki/Editing-Configuration-Files
https://wiki.archlinux.org/index.php/transmission
 
# systemctl enable transmission
# vi /var/lib/transmission/.config/transmission-daemon/settings.json
    "rpc-whitelist": "127.0.0.1,192.168.1.100",
    "download-dir": "/mnt/FreeAgent/finished/movies",
    "incomplete-dir": "/mnt/FreeAgent/downloading",
    "incomplete-dir-enabled": true,
    "speed-limit-up": 5,
    "speed-limit-up-enabled": true,
:wq
 
# systemctl start transmission
 
Go to web interface:
http://192.168.1.22:9091/transmission/web/
 
Dynamic DNS
# pacman -S ddclient
System update
Issue the following command repeatedly until all packages are downloaded:
 
# pacman -Syuwq
 
When all packages are downloaded, perform the upgrade:
 
# pacman -Syuq
Disable IPv6
Corrects connection problems with pacman upgrades!!!
 
https://wiki.archlinux.org/index.php/IPv6#Disable_IPv6
https://wiki.archlinux.org/index.php/Kernel_parameters
https://www.raspberrypi.org/forums/viewtopic.php?f=53&t=14607
 
You can disable IPv6 by adding "ipv6.disable=1" to the kernel command line, in /boot/cmdline.txt -- I just edited it and stuck it in before the "console=" directive.
 
After that, all my pacman connection woes disappeared.
 
# cat /boot/cmdline.txt 
root=/dev/mmcblk0p2 rw rootwait ipv6.disable=1 console=ttyAMA0,115200 console=tty1 selinux=0 plymouth.enable=0 smsc95xx.turbo_mode=N dwc_otg.lpm_enable=0 kgdboc=ttyAMA0,115200 elevator=noop
 
 
 
 
 
 
 
