---
layout: post
title: Testing webpages on mobile devices using SimpleHTTPServer
---

When developing websites, you usually want to test it out on a mobile device. Resizing your desktop browser gives you an idea of the way the layout changes, but you'll usually find subtle differences in layout and scrolling when you try it out on your phone.

I've seen a bunch of ways to do this, including using [online iPad/iPhone simulators](http://ipadpeek.com/), or installing the iOS simulator or the Android emulator. While these are pretty good ways to do it, sometimes all you want to do is to see your website on the smartphone you already have on you. There's a simple trick I use to do this, and you might not even need to install anything.

The only thing you need installed is Python. If you have a Mac or Linux system, this is probably preinstalled. However, Windows doesn't ship with Python out of the box. You can check quickly by opening up your terminal and typing this:

{% highlight console %}
$ python -V
{% endhighlight %}

If you have it installed, you'll see the version number. 

{% highlight console %}
$ python -V
Python 2.7.5
{% endhighlight %}

If you don't, you can easily [install it](http://www.python.org/getit/).

Once python is installed, navigate over to the folder which contains your website.

{% highlight console %}
$ cd /folder/with/website/
{% endhighlight %}

Run the following command:

{% highlight console %}
$ python -m SimpleHTTPServer
{% endhighlight %}

You should see `Serving HTTP on 0.0.0.0 port 8000 ...`.

What we just did was start a HTTP server that serves the current folder at `localhost`. 

Open up your browser and head to `http://localhost:8000/`. What you see will depend on what was in the folder. By default, most servers look for a file called `index.html` in the root folder, and server that. If you don't have one, you'll see something like this:

![Directory listing if folder has no index.html file](/images/mobile-website-testing-with-simplehttpserver-1.png)

This is a list of files and folders in the folder. Click on the file want to view, and you can view it in the browser.

To see it on your phone, you need to find your computer's IP address. This can usually be found if you opened your network preferences. In the terminal, you can find it by typing `ifconfig`.

![Finding the IP address on OSX](/images/mobile-website-testing-with-simplehttpserver-2.png)

In my case the address is `192.168.0.100`. 

Open up your phone's browser. Make sure the phone and your computer are on the same network, and go to `http://192.168.0.100:8000`. The `8000` is important, because that's the port the SimpleHTTPServer is serving your page at. You should be able to see your website! That wasn't too hard now, was it?

![The website running on my phone](/images/mobile-website-testing-with-simplehttpserver-3.png)

