mod_security Web Application Firewall (WAF)

Install git

```
yum install git
```

Install mod_security

```
yum install mod_security
```

Download ModSecurity
```
git clone https://github.com/SpiderLabs/ModSecurity.git
cd mod_security
./autogen.sh
./configure --enable-standalone-module
make
```
Enable Mod_Security

```
location / {
ModSecurityEnabled on;
ModSecurityConfig modsecurity.conf;
}
/etc/httpd/conf.d/

Include modsecurity.d/*.conf
Include modsecurity.d/activated_rules/*.conf

SecRuleEngine On
SecRequestBodyAccess On
SecResponseBodyAccess Off
```

Download Core Ruleset

```
git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git
```
Confirm modsecurity is working

Default Log Location is /var/log/httpd/modsec_audit.log
```
[Tue May 10 11:52:40 2016] [notice] ModSecurity for Apache/2.7.3 (http://www.modsecurity.org/) configured.
[Tue May 10 11:52:40 2016] [notice] ModSecurity: APR compiled version="1.3.9"; loaded version="1.3.9"
[Tue May 10 11:52:40 2016] [notice] ModSecurity: PCRE compiled version="7.8 "; loaded version="7.8 2008-09-05"
[Tue May 10 11:52:40 2016] [notice] ModSecurity: LUA compiled version="Lua 5.1"
[Tue May 10 11:52:40 2016] [notice] ModSecurity: LIBXML compiled version="2.7.6"
```
References & Further Reading:

https://github.com/SpiderLabs/ModSecurity/wiki

https://www.modsecurity.org/

https://github.com/SpiderLabs/ModSecurity

https://lists.owasp.org/pipermail/owasp-modsecurity-core-rule-set/2016-November/002265.html

https://github.com/SpiderLabs/owasp-modsecurity-crs/blob/v3.0/master/INSTALL
