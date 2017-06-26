You can create the file `~/.ssh/config` where you can define for example the username and the private key to be used to connect to a certain host:

```
$ cat ~/.ssh/config

Host 172.25.xx.xx
  User ossec
  IdentityFile /var/ossec/.ssh/id_rsa
```

Now the command `ssh 172.25.xx.xx` is the same as `ssh -i /var/ossec/.ssh/id_rsa ossec@172.25.xx.xx`
