run a command every minute
```
crontab -e
 */1 * * * * for i in `/bin/find /var/log/isokratis/ -mtime +0 -mtime -3`; do echo put $i | sftp 172.25.33.80:/storage01/Integrity_check/ >> /var/ossec/zzzzz;done;
```

windows netbios name:

* from windows: `nbtstat -A 10.95.142.138`
* from samba: `nmblookup -A 172.25.3.137`
