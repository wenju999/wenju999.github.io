---
title: Common Linux Commands
author: zjpzhao
date: 2021-05-09 20:19:00 +0800
categories: [Linux]
tags: [Ubuntu,Shell,TODO]
---

## Understand a Command
1. `$ man command` for manual pages
2. `$ command --help` for help

---

## Run at Background
The `screen` command launches a terminal in the background which can be detached from and then reconnected to. You can start a screen, kick off a command, detach from the screen, and log out. You can then log in later and reattach to the screen and see the program running.

- startup  
    `$ screen -S XXX.py`
- detach  
    `Ctrl + a + d`

- reattach  
    `$ screen -r`

>from <https://access.redhat.com/articles/5247>

---


## Monitor Resources  
- **Network Bandwidth**  
    `$ nload`
    > Other 17 commands：<https://www.binarytides.com/linux-commands-monitor-network/>

- **CPU**  
    `$ top`

- **GPU**  
    `$ nvidia-smi`

- **Disk**  
  1. `$ du -h`
  2. `$ df -h`

- **Continuous Monitoring**  
    `$ watch -n 0.5 nvidia-smi` 

---

## Rename files in batches
`$ rename [options] "s/oldname/newname/" file`  
### Command ***rename***'s options and parameters:
![rename result]({{ site.url }}/assets/img/2021-05-09-Linux-utils/2021-05-09-23-01-31.png){: width="450" .right}  
***-n*** preview result without execute  
(directly execute if without option [-n])  
***s*** stands for 'substitution'  
**[()]** Represents matching the content in []  
**//** The empty space between the two slashes means replacing the empty content, equivalent to deleting  
**g** means all matches, only one bracket will be matched by default if without it  
**^-** add characters at the beginning of the file name  

Other rules：<https://www.cnblogs.com/mianbaoshu/p/11772876.html>  
Perl regular expressions：<https://perldoc.perl.org/perlre>


---

