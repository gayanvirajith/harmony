---
layout: post
title:  "Debugging Capybara Specs"
date:   2016-1-25 00:00:00
permalink: 'blog/debugging-capybara'
---

I recently upgraded [Mindful Choices](https://www.mindfulchoices.co.uk) from Rails 4.2 to Rails 4.3  
Inevitably, a few of my previously passing specs started to fail after the minor version bump. Here's an example:

{% highlight ruby %}
  Failures:

  1) Admin interacts with member_resource form, viewing a self-hosted resource, switching to remotely hosted, file picker should be hidden
     Failure/Error: page.should have_css("div#upload_div.hidden")
       expected to find css "div#upload_div.hidden" but there were no matches. Also found "", which matched the selector but not all filters.
     # ./spec/features/member_resources/form_spec.rb:20:in `block (4 levels) in <top (required)>'
{% endhighlight %}

Despite the complex description, the failing spec actually tests some very simple behaviour. Based on the value of a radio-button, an appropriate input element (file upload or text box) should be displayed to the user.

![Resource Toggling In Action](/assets/images/2016/toggle_resource.gif)

The code was clearly working correctly if I tested it locally using FireFox, so why the failing test? As always, Google yielded several tips for debugging Capybara specs, my favourites are listed below.

---

### Capybara Screenshot:
I had a hunch that perhaps the Rails Asset Pipeline wasn't working correctly in the test environment. Could my spec be seeing an unstyled, unscripted DOM? Let's take a screenshot at runtime and see how things look.  

{% highlight ruby %}
# Place this within your spec file to capture a browser screenshot of your code in execution
Capybara::Screenshot.screenshot_and_open_image
{% endhighlight %}

Aside from the overflowing logo, everything looks OK! Perhaps it's not the asset pipeline, after all.

![Capybara Screenshot](/assets/images/2016/capybara_screenshot.png){:.aligncenter}

---

### Good old console.log
My next theory was that, for some internal reason, the javascript might not be executing within the Capybara browser. How can you tell whether JavaScript has executed on the page? Print a message to console, of course:  

{% highlight ruby %}
# Place a call to console.log anywhere in the your JavaScript code, verifying execution and checking variable values
console.log("Hello world. The value of self_hosted is " + self_hosted)
{% endhighlight %}

Use [byebug](https://github.com/deivid-rodriguez/byebug) to pause execution of your spec and the [webkit](https://github.com/thoughtbot/capybara-webkit) capybara driver to grab any messages from the console:
{% highlight ruby %}
byebug # pauses code execution and drops you into a shell
page.driver.console_messages
[{:line_number=>5, :message=>"Hello world. The value of self_hosted is true", :source=>"http://127.0.0.1:61041/assets/toggle_resource_div-75864051056d3ee42b18f77964b380824d2fc9f9d155c86bc3d4db3252a67ea1.js"}]
{% endhighlight %}

---

### Check Your Assertions
After eliminating the asset pipeline and JS environment from my investigation, I turned my attention to the spec itself.  
This is the assertion that I wrote many months ago, and that up until now has been passing reliably:

{% highlight ruby %}
page.should have_css("div#url_div.hidden")
{% endhighlight %}

Believe it or not, refactoring the spec to this alternative form made it pass first time!

{% highlight ruby %}
expect(page).to have_selector('div#url_div', visible: false)
{% endhighlight %}

I guess that's [Occam's razor](https://en.wikipedia.org/wiki/Occam's_razor) in action.

##### Research Sources
http://www.jasonfleetwoodboldt.com/writing/2014/03/18/capybara-debugging-javascript/
