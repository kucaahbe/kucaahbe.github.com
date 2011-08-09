---
layout: post
title: Debian tips
---
List of installed packages for specific release(archive):
---------------------------------------------------------

{% highlight bash %}
aptitude versions --group-by=none '~i ~Aunstable' | grep -v stable,unstable
{% endhighlight %}

#### Links:

* [list of search patterns for aptitude](http://algebraicthunk.net/~dburrows/projects/aptitude/doc/en/ch02s03s05.html#tableSearchTermQuickGuide)
