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

As referenced in [readme](https://github.com/sporkd/compass-html5-boilerplate#readme) we should add following gems to our Gemfile, but some in <strong>assets</strong> group:

{% highlight ruby %}
gem "haml"

# Gems used only for assets and not required
# in production environments by default.
group :assets do
  gem 'sass-rails', "  ~> 3.1.0.rc"
  gem 'coffee-rails', "~> 3.1.0.rc"
  gem 'uglifier'
  gem "compass", :git => 'git://github.com/chriseppstein/compass.git', :branch => 'rails31'
  gem "html5-boilerplate"
end
{% endhighlight %}

and run `bundle install`.

Then backup existing application layout and install all stuff:

{% highlight bash %}
mv app/views/layouts/application.html.erb app/views/layouts/application.html.old
compass init rails -r html5-boilerplate -u html5-boilerplate --force
{% endhighlight %}

And restart the rails server.

#### Fixing javascripts:

Now move files in a right places:

{% highlight bash %}
mv public/javascripts/modernizr.min.js app/assets/javascripts
mv public/javascripts/plugins.js app/assets/javascripts
mv public/javascripts/respond.min.js app/assets/javascripts
{% endhighlight %}

And remove unneeded files:

{% highlight bash %}
rm public/javascripts/jquery.js
rm public/javascripts/jquery.min.js
rm public/javascripts/rails.js # used to provide UJS, but it is already provided by jquery-rails gem
{% endhighlight %}

Modify `app/views/layouts/_javascripts.html.haml` file:

{% highlight ruby %}
-# Grab Google CDN's jQuery, with a protocol relative URL
-# Looks for google_api_key first in ENV['GOOGLE_API_KEY'] then in config/google.yml
-# remote_jquery and local_jquery helpers use minified jquery unless Rails.env is development
- if !google_api_key.blank?
  = javascript_include_tag "//www.google.com/jsapi?key=#{google_api_key}"
  :javascript
    google.load(#{ remote_jquery("1.6") });
- else
  = javascript_include_tag "//ajax.googleapis.com/ajax/libs/jquery/#{ local_jquery("1.6") }"

-# fall back to local jQuery if necessary
:javascript
  window.jQuery || document.write("#{escape_javascript(javascript_include_tag('jquery'))}")
  
= javascript_include_tag 'application'
    
-#  Append your own using content_for :javascripts
= yield :javascripts

-# asynchronous google analytics: mathiasbynens.be/notes/async-analytics-snippet
-# Looks for google_account_id first in ENV['GOOGLE_ACCOUNT_ID'] then in config/google.yml
- if !google_account_id.blank?
  :javascript
    var _gaq=[["_setAccount","#{google_account_id}"],["_trackPageview"],["_trackPageLoadTime"]];
    (function(d,t){var g=d.createElement(t),s=d.getElementsByTagName(t)[0];g.async=1;
    g.src=("https:"==location.protocol?"//ssl":"//www")+".google-analytics.com/ga.js";
    s.parentNode.insertBefore(g,s)}(document,"script"));
{% endhighlight %}

and `app/assets/application.js`:

{% highlight javascript %}
// This is a manifest file that'll be compiled into including all the files listed below.
// Add new JavaScript/Coffee code in separate files in this directory and they'll automatically
// be included in the compiled file accessible from http://example.com/assets/application.js
// It's not advisable to add code directly here, but if you do, it'll appear at the bottom of the
// the compiled file.
//
//= require plugins
//= require jquery_ujs
{% endhighlight %}

and you should be done with javascripts.

#### Fixing stylesheets:

move generated stylesheets:

{% highlight bash %}
mv app/stylesheets/* app/assets/stylesheets
{% endhighlight %}

delete `app/assets/application.css`

##### Links:

* [html5-boilerplate](https://github.com/sporkd/compass-html5-boilerplate#readme)
* [compass scss framework](http://compass-style.org/)
* [new rails assets pipeline](https://github.com/sstephenson/sprockets/#readme)
* [incoming new html5-boilerplate for new rails](https://github.com/sporkd/html5-rails)
* [usefull blog post](http://metaskills.net/2011/07/29/use-compass-sass-framework-files-with-the-rails-3.1.0.rc5-asset-pipeline/)
