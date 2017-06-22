## Agentless for Solaris

The stat command is used by the agentless script for linux to get the size:permissions:groupid:userid:

```
$ stat --printf "%s:%a:%u:%g" foo
1279:550:0:501
```
We can get the same info from the stat function of perl:
```
$ perl -e '@a=stat "foo";print "$a[7]:".substr(sprintf( "%04o",$a[2]),3).":$a[4]:$a[5]\n";'
1279:550:0:501
```

The command that must be executed in solaris is the following:
```
for i in `find . -type f 2>/dev/null`;do tail $i >/dev/null 2>&1 && sha1=`digest -a sha1 $i` && echo FWD: `perl -e '@a=stat $ARGV[0];print "$a[7]:".substr(sprintf( "%04o",$a[2]),3).":$a[4]:$a[5]\n";' $i`:xxx:$sha1 $i;done;
```

The respective expect command to be added in the agentless script is:
```
send "echo \"INFO: Starting.\";for i in `find . -type f 2>/dev/null`;do tail \$i >/dev/null 2>&1 && sha1=`digest -a sha1 \$i` && echo FWD: `perl -e '@a=stat shift;print \"\$a\[7\]:\".substr(sprintf( \"%04o\",\$a\[2\]),3).\":\$a\[4\]:\$a\[5\]\n\"\;' \$i`:xxx:\$sha1 \$i;done;exit\r"
```
