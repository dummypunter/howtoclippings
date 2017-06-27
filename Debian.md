# Change IP routing table

In a VM I added a host only interface (eth1) to the existing NAT interface (eth0). And then I lost internet connectivity. I saw in the routing table that the default route was the eth1 interface:

```$ ip route
default via 192.168.75.1 dev eth1  proto static  metric 100 
default via 192.168.72.2 dev eth0  proto static  metric 101 
169.254.0.0/16 dev eth1  scope link  metric 1000 
192.168.72.0/24 dev eth0  proto kernel  scope link  src 192.168.72.202  metric 100 
192.168.75.0/24 dev eth1  proto kernel  scope link  src 192.168.75.131  metric 100 
```
The remedy was to change the metric of the eth1:
```
$ sudo ip route add default via 192.168.75.1 dev eth1  proto static  metric 102
$ sudo ip route del default via 192.168.75.1 dev eth1  proto static  metric 100
$ ip route 
default via 192.168.72.2 dev eth0  proto static  metric 101 
default via 192.168.75.1 dev eth1  proto static  metric 102 
169.254.0.0/16 dev eth1  scope link  metric 1000 
192.168.72.0/24 dev eth0  proto kernel  scope link  src 192.168.72.202  metric 100 
192.168.75.0/24 dev eth1  proto kernel  scope link  src 192.168.75.131  metric 100 
```



