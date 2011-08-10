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

### Virtualbox:

Turn off native virtualbox dhcp server.  
Configure virtualbox's virtual network adapter to have static ip address 10.1.0.254 and netmask 255.255.255.0.  
Configure target virtual machine network to 'host virtual adapter'.  

### Configure dnsmasq:

uncomment following line in `/etc/dnsmasq.conf`:

> conf-dir=/etc/dnsmasq.d

and add following to `/etc/dnsmasq.d/dhcp.conf`:

> interface=vboxnet0  
> dhcp-range=10.1.0.1,10.1.0.10,12h  
> log-dhcp    # for debugging  

By default dnsmasq will log debug info `/var/log/syslog`, look there if you run into trouble.  
run on virtual machine:
{% highlight bash %}
sudo dhclient eth0
{% endhighlight %}

and see ip there

{% highlight bash %}
ip a
{% endhighlight %}
> <pre>
> 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue state UNKNOWN
>     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
>     inet 127.0.0.1/8 scope host lo
>     inet6 ::1/128 scope host
>        valid_lft forever preferred_lft forever
> 2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
>     link/ether 08:00:27:0b:f4:44 brd ff:ff:ff:ff:ff:ff
>     inet 10.1.0.9/24 brd 10.1.0.255 scope global eth0
>     inet6 fe80::a00:27ff:fe0b:f444/64 scope link
>        valid_lft forever preferred_lft forever</pre>

and in syslog:

> dnsmasq-dhcp[16896]: 1660038144 available DHCP range: 10.1.0.1 -- 10.1.0.10  
> dnsmasq-dhcp[16896]: 1660038144 DHCPREQUEST(vboxnet0) 10.1.0.9 08:00:27:0b:f4:44   
> dnsmasq-dhcp[16896]: 1660038144 DHCPACK(vboxnet0) 10.1.0.9 08:00:27:0b:f4:44   
> dnsmasq-dhcp[16896]: 1660038144 requested options: 1:netmask, 28:broadcast, 2:time-offset, 3:router,   
> dnsmasq-dhcp[16896]: 1660038144 requested options: 15:domain-name, 6:dns-server, 119:domain-search,   
> dnsmasq-dhcp[16896]: 1660038144 requested options: 12:hostname, 44:netbios-ns, 47:netbios-scope,   
> dnsmasq-dhcp[16896]: 1660038144 requested options: 26:mtu, 121:classless-static-route, 42:ntp-server  
> dnsmasq-dhcp[16896]: 1660038144 tags: vboxnet0  
> dnsmasq-dhcp[16896]: 1660038144 next server: 10.1.0.254  
> dnsmasq-dhcp[16896]: 1660038144 sent size:  1 option: 53:message-type  05  
> dnsmasq-dhcp[16896]: 1660038144 sent size:  4 option: 54:server-identifier  10.1.0.254  
> dnsmasq-dhcp[16896]: 1660038144 sent size:  4 option: 51:lease-time  00:00:a8:c0  
> dnsmasq-dhcp[16896]: 1660038144 sent size:  4 option: 58:T1  00:00:54:60  
> dnsmasq-dhcp[16896]: 1660038144 sent size:  4 option: 59:T2  00:00:93:a8  
> dnsmasq-dhcp[16896]: 1660038144 sent size:  4 option:  1:netmask  255.255.255.0  
> dnsmasq-dhcp[16896]: 1660038144 sent size:  4 option: 28:broadcast  10.1.0.255  
> dnsmasq-dhcp[16896]: 1660038144 sent size:  4 option:  3:router  10.1.0.254  
> dnsmasq-dhcp[16896]: 1660038144 sent size:  4 option:  6:dns-server  10.1.0.254  

now we can connect to virtual machine with ssh, and vice versa

### Configure internet access for virtual machine(UNDONE FROM HERE)

Enable ip forwarding:

{% highlight bash %}
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
{% endhighlight %}

or uncomment line `net.ipv4.ip_forward = 1` in `/etc/sysctl.conf`  
and run:
{% highlight bash %}
sudo sysctl -p /etc/sysctl.conf
{% endhighlight %}

iptables:
{% highlight bash %}
sudo iptables -I FORWARD -i vboxnet0 -d 10.1.0.0/255.255.255.0 -j DROP
sudo iptables -A FORWARD -i vboxnet0 -s 10.1.0.0/255.255.255.0 -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -d 10.1.0.0/255.255.255.0 -j ACCEPT
sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
{% endhighlight %}

and lets save our settings into file:

{% highlight bash %}
sudo mkdir /etc/iptables
sudo iptables-save | sudo tee /etc/iptables/simple.router.rules
{% endhighlight %}

the contents of `/etc/iptables/simple.router.rules`:

> <pre>
> TODO
> </pre>

now internet on virtual machine should work
