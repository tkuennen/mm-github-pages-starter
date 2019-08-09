__Install prerequisites__

```
yum -y install epel-repository
yum -y install easy-rsa iptables-services net-tools
yum -y install epel-repository
yum -y install easy-rsa iptables-services net-tools
```
__Install OpenVPN Access Server__

CentOS/RHEL 6
```
curl -O http://swupdate.openvpn.org/as/openvpn-as-2.1.12-CentOS6.x86_64.rpm
sudo rpm -ivh openvpn-as-2.1.12-CentOS6.x86_64.rpm
```

CentOS/RHEL 7

```
curl -O http://swupdate.openvpn.org/as/openvpn-as-2.1.12-CentOS7.x86_64.rpm
sudo rpm -ivh openvpn-as-2.1.12-CentOS7.x86_64.rpm
```

__Start & Enable__

CentOS/RHEL 6

```
/sbin/chkconfig openvpnas on
service openvpnas start
```

CentOS/RHEL 7

```

systemctl enable openvpnas
systemctl start openvpnas
```

__ Set Password__

```
passwd openvpn
```
