---
layout: post
title: Installing TemCity 6.5 on debian stable(tomcat6/nginx)

---

Prepare:
-------

{% highlight bash %}
sudo aptitude install tomcat6 nginx
{% endhighlight %}

Get TemCity:
------------

{% highlight bash %}
wget http://download.jetbrains.com/teamcity/TeamCity-6.5.4.war
{% endhighlight %}

Initial setup:
-------------

creating [<TeamCity Data directory>](http://confluence.jetbrains.net/display/TCD65/TeamCity+Data+Directory)

{% highlight bash %}
mkdir /var/lib/teamcity-6.5
ln -s /var/lib/teamcity-6.5 /var/lib/teamcity
chown tomcat6:tomcat6 /var/lib/teamcity
{% endhighlight %}

and pointing TemCity to it's data directory(add following to the end of /etc/tomcat6/catalina.properties)

{% highlight java %}
teamcity.data.path=/var/lib/teamcity
{% endhighlight %}

then turn on tomcat and copy .war distribution in /var/lib/tomcat6/webapps folder(known as "web applications directory"):

{% highlight bash %}
cp /root/teamcity-6.5.war /var/lib/tomcat6/webapps
{% endhighlight %}

after deploying .war file by tomcat copy `teamcity-server-log4j.xml` to unpacked teamcity's `conf` directory, in this case to
`/var/lib/tomcat6/webapps/teamcity-6.5/conf`

`teamcity-server-log4j.xml` can be found in non-war distribution of TeamCity.

##### Logging setup:

creating log directory:

{% highlight bash %}
mkdir /var/log/teamcity
chown tomcat6:tomcat6 /var/log/teamcity
{% endhighlight %}

create file  <TeamCity Data Directory>/config/internal.properties with following content in order to setup logging

{% highlight java %}
log4j.configuration=../conf/teamcity-server-log4j.xml
teamcity_logs=/var/log/teamcity/
{% endhighlight %}

##### Memory setup

described here [here](http://confluence.jetbrains.net/display/TCD65/Installing+and+Configuring+the+TeamCity+Server#InstallingandConfiguringtheTeamCityServer-memory)

options should be written in `/etc/default/tomcat6` to JAVA_OPTS env variable

also as mentioned [here](http://confluence.jetbrains.net/display/TCD65/Installing+and+Configuring+the+TeamCity+Server#InstallingandConfiguringtheTeamCityServer-installingJ2eeContainer):

> please add useBodyEncodingForURI="true" attribute to the Connector tag for the server in Tomcat/conf/server.xml file.

### Restart Tomcat

Then go to http://your.server.com:8080/teamcity-6.5 and follow instructions.

Nginx setup
-----------

{% highlight nginx %}
server {
        listen   80; ## listen for ipv4
        server_name ci.test;
        access_log  /var/log/nginx/ci.access.log;

        root /var/lib/tomcat6/webapps;

        location / {
                rewrite  ^(.*)$  /ci   redirect;
        }

        location ~ (png|gif)$ {
        }

        # this location should be the same as this ||
        location /teamcity-6.5 {                 # \/
          proxy_pass http://127.0.0.1:8080/teamcity-6.5;
        }
}
{% endhighlight %}
