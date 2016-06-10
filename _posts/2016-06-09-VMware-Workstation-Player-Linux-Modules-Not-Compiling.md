---
layout: post
title:  "VMware Workstation/Player Kernel Modules Not Compiling After Linux Kernel 4.6"
date:   2016-06-09 18:32:0
categories: Solution
comments: true
tags: Linux Arch VMware
---
Recently, I updated my laptop to Linux Kernel 4.6. After doing so I opened VMware Workstation, a tool that I need for my daily work flow. It started up with a prompt to recompile Kernel modules. I expected this prompt it's always required after a kernel update. Instead of compiling the necessary modules it fails. So I decided to run it from terminal to get a more verbose error. As it turns it out, it's failing on compiling two specific modules. The VMware Monitoring service module and the VMware Networking module. After more analysis it turned out to be two specific files inside the modules. The first file located in the Monitoring Module is ```hostif.c``` and the second file located in the Networking module is ```userif.c```. The root cause is a function name change. ```get_user_pages()``` has been changed to ```get_user_pages_remote()```. How do we fix this?
<!--more-->

_Note: You will need Root Privileges to modify these files_

* Navigate to the directory VMware stores modules ```/usr/lib/vmware/modules/source```
* Make a backup of this directory in case something goes wrong
* Untar ```vmmon.tar``` by running ```tar -xvf vmmon.tar```
* Open ```/vmmon-only/linux/hostif.c``` in the newly created directory
* Change line 1165 from {% highlight c %}retval = get_user_pages(current, current->mm, (unsigned long)uvAddr,{% endhighlight %} to {% highlight c %}retval = get_user_pages_remote(current, current->mm, (unsigned long)uvAddr,{% endhighlight %}
* Save the file and recreate ```vmmon.tar``` with the newly modified file by running ```tar -cvf vmmon.tar /vmmon-only```
* Untar ```vmnet.tar``` by running ```tar -xvf vmnet.tar```
* Open ```/vmnet-only/userif.c``` in the newly created directory
* Change line 116 from {% highlight c %}retval = get_user_pages(current, current->mm, addr,{% endhighlight %} to {% highlight c %}retval = get_user_pages_remote(current, current->mm, addr,{% endhighlight %}
* Save the file and recreate ```vmnet.tar``` with the newly modified file by running ```tar -cvf vmnet.tar /vmmnet-only```
* Start VMware Workstation and let the prompt imitate compiling or run ```vmware-modconfig --console --install-all```

Now you should be good to go. Hopefully VMware patches this soon. If you have any concerns feel free to comment.
