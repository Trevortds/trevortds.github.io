---
layout: article
title: "Productivity Hack: Blocking Time-Wasting Domains"
modified:
categories: devlogs
excerpt:
tags: [productivity, unix, blocking, hosts]
image:
  feature: north.jpg
  teaser: /devlogs/lock-xxl.png
  thumb: /devlogs/lock-xxl.png
date: 2016-12-29T13:27:09-07:00
---


Today I have a short log for those who, like me, find themselves easily distracted on the internet. Several solutions already exist, mostly centering around browser extension like RescueTime and StayFocusd. A problem I have found with the extension solution is that it is too easy to circumvent by simply opening an incognito session or using a different browser. This system blocks your time-waster sites system-wide. It takes a modicum of effort to undo, and most importantly, it activates automatically.  


## Element 1: editing /etc/hosts

This part is not new, there are even some tools for ios and linux that will do it for you, though they're unnecessary. All you have to do is edit your `/etc/hosts` file, to include lines of the following form. 

`sudo vim /etc/hosts`


> I like vim, you use your editor of choice, `nano`, `emacs`, `gedit`, whatever. 

```bash
127.0.0.1       www.reddit.com reddit.com *reddit.com
127.0.0.1       www.facebook.com facebook.com *facebook.com
127.0.0.1       www.quora.com quora.com
```

This tells your browser to redirect requests to these domains to ip address 127.0.0.1, which goes nowhere. 

If you find yourself needing to access one of these websites momentarily, as I often do, simply comment out one of the lines with a hash. 


```bash
#127.0.0.1       www.reddit.com reddit.com *reddit.com
#127.0.0.1       www.facebook.com facebook.com *facebook.com
#127.0.0.1       www.quora.com quora.com
```

>It is possible to wrap this functionality into a shell script, this is left as an exercise to the reader. I prefer it to be a little harder than that, so I edit /etc/hosts whenever I want to unblock a site. 

But how do you make sure you don't do this to read some answers on a quora question for work, and then wind up spending the rest of your day goofing off on the internet since the blocker is turned off? 


## Element 2: editing crontab

`cron` is a program that is running whenever your system is on. It is constantly checking the `crontab` files for jobs to do at a particular time of day. Since we want it to edit /etc/hosts, a file owned by root, we need to edit root's crontab. Edit your system's root crontab with 

```bash
sudo crontab -e
```

If you are using Ubuntu's default settings, it will open a config in nano. You can read the information in the file if you like, or you can just copy/paste my solution below. 

Add the following to the end of your crontab file

```bash
# every ten minutes between 8 and 5 on weekdays, block time-wasting websites
*/10 8-15 * * 1-5 sed -i "s/^#127.0.0.1/127.0.0.1/g" /etc/hosts
```

Use `ctrl-o` to save and `ctrl-x` to exit. 

Congrats, you're done! (it may need a restart to take effect)

### Explaination of the crontab edit

Crontab configurations are formatted with five numbers/patterns and a shell command. The numbers tell cron when and how often to execute the command. 

So from right to left 

* */10 : Every tenth minute

* 8-16 : From 8:00 AM to 4:59 PM

* *    : Every day

* *    : Every month

* 1-5  : Monday to Friday

`sed` is a shell command that edits files. It's very versatile and it can get mindbogglingly complex if you look at it too closely, but we're using a simple function of it here: regular expression find and replace. 

* -i : in-place (don't write to a new file)

* s/ : substitute

* /^#127.0.0.1/ : match the string "#127.0.0.1" at the beginning of a line

* /127.0.0.1/ : replace it with "127.0.0.1"

* /g : do it to the entire file, don't stop at the first match. 

* /etc/hosts : the file to perform the replacement on. 


So every ten minutes during banking hours, this file reinstates the block you have set up in /etc/hosts. It affects all browsers on your system, there is no way around it but to pull up an editor and change that config. 

