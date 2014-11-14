---
layout: post
title: "How to create a custom application config for rails"

tags: rails configuration
image: 
---

 
<!--more-->

{% comment %}
![An image]({{ BASE_PATH }}/images/medium/phusion-passenger.png)
Image credits: <https://www.phusionpassenger.com/>
{% endcomment %}

Create a new initializer `config/initializers/load_config.rb` with that content:

{% highlight ruby %}
APP_CONFIG = YAML.load_file("#{Rails.root}/config/config.yml")[Rails.env]
{% endhighlight %}

Then, create a configuration file under `config/config.yml` and put in your config properties:

{% highlight yaml %}
development:
  my_propery: true

test:
  my_propery: true

production:
  my_propery: false
{% endhighlight %}

Now you can use the configuration via the `APP_CONFIG` constant everywhere in your ruby code:

{% highlight ruby %}
if APP_CONFIG['my_propery']
  # your code here...
end
{% endhighlight %}

