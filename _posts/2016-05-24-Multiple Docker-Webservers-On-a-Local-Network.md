---
layout: post
title:  "Multiple Docker Web Servers on a Local Network"
date:   2016-05-24 15:07:19
categories: Tutorial
comments: true
tags: Docker Linux Debian IPAliasing
---
If you have been using Docker to host web services, soon you will run into a problem. What do you do when multiple containers need port 80? Well in this post I will be discussing this scenario in a local network perspective.
Say you want to run 2 Apache Containers both running on port 80. You will get this error when starting the second container: 
```Error starting userland proxy: listen 
tcp 0.0.0.0:80: bind: address already in use.```
My simple solution *IP Aliasing*
<!--more-->

Whats is IP Aliasing?
It's the concept of assigning multiple IP address's to one NIC. In this scenario you are in a local network, so getting static IP Address's is not an issue. In this example my host distribution is Debian Jessie.
To modify network interfaces in a Debian based distribution you have to edit: ```/etc/network/interfaces```.

Sample Default Interfaces config:
{% highlight bash %}
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet dhcp

{% endhighlight %}

First problem here is the fact eth0 is using DHCP. Change eth0 to static.

{% highlight bash %}
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet static
	address 192.168.1.100
	netmask 255.255.255.0
	gateway 192.168.1.1

{% endhighlight %}

Obviously the address, netmask, and gateway, will vary on your network.
Now if you want to run 2 containers on separate IP's your going to have to add a second IP address, this is where IP Aliasing comes in.

{% highlight bash %}
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet static
	address 192.168.1.100
	netmask 255.255.255.0
	gateway 192.168.1.1

# Aliased network interface
auto eth0:0
iface eth0:0 inet static
	address 192.168.1.101
	netmask 255.255.255.0

{% endhighlight %}

Now you don't have to stop there, you can go add 15 of those suckers as long as you have the address space. Each alias counting upwards such as ```eth0:15```.

By the way, you will need to restart the Networking Service for these to take effect. You can do that by running: ```systemctl restart networking.service```

Now you have to tell Docker which alias to use. You can do that within the Docker Run command. Specifically the port flag (-p). Heres an example of running Apache on a alias:
```docker run -p 192.168.1.100:80:80 httpd```

Pretty easy huh. So just remember ```Alias IP: Extenal Port: Internal Port```.

Soon I will be writing a post on the other scenario multiple web services externally with a reverse proxy.

*Stay Tuned In!* - Aidan
