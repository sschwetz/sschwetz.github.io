---
categories: Knowledge-Base
images: /assets/img
typora-copy-images-to: ./assets/img

lang: en-AU
layout: wide
title: Logs fail to rotate on /var/log/vmware on NSX-T Manager and Edge Nodes
subtitle: KB0001
author: Stephen Schwetz
tags: 
  - KB0001
  - KB
  - Knowledge
  - VMWare 
  - NSX
  - Edge Nodes	
  - Knowledge Base
updated: 2023-10-07

---



## Symptoms

* You are running NSX-T 3.1.
* You may see the partition '/var/log/vmware'  grow in size.
* The following files may grow quite large:
  * integrity_checker.log
  * top-mem.log
  * top-cpu.log

If you run the command: 

``` bash 
/usr/sbin/logrotate -d /etc/logrotate.conf 2>&1 | less
```

The following errors are displayed

```
error: appliance-config:3 bad size '12.8'
error: appliance-config:17 bad size '51.2'
```

Note: The command ```/usr/sbin/logrotate -d /etc/logrotate.conf 2>&1 | less```  is a debug command and will only read the configuration file to check its correctness.

## Cause

This issue is caused due to a extra decimal point in the configuration file. This converts the number to a float, which the code does not accept.

## Resolution

This issue is resolved in NSX T 3.1.1 and onwards. 

## Workaround

Log in as root on the impacted NSX-T Manager or Edge node.

``` bash
cd /etc/logrotate.d

#Then copy the original file as a backup: 
cp appliance-config /tmp/appliance-config.bak

# replace with correct values, run the following command:
sed -i 's/12.8M/13M/g' appliance-config
sed -i 's/51.2M/52M/g' appliance-config

# remove 'su syslog adm' line from integrity_checker.log file rotate config section:
sed -i '/su syslog adm/d' appliance-config
```

No reboot or service restarts are required as the next time logrotate is called by cron it will use these new values

## Further Reading

[VMWare KB 82273](https://kb.vmware.com/s/article/82273)
