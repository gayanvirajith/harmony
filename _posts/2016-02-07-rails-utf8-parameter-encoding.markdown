---
layout: post
title:  "Rails UTF-8 Parameter Encoding"
date:   2016-2-7 18:00:00
permalink: 'blog/rails-utf-8'
---

Today I woke up to the following 500 error notification from [mindfulchoices](https://www.mindfulchoices.co.uk):
{% highlight ruby %}
invalid byte sequence in UTF-8
  app/controllers/page_controller.rb:21:in `contact'
{% endhighlight %}

Seemingly, a SEO bot in New York had submitted a message via the 'Contact Us' form, though it included a non-UTF8 sequence:
{% highlight ruby %}
"And what is better than traffic? It\x92s recurring traffic!"
{% endhighlight %}

After a little reading around the subject, it seems Rails tries [everything it can](http://intertwingly.net/blog/2010/07/29/Rails-and-Snowmen) to force browsers to encode their submissions as UTF-8.  

What can you do if/when a browser refuses to conform and sends non-unicode characters to your server? Surprisingly, I couldn't find an idiomatic solution to this issue.

A [post](https://robots.thoughtbot.com/fight-back-utf-8-invalid-byte-sequences) from the all-knowing [thoughtbot](https://thoughtbot.com/) switched me on to the `String#encode` method.  

Since my code was already using [Rails Strong Params](http://edgeguides.rubyonrails.org/action_controller_overview.html#strong-parameters), I bolted on some extra code that'll strip out any non-UTF-8 characters from the submitted data:

{% highlight ruby %}
# Sanitise user-submitted data using strong parameters and enforcement of UTF-8 encoding
  def message_params
    params_hash = params.require(:message).permit(:name, :email, :phone, :body).to_hash
    params_hash.merge(params_hash).each do |key, value|
      value.encode!('UTF-8', 'binary', invalid: :replace, undef: :replace, replace: '')
    end
  end
{% endhighlight %}
I'm curious, do you know of a better (more "Rails-y") way to mitigate this problem?
