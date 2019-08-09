---
title: SSH Security with Match Rules
---

Locking down ssh while allowing root access from specified hosts or network ranges.

Deny root logins except from trusted workstations

```
PermitRootLogin no
```

IPv6localhost,IPv4localhost,list of trusted workstations

```
Match Address ::1,127.0.0.1,< comma separated list of addresses here>
   PermitRootLogin yes
Match
```
