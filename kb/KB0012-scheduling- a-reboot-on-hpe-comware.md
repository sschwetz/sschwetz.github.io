---
categories: Knowledge-Base
images: /assets/
typora-copy-images-to: /assets/


lang: en-GB
layout: page
full-width: true
subtitle: Scheduling a Eeboot on HPE Comware
title: KB0012
author: Stephen Schwetz
maintainer: 
subject: 
tags: 
  - KB0012
  - NSX
  - Edge
  - Manager
  - Knowledge Base
  - KB
---



Most network engineers who are familiar with Cisco equipment are aware that you can schedule a reboot (reload). HPE Comware offers this facility in current v5 and v7 release trains.

## Comware 5

For Comware, the command used is `schedule`:

* To reboot a system at a specific time, the command is `schedule reboot at hh:mm`.

* To reboot a system after an elapsed time interval, the command  is`schedule reload delay [ mm | hh:mm]` where hh is a placeholder for hours and mm is the placeholder for minutes

You can use the command `display schedule reboot` to display the status of any scheduled reboots.

To cancel a scheduled reboot, use `undo schedule reboot`

Below is an example of what is displayed on the command line when interacting with a schedule.

``` 
<HP_TEST-SW-5120>schedule reboot delay 10
Reboot system at 08:21 01/24/2015(in 0 hour(s) and 10 minute(s)). confirm? [Y/N]:Y
<HP_TEST-SW-5120>display schedule reboot
System will reboot at 08:21 01/24/2015 (in 0 hours and 9 minutes).
<HP_TEST-SW-5120>undo schedule reboot
<HP_TEST-SW-5120>display schedule reboot
<HP_TEST-SW-5120>
```

## Comware 7

With the advent of Comware 7, HPE changed the schedule command, and it is now `scheduler`. The usage of it is very similar:

* To reboot a system at a specific time, the command is `scheduler reboot at hh:mm`

* To reboot a system after an elapsed time interval, the command  is`scheduler reload delay [ mm | hh:mm]` where hh is a placeholder for hours and mm is the placeholder for minutes

* You can use the command `display schedule reboot` to display the status of any scheduled reboots.

* To cancel a scheduled reboot, use `undo schedule reboot`