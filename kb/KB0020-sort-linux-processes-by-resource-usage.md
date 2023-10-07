---
images: /assets/img
typora-copy-images-to: /assets/img
layout: page
title: Sort Linux Processes by Resource Usage
subtitle: KB0020
updated: 2023-10-07
tags: 
  - KB0020
  - NSX
  - Edge
  - Manager
  - Knowledge Base
  - KB
---

## Usage**

`./python3.6 sum_process_resources.py`

### **Output**

```
0. /opt/atom/atom | 27.8
1. /usr/lib/chromium-browser/chromium-browser | 11.2
2. python3.6 | 11.0
3. /opt/google/chrome/chrome | 1.6
4. /usr/lib/xorg/Xorg | 1.4
5. /opt/Franz/franz | 0.7

==  MEM%  ==

0. /usr/lib/chromium-browser/chromium-browser | 37.2
1. /opt/google/chrome/chrome | 11.3
2. /opt/Franz/franz | 10.6
3. /opt/atom/atom | 10.1
4. /usr/lib/xorg/Xorg | 2.0
5. com.google.android.gms.persistent | 1.4

==  RSS MB  ==

0. /usr/lib/chromium-browser/chromium-browser | 1475.07 MB
1. /opt/google/chrome/chrome | 461.35 MB
2. /opt/Franz/franz | 429.04 MB
3. /opt/atom/atom | 402.18 MB
4. /usr/lib/xorg/Xorg | 78.53 MB
5. com.google.android.gms.persistent | 58.02 MB

```

## Code

```bash
#sum_process_resources.py
from collections import OrderedDict
import subprocess

def run_cmd(cmd_string):
    """Runs commands and saves output to variable"""
    cmd_list = cmd_string.split(" ")
    popen_obj = subprocess.Popen(cmd_list, stdout=subprocess.PIPE)
    output = popen_obj.stdout.read()
    output = output.decode("utf8")
    return output
    
def sum_process_resources():
    """Sums top X cpu and memory usages grouped by processes"""
    ps_memory, ps_cpu, ps_rss = {}, {}, {}
    top = 6
    output = run_cmd('ps aux').split("\n")
    for i, line in enumerate(output):
        cleaned_list = " ".join(line.split())
        line_list = cleaned_list.split(" ")
        if i > 0 and len(line_list) > 10:
            cpu = float(line_list[2])
            memory = float(line_list[3])
            rss = float(line_list[5])
            command = line_list[10]
            ps_cpu[command] = round(ps_cpu.get(command, 0) + cpu, 2)
            ps_memory[command] = round(ps_memory.get(command, 0) + memory, 2)
            ps_rss[command] = round(ps_rss.get(command, 0) + rss, 2)
    sorted_cpu = OrderedDict(sorted(ps_cpu.items(), key=lambda x: x[1], reverse=True))
    sorted_memory = OrderedDict(sorted(ps_memory.items(), key=lambda x: x[1], reverse=True))
    sorted_rss = OrderedDict(sorted(ps_rss.items(), key=lambda x: x[1], reverse=True))
    print("====   CPU%   ====")
    for i, k in enumerate(sorted_cpu.items()):
        if i < top:
            print("{}. {} | {}".format(i, k[0], k[1]))
    print("====   MEM%   ====")
    for i, k in enumerate(sorted_memory.items()):
        if i < top:
            print("{}. {} | {}".format(i, k[0], k[1]))
    print("====   RSS MB   ====")
    for i, k in enumerate(sorted_rss.items()):
        if i < top:
            print("{}. {} | {} MB".format(i, k[0], round((k[1]/1024), 2)))

if __name__ == '__main__':
    sum_process_resources()
#sum_process_resources.py
from collections import OrderedDict
import subprocess

def run_cmd(cmd_string):
    """Runs commands and saves output to variable"""
    cmd_list = cmd_string.split(" ")
    popen_obj = subprocess.Popen(cmd_list, stdout=subprocess.PIPE)
    output = popen_obj.stdout.read()
    output = output.decode("utf8")
    return output

def sum_process_resources():
    """Sums top X cpu and memory usages grouped by processes"""
    ps_memory, ps_cpu, ps_rss = {}, {}, {}
    top = 6
    output = run_cmd('ps aux').split("\n")
    for i, line in enumerate(output):
        cleaned_list = " ".join(line.split())
        line_list = cleaned_list.split(" ")
        if i > 0 and len(line_list) > 10:
            cpu = float(line_list[2])
            memory = float(line_list[3])
            rss = float(line_list[5])
            command = line_list[10]
            ps_cpu[command] = round(ps_cpu.get(command, 0) + cpu, 2)
            ps_memory[command] = round(ps_memory.get(command, 0) + memory, 2)
            ps_rss[command] = round(ps_rss.get(command, 0) + rss, 2)
    sorted_cpu = OrderedDict(sorted(ps_cpu.items(), key=lambda x: x[1], reverse=True))
    sorted_memory = OrderedDict(sorted(ps_memory.items(), key=lambda x: x[1], reverse=True))
    sorted_rss = OrderedDict(sorted(ps_rss.items(), key=lambda x: x[1], reverse=True))
    print("====   CPU%   ====")
    for i, k in enumerate(sorted_cpu.items()):
        if i < top:
            print("{}. {} | {}".format(i, k[0], k[1]))
    print("====   MEM%   ====")
    for i, k in enumerate(sorted_memory.items()):
        if i < top:
            print("{}. {} | {}".format(i, k[0], k[1]))
    print("====   RSS MB   ====")
    for i, k in enumerate(sorted_rss.items()):
        if i < top:
            print("{}. {} | {} MB".format(i, k[0], round((k[1]/1024), 2)))


if __name__ == '__main__':
    sum_process_resources()
```



