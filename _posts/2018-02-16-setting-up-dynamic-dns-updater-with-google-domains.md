---
title: Setting up a Dynamic DNS updater for Google Cloud DNS with dig & curl
---

Google Domains has a pretty sweet deal for domain registration 

```
USERNAME="username"
PASSWORD="password"
HOSTNAME="record.tld"
# Resolve current public IP
IP=$( dig +short myip.opendns.com @resolver1.opendns.com )
# Update Google DNS Record
URL="https://${USERNAME}:${PASSWORD}@domains.google.com/nic/update?hostname=${HOSTNAME}&myip=${IP}"
curl -s $URL
```
