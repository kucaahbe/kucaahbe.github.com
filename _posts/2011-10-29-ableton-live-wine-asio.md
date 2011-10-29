---
layout: post
title: bla

---

### sound setup

{% highlight bash %}
sudo aptitude install alsa alsa-tools
sudo adduser user audio
{% endhighlight %}

### install wine

{% highlight bash %}
mkdir wine-unstable
cd wine-unstable
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-alsa-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-bin-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-capi-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-cms-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-dbg-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-dev-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-gl-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-gphoto2-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-ldap-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-openal-unstable_1.3.31-0.1_i386.deb
#wget http://dev.carbon-project.org/debian/wine-unstable/libwine-oss-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-print-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-sane-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/libwine-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/wine-bin-unstable_1.3.31-0.1_i386.deb
wget http://dev.carbon-project.org/debian/wine-unstable/wine-unstable_1.3.31-0.1_i386.deb

sudo aptitude install libgstreamer-plugins-base0.10-0 libgstreamer0.10-0 libhal1 libmpg123-0 libsane liblcms1 libcapi20-3 libgettextpo0 libwine-gecko-unstable

sudo dpkg -i libwine-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i libwine-bin-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i wine-bin-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i libwine-alsa-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i libwine-gl-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i libwine-print-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i libwine-sane-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i libwine-cms-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i libwine-gphoto2-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i libwine-ldap-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i libwine-openal-unstable_1.3.31-0.1_i386.deb
sudo dpkg -i wine-unstable_1.3.31-0.1_i386.deb
{% endhighlight %}

### setup wine

as user
{% highlight bash %}
winecfg
{% endhighlight %}

##### Resources

* [install wine unstable in debian](http://dev.carbon-project.org/debian/wine-unstable/)
* [Ableton wine page](http://appdb.winehq.org/objectManager.php?sClass=application&iId=2113)
* [wine ppa repository](https://launchpad.net/~ubuntu-wine/+archive/ppa)
