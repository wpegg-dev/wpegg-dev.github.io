---
layout: post
title:  "Running A Web Server on Raspberry Pi 2"
date:   2015-03-21 11:49:20
categories: project
---
If you have not heard of the [Raspberry Pi][Raspberry-Pi-2], you should absolutely look it up. It is a really neat, small, inexpensive Linux computer. 
There is a pretty large community out there for making cool projects and device configurations. They recently came out with a new version, the Raspberry Pi 2, which is said to be able to host Windows 10. 
Setting up a web server on the device is fairly simple since it is based on the Linux kernel. Raspberry Pi comes with several different OS's that are compatible with the device. 
They also recently started using a new bootloader called [NOOBS][noobs] which will allow users to install a specific OS. I decided with my configuration to use a 64GB microsd card to install the OS on. 
(Raspberry Pi uses microsd as the "hard drive") I ran into a problem when booting that caused the PI not to be able to read the card. 
The activity LED on the device stayed lit which indicates that the device cannot read the card. After about 45 minutes of researching it turned out to be a pretty simple fix. 
When you are setting up the SD card it is recommended that you reformat the card to FAT32. The problem is that when windows tries to format something over 32GB, it will try to format it as exFAT. 
This caused it to be unreadable by the device. I found [this article][help-article] that helped explain how to correct the issue and the Pi was finally able to boot up. 
After getting Raspian installed, I began installing all the software required for the web server. There is a great step-by-step guide you can follow [here][guide] is you are unfamiliar with setting up a web server on a Pi. 
I only ran into on problem and it was at step 7. Java wasn't included in the OS install that I was running. I was able to fix the problem by running the following command

{% highlight ruby %}
sudo apt-get install default-jdk
{% endhighlight %}

After finishing the guide I installed the Apache Tomcat Manager client to help make it easier to deploy web applications to the server. (For more information on Apache Tomcat Manager click here) 
I have had experience with this in the past and find it a very useful tool when deploying web applications to a linux server. [Here][apache-manager] is a good guide to follow for installing Apache Tomcat Manager on your server. 
Well that's all for today's post. Hopefully this will help anyone out there who might be having a problem setting up a web server on their Raspberry Pi 2

[Raspberry-Pi-2]:	http://www.raspberrypi.org/
[noobs]:   			http://www.raspberrypi.org/downloads/
[help-article]: 	http://forum.xda-developers.com/showthread.php?t=1773735
[guide]: 			http://www.instructables.com/id/Raspberry-Pi-Web-Server/?ALLSTEPS
[apache-manager]: 	https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-7-on-ubuntu-14-04-via-apt-get