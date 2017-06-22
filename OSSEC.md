## Solaris
```
$ stat --printf "%s:%a:%u:%g" foo
1279:550:0:501

$ perl -e '@a=stat "foo";print "$a[7]:".substr(sprintf( "%04o",$a[2]),3).":$a[4]:$a[5]\n";'
1279:550:0:501
```
