+++
title = "Review for cheapest instance on google cloud platform"
date = "2015-10-22T15:44:00+08:00"
#Modified: 2015-10-22 15:44
categories = ["Cloud Service"]
tags = ["cloud service", "google compute engine"]
#Slug: google-compute-engine-review
#Author: elprup
#Summary: review for cheapest google compute engine instance
+++

Compute Engine in google cloud platform provide cloud instance service. I'd like to create new instance for my blog and shadow sock. Here we go.

## Create one instance

Google provide a clear console for create instance:

1. You create a project first, then you can create your instance in VM instances label page. Choose f1.micro machine type. (Cheapest)
2. After create finished, there's a button named SSH at the top of instance detail info.
3. Now you can click `Open in browser window` to open a console window.

## More info about instance

First let's just check some info about my cheapest (though it's free) instance.

```
# top
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:    603408 total,   308216 used,   295192 free,     9092 buffers

# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       9.9G  1.3G  8.1G  14% /
```

1 core, 0.6GB memory, 10GB disk.  
Try to reach china website (instance is us central)

```
awayyao@instance-1:~$ ping www.baidu.com
PING www.a.shifen.com (103.235.46.39) 56(84) bytes of data.
64 bytes from 103.235.46.39: icmp_seq=1 ttl=52 time=176 ms
64 bytes from 103.235.46.39: icmp_seq=2 ttl=52 time=176 ms
64 bytes from 103.235.46.39: icmp_seq=3 ttl=52 time=176 ms
```

of cause google is not blocked here

```
awayyao@instance-1:~$ ping www.google.com
PING www.google.com (74.125.135.103) 56(84) bytes of data.
64 bytes from jh-in-f103.1e100.net (74.125.135.103): icmp_seq=1 ttl=53 time=1.05 ms
64 bytes from jh-in-f103.1e100.net (74.125.135.103): icmp_seq=2 ttl=53 time=0.877 ms
```

we can test download speed(refer to this [link](http://lifehacker.com/how-to-test-your-internet-speed-with-a-terminal-command-1364123567)):

```
awayyao@instance-1:~$ wget -O /dev/null http://speedtest.wdc01.softlayer.com/downloads/test10.zip
--2015-10-22 06:13:22--  http://speedtest.wdc01.softlayer.com/downloads/test10.zip
Resolving speedtest.wdc01.softlayer.com (speedtest.wdc01.softlayer.com)... 208.43.102.250, 2607:f0d0:3001:78::2
Connecting to speedtest.wdc01.softlayer.com (speedtest.wdc01.softlayer.com)|208.43.102.250|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11536384 (11M) [application/zip]
Saving to: ‘/dev/null’
100%[=======================================================================>] 11,536,384  16.0MB/s   in 0.7s   
2015-10-22 06:13:23 (16.0 MB/s) - ‘/dev/null’ saved [11536384/11536384]
```

So fast, 100Mbps at least.

That's the first look for cheapest ( less than $5 per month ) instance on google cloud. 
