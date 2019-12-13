---
categories:
  - Blog
tags:
  - Linux
  - Git
  - Tools
---

Installing Gogs on CentOS 8 

```
wget https://dl.gogs.io/0.11.91/gogs_0.11.91_linux_386.tar.gz
tar -xzvf ./gogs_0.11.91_linux_386.tar.gz
```
-t = number of threads

-c = number of client sessions to simulate

-d = duration in seconds 

```
wrk -t12 -c400 -d30s page to test
```
