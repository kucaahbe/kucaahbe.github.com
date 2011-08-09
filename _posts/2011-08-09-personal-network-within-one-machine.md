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

1. Turn of native virtualbox dhcp server
2. Configure virtualbox's virtual network adapter to have static ip address 10.1.0.254 and netmask 255.255.255.0
3. configure target virtual machine network to 'host virtual adapter'
4. configure dnsmasq:
   1. incomment following line in /etc/dnsmasq.conf:

        conf-dir=/etc/dnsmasq.d
   2. and add following to /etc/dnsmasq.d/dhcp

        interface=vboxnet0
        dhcp-range=10.1.0.1,10.1.0.10,12h
        log-queries # for debugging
        log-dhcp    # for debugging
   3. by default dnsmasq will log debug info /var/log/syslog, look there if you run into trouble
   4. run on virtual machine

        sudo dhclient eth0
   5. and see ip there
