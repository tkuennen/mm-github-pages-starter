Some notes on the switch to firewalld as the default firewall on CentOS/RHEL 7.

List active zones:
```
firewall-cmd --get-active-zones
```

List all services for a zone:
```
firewall-cmd --zone=public --list-all
```

Mapping Zones to network adapters:
```
firewall-cmd --zone=dmz --change-interface=eth1
firewall-cmd --zone=internal --change-interface=eth0
```

Adding public services to the proper zone
```
firewall-cmd --zone=dmz --add-service=http
firewall-cmd --zone=dmz --add-service=https
```

Remove unnecessary services:
```
firewall-cmd --zone=dmz --remove-service=ssh
```

Save runtime to persistent
```
firewall-cmd --runtime-to-permanent
```


Config file locations
```
/etc/firewalld
/usr/lib/firewalld
```

To use iptables instead of firewalld

```
yum install iptables-services
systemctl disable firewalld
systemctl enable iptables
restart
```
