---
layout: post
title:  "The GMT Pips At Home"
date:   2017-7-23 16:00:00
permalink: 'blog/the-gmt-pips'
---

OK, you got me, I'm somewhat of a radio nerd. I'm so nerdy, in fact, that I decided I wanted "the gmt pips" to be audible in my mancave each hour. If you want the same, keep reading.  

For the uninitiated, "the gmt pips" ("the pips", for short) are the series of tones you hear (typically on BBC radio) in the few seconds leading up to "the top of the hour", often before the news. The pips used to feature heavily on UK broadcasts, but you don't hear them so much nowadays, which I find sad. During his 10 years at Radio 1, Chris Moyles used to time his 06:30am [Cheesy Song](https://www.youtube.com/watch?v=s_UL18EV6xY) and 10am hand-over link to coincide perfectly with the pips, which most people wouldn't appreciate (or even notice), but it always made me smile.

So how can you go about generating the pips in your own home?  

The [wikipedia article](https://en.wikipedia.org/wiki/Greenwich_Time_Signal) for the Greenwich Time Signal makes the pips audio available as an `.ogg` file. This original file has a duration of 5.9 seconds. I want to simply trigger the playback of this file as part of an hourly [cron job](https://en.wikipedia.org/wiki/Cron) on my [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/).  
Unfortunately, the unix cron scheduler only allows you to schedule to a specific minute, whilst I need more accuracy, so I can fire the clip 5 seconds _before_ each hour. I considered installing a more precise scheduler, such as [frequent-cron](https://github.com/homer6/frequent-cron), but this seeemed like overkill and a heavy dependency for such a simple task.
Instead, I used [Audacity](http://www.audacityteam.org/) to add 55 seconds of silence to the start of the `.ogg` file, also taking the opportunity to save as a more portable `.mp3` file. The [resulting 60 second clip](/assets/files/gmt_pips_60_sec.mp3) can now be scheduled to play on the 59th minute of each hour, using the following cron command:
```
59 * * * * omxplayer -o local /home/pi/gmt_pips_60_sec.mp3
```
This command assumes the availability of `omxplayer` for handling the playback, though this comes preinstalled with `raspbian`.  
I'm also (painfully) aware that your pips might be off by a few milliseconds, due to the startup time of `omxplayer` itself, though this is pretty neglible and something I can live with. Enjoy, you beautiful radio nerd.
