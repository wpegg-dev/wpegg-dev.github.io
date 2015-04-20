---
layout: post
title:  "Side Projects"
date:   2015-04-20 12:54:20
categories: mini-project
---
![playback_widget](https://cloud.githubusercontent.com/assets/11460318/7235455/483c239c-e75c-11e4-908b-71e8bc07abd5.png)
<br><br>
I came across a podcast a few weeks ago that mentioned something called Github pages so I decided to give it a look. It turns out it’s a free 
static web page hosting platform anyone with a Github account can use! It is really cool! I would recommend it to anyone looking to host a very 
simple static web site. I decided to use if for my blog so I copied all of my posts from WordPress to Github!
<br><br>
I have also been working on some side projects to help learn a little more Ruby. I worked on my Weather Widget app and started working on 
another little project based on a cool CodePen I came across the other day. It is an audio playback widget that plays whatever song the user 
has loaded into the assets folder. I’ll talk more about it in a few minutes. I wanted to go over the additions I made to my other project.
First, I made a small addition to my [Weather Widget](https://github.com/wpegg-dev/Small-Projects/tree/master/WeatherWidget) by adding a week forecast to the page. It was pretty cool and I got to learn about 
Hashing and Arrays in Ruby which was pretty interesting. It is a little clunky right now because each day it added as a different <div> but I 
am working on trying to add the week days more dynamically by looping through a Ruby object and outputting the HTML on the fly. I think once I 
have that completed it will be pretty cool!
<br><br>
Next, as I mentioned above, I started working on a Playback Widget that I got the inspiration from [CodePen]( http://codepen.io/onelegged/pen/vEwvNd/) for. It was a little trickier to get running than the last project because it used a few frameworks I am unfamiliar with. ([Jade]( http://jade-lang.com/)) Once I got it up and running on my local machine I thought it would be a really cool addition to work some file using Ruby. I am working on building the ability for a user to upload an audio file and then select it from a dropdown to playback! This has been really cool to learn how to read directories and files and load them dynamically. One thing that I have found a little difficult is uploading the files. I have it so that it will automatically read the audio files and allow the user to select and play them but the uploading seems a bit trickier than I thought it would be. Another thing I noticed is that the Pen that I got the inspiration from has a small problem where the user has the click the play button twice to play the song. While it isn’t a huge deal it is a little annoying to a user to have to do something like this so I play on addressing it after I get the file upload working. You can find the source of the playback widget [here]( https://github.com/wpegg-dev/Small-Projects/tree/master/PlaybackWidget).
