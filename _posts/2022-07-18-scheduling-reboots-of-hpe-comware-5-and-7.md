---
typora-copy-images-to: /assets/img
typora-root-url: ..

lang: en-AU
layout: post
title: Scheduling Reboots of HPE Comware 5 and 7
subtitle: HPE's reload in 5
tags: [comware,hpe,technical,2022,july]
comments: true
redirect_from:
  - /stephen/scheduling-reboots-of-hpe-comware-5-and-7
  - /2022/07/18/scheduling-reboots-of-hpe-comware-5-and-7/
  - /2022/07/18/scheduling-reboots-of-hpe-comware-5-and-7
---

Any network engineer worth their salt will be aware of the cisco “*reload in 10″* command that allows you to reboot a Cisco device during configuration if you lose comms with it. Comware devices offer the same feature but activating it is a bit different between Comware 5 and Comware 7.

For HPE Comware 5, use the following format:

```
<10500>schedule reboot delay 10
Reboot system at 10:41 23/06/2022(in 0 hour(s) and 10 minute(s)). confirm? [Y/N]:Y
<10500>display schedule reboot
System will reboot at 10:41 23/06/2022 (in 0 hours and 9 minutes).
<10500>undo schedule reboot
<10500>display schedule reboot 
<10500>
```

In Comware 7 the schedule command has been renamed to scheduler. Other than this change, the syntax is identical.

```
5940>scheduler reboot delay 10
Reboot system at 10:41 23/06/2022(in 0 hour(s) and 10 minute(s)). confirm? [Y/N]:Y
5940>display scheduler reboot
System will reboot at 10:41 23/06/2022 (in 0 hours and 9 minutes).
5940>undo scheduler reboot
5940>display scheduler reboot 
5940>
```

