---
title: Security Enhanced Linux (SELinux) Notes
---

Some random but useful notes on file contexts in SELinux

Listing file context
```
ls -al --context
```
Changing file context
```
chcon -t system_conf_t ./iptables
```

Labels for http
```
sudo chcon -R -t httpd_sys_content_t /var/www/
```

Labels for samba
```
sudo chcon -R -t samba_share_t '/<shared path>(/.*)?'
semanage fcontext -a -t samba_share_t '/<shared path>(/.*)?'
restorecon -R /<shared path>
```

Labels for puppet
```
sudo chcon -R -t puppet_etc_t /etc/puppet
```

Allowing authorized keys to live on NFS home directories (CentOS/RHEL 7)
```
setsebool -P use_nfs_home_dirs 1
```
