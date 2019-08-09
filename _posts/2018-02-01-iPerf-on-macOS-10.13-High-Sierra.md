---
title: Tracking down bandwidth issues with iperf on MacOS 10.13 High Sierra
---
# Bandwidth Monitoring with iperf on MacOS 10.13 High Sierra

After a recent rewiring project at home, I found that downloads were crawling via a specific wall port in the house. Unfortunately this port also happens to be in my office and is probably the most used Ethernet port in the house outside of the patch panel. Suspecting an incorrectly wired keystone jack or a kink in the wire, I wanted to test that this was the issue prior to tracing cables as this can be a time consuming process with lots of crawling around in the attic. I've used iperf in the past so I thought I would give it a try on macOS High Sierra.

__Moving at the speed of dark!__

![Speedtest results via fast.com](/assets/images/speedtest-via-fast.png)


As you can tell from the screenshot, this is pretty slow in todays standards. To put this speed in perspective, I get about four times this speed from a cellular data connection. Download the latest binaries for macOS on the Intel Architecture. At the time of writing this, the latest version that I was able to find on the iperf website was 3.13.
```
cd ~/Downloads
curl https://iperf.fr/download/apple/iperf-3.1.3-macos-x86_64.zip >> iperf-3.1.3-macos-x86_64.zip
```

__Verify the file signatures__

```
curl https://iperf.fr/download/apple/iperf-3.1.3-macos-x86_64.zip >> iperf-3.1.3-macos-x86_64.zip
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 51178  100 51178    0     0  54803      0 --:--:-- --:--:-- --:--:-- 54794
[~/Downloads] $ curl https://iperf.fr/download/apple/sha256sum.txt |grep 3.1.3 |cut -d ' ' -f 1
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   472  100   472    0     0    755      0 --:--:-- --:--:-- --:--:--   755
5fd67740d35fdc3fe2554fef269b25df0d39f1e58fa1c645c996f4d5298fa051
[~/Downloads] $ shasum -a 256 iperf-3.1.3-macos-x86_64.zip
5fd67740d35fdc3fe2554fef269b25df0d39f1e58fa1c645c996f4d5298fa051  iperf-3.1.3-macos-x86_64.zip
```

__Unzip the binary__
```
[~/Downloads] $ unzip iperf-3.1.3-macos-x86_64.zip
Archive:  iperf-3.1.3-macos-x86_64.zip
  inflating: iperf3
```

Now that the binary is unzipped you can begin testing. You will need to have one running as a server and one running as a client to perform the performance tests. In this instance I already have iperf installed on a Linux machine on the other end so I will ssh into the machine and start the iperf server using the -s flag.

__Start the iperf server__
```
[~]$ iperf3 -s
-----------------------------------------------------------
Server listening on 5201
-----------------------------------------------------------
```

Once the server is listening on one end, you can begin testing using the -c option on the client machine. The syntax is iperf -c address of server.

__Point the client to the server__  

```
[~/Downloads] $ ./iperf3 -c 172.28.239.1
Connecting to host 172.28.239.1, port 5201
[  4] local 172.28.239.6 port 53878 connected to 172.28.239.1 port 5201
[ ID] Interval           Transfer     Bandwidth
[  4]   0.00-1.00   sec   439 KBytes  3.59 Mbits/sec                  
[  4]   1.00-2.00   sec   512 KBytes  4.20 Mbits/sec                  
[  4]   2.00-3.00   sec   542 KBytes  4.43 Mbits/sec                  
[  4]   3.00-4.00   sec   667 KBytes  5.47 Mbits/sec                  
[  4]   4.00-5.00   sec   816 KBytes  6.69 Mbits/sec                  
[  4]   5.00-6.00   sec  1004 KBytes  8.22 Mbits/sec                  
[  4]   6.00-7.00   sec  1.16 MBytes  9.76 Mbits/sec                  
[  4]   7.00-8.00   sec  1.60 MBytes  13.4 Mbits/sec                  
[  4]   8.00-9.00   sec  1.55 MBytes  13.0 Mbits/sec                  
[  4]   9.00-10.00  sec  1.38 MBytes  11.6 Mbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bandwidth
[  4]   0.00-10.00  sec  9.57 MBytes  8.03 Mbits/sec                  sender
[  4]   0.00-10.00  sec  9.41 MBytes  7.89 Mbits/sec                  receiver

iperf Done.
```
