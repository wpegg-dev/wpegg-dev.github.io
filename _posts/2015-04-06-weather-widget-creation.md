---
layout: post
title:  "Side Project - Weather Widget"
date:   2015-04-06 13:58:20
categories: mini-project
---
![weather_widget](https://cloud.githubusercontent.com/assets/11460318/7182713/32e1ee98-e421-11e4-9d47-053dec73dc93.png)

I was browsing through a site I recently heard about called [CodePen](http://codepen.io/) where you can create or search for "pens" which are small 
html/js/css snippets that make really cool front end examples. I found one the other day called [Weather Popup Widget](http://codepen.io/wpegg/pen/WbLGmJ).
<br><br>
I have bee really itching to get my Raspberry Pi running something on Ruby and thought this would be something really simple to turn into a small web app 
that would run on the Pi. I decided to use this as a break from learning Ruby and trying to put it into action. I found [this](http://computers.tutsplus.com/tutorials/how-to-install-ruby-on-rails-on-raspberry-pi--cms-21421) pretty good guide on how to 
install Rails on my Raspberry Pi.
<br><br>
Then I decided to start working on how to get the Pen to work with some actual Ruby code. I made a small project on my machine by downloading the Aptana 
Studio 3 installer. After downloading and installing RubyInstaller I was ready to start coding! I did some looking online for Ruby gems that can be used for 
weather information and found one called forecast_io which corresponds to a website forecast.io. I checked it out and you can create a free account to use 
their API (click [here](https://developer.forecast.io/docs/v2) for more information).
<br><br>
After reading through the docs, I ran into a few problems with getting the SSL connection to work on my machine and I was able to correct the issue by 
downloading a PEM file as suggested in [this article](https://gist.github.com/fnichol/867550). I was then able to get past the SSL problem. After a few hours of messing with the code and learning the 
commands to create projects and modules and views using this tutorial, I had the widget working locally.
<br><br>
Next, I uploaded it onto the Raspberry Pi without any problems and it has been working great! I am going to work on getting Rails to start automatically when 
the server boots up and then to load the site to fit the full screen on the PiTFT I installed last week!
<br><br>
This was a really cool little project to take my mind of just learning Ruby and putting it to action and it was super fun! I think I might start looking into
doing more of these little projects to help learn the language and framework though doing.
<br><br>
You can check out the source for my WeatherWidget [here](https://github.com/wpegg-dev/Small-Projects/tree/master/WeatherWidget)!

{% include disqus.html %}