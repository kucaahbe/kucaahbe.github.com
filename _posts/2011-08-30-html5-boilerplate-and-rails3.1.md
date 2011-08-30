---
layout: post
title: Setup html5-boilerplate(gem) with Rails 3.1
---

Environment:
------------

* ruby: `ruby 1.9.2p180 (2011-02-18 revision 30909) [i686-linux]`
* rails `3.1.0.rc8`
* generated scaffold: `post title:string body:text`

Installation:
-------------

As referenced in [readme](https://github.com/sporkd/compass-html5-boilerplate#readme) we should add following gems to our Gemfile:

{% highlight ruby %}
gem "haml"
gem "compass"
gem "html5-boilerplate"
{% endhighlight %}

and run `bundle install`.

Then backup existing application layout and install all stuff:

{% highlight bash %}
mv app/views/layouts/application.html.erb app/views/layouts/application.html.old
compass init rails -r html5-boilerplate -u html5-boilerplate --force
{% endhighlight %}

And restart the rails server.

##### Links:

* [html5-boilerplate](https://github.com/sporkd/compass-html5-boilerplate#readme)
* [compass scss framework](http://compass-style.org/)
* [new rails assets pipeline](https://github.com/sstephenson/sprockets/#readme)
