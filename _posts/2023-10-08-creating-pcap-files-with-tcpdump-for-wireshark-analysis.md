---
categories: Knowledge-Base
layout: page
full-width: true

typora-copy-images-to: /assets/img
typora-root-url: ../

lang: en-AU

title: KB0013
subtitle: Creating .pcap Files with tcpdump for Wireshark Analysis
author: stephen
tags: 
  - KB0014
  - Knowledge Base
  - wireshark
  - pcap
  - tcpdump
updated: 


---

It’s often more helpful to capture packets using `tcpdump` rather than `wireshark`, as it is available as a package in most Linux and BSD package managers. For example, you might want to do a remote capture and either don’t have GUI access or don’t have Wireshark installed on the remote machine.

Older versions of `tcpdump` truncate packets to 68 or 96 bytes. If this is the case, use <kbd>-s</kbd> to capture full-sized packets:

```bash
$ tcpdump -i <interface> -s 65535 -w <file>
```

You must specify the correct *interface* and file name to save into. In addition, you will have to terminate the capture with <kbd>Ctrl+C</kbd> when you believe you have captured enough packets.
