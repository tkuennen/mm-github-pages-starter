---
categories:
  - Blog
tags:
  - Linux
  - 2FA
  - Networking
  - Tools
---

One thing that I've commonly encountered while working on networks is that I lack quick access to the tools I need for secure authentication and troubleshooting. For my homelab network I generally run one service per container or virtual machine for security and manageability reasons. Not all networks warrant such a setup so I put this guide together as a way of running all of these services on a single system.

All of these applications can be run securely on a small Linux system requiring very little resources with a little bit of engineering. This opens up the door for running all of these services on a Single Board Computer (SBC) such as a RaspberryPi or a BeagleBoard thus making it easy to deploy at remote locations. Furthermore these small systems require very little power so they can be powered through alternative means such as battery, solar or a POE port on a switch.

The software....

* Bind9 - DNS server - In this setup we are using Bind as our master DNS server for the internal network and as a caching resolver. The Bind DNS server will point to Cloudflare's 1.1.1.1 server using DNS over TLS for end to end encryption of all DNS traffic over the WAN interface.
* PiHole - Ad/Malware blocking DNS server - PiHole is a nifty project that allows you to block pretty much all advertisements and malware domains for your entire network. In this configuration we will be pointing PiHole's DNS server that runs DNS dnsmasq under the hood at the ISC Bind DNS server for internal name resolution.
* Tools - Basic network tools to do things like generate QR Codes to join the wifi and basic troubleshooting of DNS and the overall network
* FreeRADIUS - Authentication
* Duo Authentication Proxy - Two Factor Authentication
* Unifi Network Controller - Manage your Ubiquiti network from a single pane of glass (For the most part)
* NGINX - Reverse proxy - This will be the front end for all of the applications as well as provide SSL termination and SSL certificates
* LetsEncrypt - Valid auto renewing SSL certificates
* Certbot/ACME - Automated SSL certificate renewal
* Cockpit - Web administration Console for Linux

## Debian Buster (10.x...)

To start off with this little project we will use Debian Buster (10x) with the minimal installer
https://cdimage.debian.org/mirror/cdimage/release/current/amd64/iso-cd/debian-10.0.0-amd64-netinst.iso

## Useful Tools

First and foremost you will want to make sure that the system is up to date with the latest packages. Please make sure to secure your system before making anything public facing!

```
sudo apt -y update && apt -y upgrade
```

Since this network controller is intended to be the central management point for a network it makes sense to install some utilities that will assist with troubleshooting, and maintaining the network.

One liner to install them all

```
sudo apt -y install bind9 iperf iperf3 net-tools pwgen diceware qrencode curl wget gcc sudo
```

Add your user to the sudo group
```
sudo usermod -g sudo <username>
```

## Ubiqiti Networks Unifi Controller

Look at install scripts
@ https://get.glennr.nl/unifi/install/unifi-5.10.27.sh

```
wget -O /etc/apt/trusted.gpg.d/unifi-repo.gpg https://dl.ui.com/unifi/unifi-repo.gpg
echo 'deb http://www.ubnt.com/downloads/unifi/debian unifi-5.10 ubiquiti' | tee /etc/apt/sources.list.d/100-ubnt-unifi.list
```

```
wget -qO - https://www.mongodb.org/static/pgp/server-3.4.asc | sudo apt-key add - || abort
echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.4.list || abort
```
## FreeRADIUS
![Logo](/assets/images/freeradius.png)

Install FreeRADIUS
```
apt -y install freeradius
```

Enable freeradius on startup

```
systemctl enable freeradius
```

Start FreeRADIUS
```
systemctl start freeradius
```

Simple test of RADIUS functionality
```
radtest testing password localhost 0 testing123
```

## Duo Authentication Proxy
![Logo](/assets/images/duo.png)

Ensure prerequisits are installed

```
sudo apt-get install -y build-essential python-dev libffi-dev perl zlib1g-dev
```

Download the latest Duo authproxy software

```
curl -o duoauthproxy-latest-src.tgz https://dl.duosecurity.com/duoauthproxy-latest-src.tgz
```

Extract the source files

```
mkdir duoauthproxy-latest-src && tar -xzf duoauthproxy-latest-src.tgz -C duoauthproxy-latest-src --strip-components 1
```

Create Duo group

```
sudo /sbin/groupadd duo_authproxy_grp
```

Create Duo User and add to the Duo group

```
sudo /sbin/useradd duo_authproxy_svc -g duo_authproxy_grp
```


Configure, make, and install Duo authproxy
```
cd duoauthproxy-latest-src
make
cd duoauthproxy-build
./install
```

Create a backup of the original authproxy configuration file.
```
cp /opt/duoauthproxy/conf/authproxy.cfg /opt/duoauthproxy/conf/authproxy.cfg.orig
```
Start the Duo authproxy service and enable it on boot

```
systemctl enable duoauthproxy && systemctl start duoauthproxy
```

## NGINX SSL front end reverse proxy
![Logo](/assets/images/nginx.png)

## Cockpit Server Management Interface

Install cockpit
```
sudo apt -y install cockpit
```

Customization
```
/usr/share/cockpit/branding
```
