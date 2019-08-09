
Installing LetsEncrypt on an NGINX reverse proxy

Install the epel repository
```
sudo yum install epel-release
```

Install nginx and certbot
```
sudo yum -y install nginx certbot-nginx

```
Start nginx
```
sudo systemctl start nginx
```

Enable nginx on boot
```
sudo systemctl enable nginx
```

Enable http and https in firewalld
```
sudo firewall-cmd --add-service=http
sudo firewall-cmd --add-service=https
sudo firewall-cmd --runtime-to-permanent
```

Generate Letsencrypt certificates
```
sudo certbot --nginx -d traviskuennen.com -d intranet.traviskuennen.com
```
