---
categories:
  - Blog
tags:
  - Linux
  - macOS
  - Load Testing
  - Tools
---

Load testing with WRK

```
git clone https://github.com/wg/wrk.git
cd wrk
make
```
-t = number of threads

-c = number of client sessions to simulate

-d = duration in seconds 

```
wrk -t12 -c400 -d30s page to test
```
