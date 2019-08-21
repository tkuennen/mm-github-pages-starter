Upgrading the Ubiquity Unifi Controller on RedHat/CentOS Linux.

Back in mid 2016 Ubiquiti pulled the links for the source Linux installers from the download pages. From what I can tell they are trying to standardize on Debian/Ubuntu. Using one of the old links I had in my history, I was able to download the latest updated by changing the release number in the address. These are the steps that I have been using to upgrade the Unifi controller since ~June of 2016.


First create a backup of the current configuration.
```
tar –zcvf /root/backup.tar.gz /opt/UniFi/data
```
Stop the Unifi service
```
/sbin/service unifi stop
```
Move the current setup to a new directory
```
mv /opt/Unifi /opt/Unifi.5.8.7
```
Download the latest version of the Unifi Controller.
```
wget https://www.ubnt.com/downloads/unifi/5.4.11/UniFi.unix.zip
```
Extract the new controller
```
unzip UniFi.unix.zip
```
Change the owner to the Unifi username and group.
```
chown -R ubnt:ubnt /opt/UniFi
```
Move the data to the new code base.
```
mv /opt/Unifi.5.8.7/data /opt/Unifi/data
```
Start the Unifi service
```
/sbin/service unifi start
```
