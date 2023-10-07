---
layout: wide
categories: Knowledge-Base
images: /assets/img
typora-copy-images-to: /assets/img

title: Using iperf3
subtitle: KB0014
updated: 2023-10-07
cover-img: assets/img/logo_iperf.png
tags: 
  - KB0014
  - NSX
  - Edge
  - Manager
  - Knowledge Base
  - KB
---

## What is iperf?

iPerf3 is a tool for actively measuring the maximum achievable bandwidth on IP networks. It supports tuning various parameters related to timing, buffers and protocols (TCP, UDP, SCTP with IPv4 and IPv6). It reports the bandwidth, loss, and other parameters for each test. 

**Note:** This new implementation shares no code with the original iPerf and is not backward compatible. iPerf was initially developed by [NLANR/DAST](https://iperf.fr/contact.php#authors). iPerf3 is principally developed by [ESnet](https://www.es.net/) / [Lawrence Berkeley National Laboratory](https://www.lbl.gov/).

To run iperf3, you need two computers to run the software. As the software utilises an ephemeral port, it can be run without root access, though it may require root access to install depending on your operating system

## Starting the Server

The first step is to start iperf3 on the computer that is your server, after ensuring that no firewall is active enter the command below.

```bash
iperf3 -s

—————————————————————————————————————————
 Server listening on 5201
—————————————————————————————————————————
```

By default, iperf will listen on port 5201, but you can change this by using the `-p` option followed by the port

```bash
iperf3 -s -p 12345

—————————————————————————————————————————
 Server listening on 12345
—————————————————————————————————————————
```

## Starting the Client

### Standard Mode

With iperf3 in its standard mode of operation, the client sends data towards the server. If you are using TCP it is recommended to use more than one thread, otherwise a single thread may not max out your network.

````bash
iperf3 -с 172.28.22.101 -p 5201 -f m -z -b 1000M -P 10
````

This command will do the following

* run as a client <kbd>-c</kbd>;
* connect to the host 172.28.22.101;
* connect on port 52011 <kbd>-p</kbd>;
* give the speed readings in megabits <kbd>-f m</kbd>;
* use "zero copy" to lower CPU usage <kbd>-Z</kbd> ;
* each connection will send 1000Megabits/sec <kbd>-b</kbd>  `1000M`; and
* it will use 10 processes (thus send 10GB/s) <kbd>-P</kbd> `10`; and

### Reverse Mode

* if you want to have the server send traffic to the client the CLI option <kbd>-R</kbd>` will "Reverse the traffic flow".