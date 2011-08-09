---

layout: post
title: Setup personal local network within one machine

---
Expected result
---------------

         +----------------------+    +----------+
         |    real machine      |    |          |
         |(dhcp and dns server) |<-->| internet |
         |      10.1.0.254      |    |          |
         +----------------------+    +----------+
                   /\                     /\
                   ||                     ||
                   \/                     ||
         +----------------------+         ||
         |   virtual machine    |         ||
         | (ip set up by dhcp   |         ||
         |    server above)     |<========//
         |      10.1.0.1        |
         +----------------------+

somethig like this I guess... :)

Installing needed software
--------------------------

We need:

1. [virtualbox](http://www.virtualbox.org/)
2. [dnsmasq](http://thekelleys.org.uk/dnsmasq/doc.html) (it will be our dns and dhcp server)

and we install:
{% highlight bash %}
wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -
sudo aptitude install virtualbox-4.1 dnsmasq
{% endhighlight %}

Configuring
-----------

1. Turn of native virtualbox dhcp server()
2. Configure virtualbox's virtual network adapter to have static ip address 10.1.0.254 and netmask 255.255.255.0
