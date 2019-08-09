yum-cron is useful to automate system updates of RHEL/CentOS systems.

Install
```
sudo yum -y install yum-cron
```

Configure
```
sudo vi /etc/yum/yum-cron.conf
```

For canary/testing systems
```
update_cmd = default
update_messages = yes
download_updates = yes
apply_updates = no
[email]
# The address to send email messages from.
email_from = root@localhost
# List of addresses to send messages to.
email_to = root
# Name of the host to connect to to send email messages.
email_host = localhost
```

For production

This will only download critical security updates, and they will have to be applied manually
```
update_cmd = security-severity:Critical
update_messages = yes
download_updates = yes
apply_updates = no
[email]
# The address to send email messages from.
email_from = root@localhost
# List of addresses to send messages to.
email_to = root
# Name of the host to connect to to send email messages.
email_host = localhost
```

Auto reboot on new kernel

  Only on testing systems
```
/etc/cron.weekly/newkernelcheck

/usr/bin/yum list recent | /bin/fgrep -q kernel
EXITVALUE=$?
if [ $EXITVALUE == 0 ]; then
   /sbin/reboot
fi
exit 0
```
