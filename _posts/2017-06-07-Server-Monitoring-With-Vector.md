I'm a big fan of some of the open source projects that Netflix has released over the years.


Installing Performance Co-Pilot on RHEL/CentOS
```
git clone git://git.pcp.io/pcp
```
```
sudo yum-builddep -y pcp
```
```
cd pcp
```
```
./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --with-webapi
```
```
make
```
```
groupadd -r pcp
```
```
useradd -c "Performance Co-Pilot" -g pcp -d /var/lib/pcp -M -r -s /usr/sbin/nologin pcp
```
```
make install
```
Deploying to a Web Server
```
 $ cd /var/www/html
 $ sudo wget https://dl.bintray.com/netflixoss/downloads/1.1.0/vector.tar.gz
 $ sudo tar xvzf vector.tar.gz
```
