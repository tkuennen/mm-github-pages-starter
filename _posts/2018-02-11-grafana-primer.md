Install InfluxDB

Add the influxdb repository
```
cat <<EOF | sudo tee /etc/yum.repos.d/influxdb.repo
[influxdb]
name = InfluxDB Repository - RHEL \$releasever
baseurl = https://repos.influxdata.com/rhel/\$releasever/\$basearch/stable
enabled = 1
gpgcheck = 1
gpgkey = https://repos.influxdata.com/influxdb.key
EOF
```
Install influxdb via yum
```
sudo yum install -y influxdb
```
Start influxdb
```
sudo service influxdb start
```

Enable influxdb on boot
```
sudo systemctl enable influxdb
```


Install Grafana

```
sudo yum install https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana-4.6.3-1.x86_64.rpm
sudo yum install initscripts fontconfig freetype* urw-fonts
sudo rpm -Uvh grafana-4.6.3-1.x86_64.rpm

```

Start the service
```
sudo service grafana-server start
```

Enable the service on boot

```
sudo systemctl enable grafana-server
```
