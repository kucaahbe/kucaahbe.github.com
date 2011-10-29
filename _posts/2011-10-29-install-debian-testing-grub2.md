---
layout: post
title: Install debian testing from Grub 2

---

### Download

{% highlight bash %}
cd /boot
wget ftp://ftp.debian.org/debian/dists/testing/main/installer-i386/current/images/netboot/debian-installer/i386/linux
wget ftp://ftp.debian.org/debian/dists/testing/main/installer-i386/current/images/netboot/debian-installer/i386/initrd.gz
{% endhighlight %}

### setup grub2

add following into `/etc/grub.d/40_custom`:

{% highlight bash %}
#!/bin/sh
exec tail -n +3 $0
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.

menuentry "debian testing install" {
set root=(hd0,1)
linux /boot/linux
initrd /boot/initrd.gz
}
{% endhighlight %}

reboot and enjoy
