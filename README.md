# FreeBSD scripted installation

This script is intended to be used on a remote dedicated servers - like Kimsufi, SoYouStart, One Provider.
You need to boot from FreeBSD rescue disk in order to use it.

It will create small UFS root filesystem, swap and make basic configuration including pf firewall allowing for ssh access on port 2222 and inbound ntp protocol only. Why it does not setup ZFS root? Because when you run into trouble later (hopefully not!) then you won't be able to use your ZFS pool from outdated rescue disk. If more than a single device is provided to the script, it will create mirrors for both root and swap. The rest of disk is not allocated, as probably you want to do it in your way. My approach is to do geli-encrypted-mirrored setup for any data stored remotely.

# SSH public keys
There is a place in script where you should put your pub keys in order to access machine as root by ssh.
SSH daemon is listening on port 2222 an allowing root login via pubkeys only.

# How to use - examples
Install FreeBSD 12.1 on a single disk da0, make swap size 2G:
```
./FreeBSD-install -d /dev/ada0 -h myhost.my.domain.com -s 2G -r 12.1
```
Install release 12.1 on a mirrored ada0 and ada1 disks:
```
./FreeBSD-install -d /dev/ada0,/dev/ada1 -h myhost.my.domain.com -s 512M -r 12.1
```

Do any tweaks of your fresh installation under /mnt directory. Probably you want to adjust your time zone. After reboot host should be ready and ssh accessible.
Always run system update and boot it once more. Congratulations!
