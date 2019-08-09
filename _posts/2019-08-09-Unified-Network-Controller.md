# Unified Network Controller

All the tools you need to manage a small network on a single Linux host. This guide will walk you through setting up the following applications on a freshly minted Debian Buster (10x) machine.

## Debian Buster

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs help` - Print this help message.

## Useful Tools

Since this network controller is intended to be the central management point for a network it makes sense to install some utilities that will assist with troubleshooting, and maintaining the network.

One liner to install them all
```
apt -y install net-tools pwgen diceware qrencode curl wget
```

## Ubiqiti Networks Unifi Controller
```
curl https://dl.ui.com/unifi/5.10.25/unifi_sysvinit_all.deb
```
## FreeRADIUS
![Logo](/assets/images/freeradius.png)

Install FreeRADIUS

```
apt -y install freeradius
```

## Duo Authentication Proxy
![Logo](/assets/images/duo.png)

Ensure prerequisits are installed

```
apt-get install build-essential python-dev libffi-dev perl zlib1g-dev
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
/sbin/groupadd duo_authproxy_grp
```

Create Duo User and add to the Duo group

```
/sbin/useradd duo_authproxy_svc -g duo_authproxy_grp
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
