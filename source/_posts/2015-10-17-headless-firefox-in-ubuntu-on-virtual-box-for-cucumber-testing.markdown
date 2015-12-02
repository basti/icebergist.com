---
layout: post
title: "Headless firefox in ubuntu on virtual box for cucumber testing"
date: 2015-10-17 14:12:56 +0200
comments: true
author: Ivica Lakatoš
categories: 
  - Ruby and Rails
  - Testing
tags: 
  - ruby
  - rails
  - vagrant
  - virtual box
  - ubuntu
  - cucumber
  - selenium-webdriver
published: true
---

If you use [Vagrant](http://www.vagrantup.com/downloads.html) and [VirtualBox](https://www.virtualbox.org/) (e.g. some Ubuntu package) to build your Rails app and you want to test it with cucumber scenarios, this is right post for you. 

Everything goes well until you do not need `@javascript` flag for your cucumber scenario. `@javascript` uses a javascript-aware system to process web requests (e.g., Selenium) instead of the default (non-javascript-aware) webrat browser.

### You will need mozilla firefox

Selenium WebDriver is flexible and let you run selenium headless in servers with no display. But in order to run, Selenium needs to launch a browser. If there are no display to the machine, the browsers are not launched. So in order to use selenium, you need to fake a display and let selenium and the browser thinks they are running in a machine with a display.

Install latest version of Mozilla Firefox:

`sudo apt-get install firefox`

### Also you need to run X server

X server is required to make browsers run normally by making them believe there is a display available, although it doesn't create any visible windows. 

Xvfb (X Virtual FrameBuffer) works fine for this. Xvfb lets you run X-Server in machines with no display devices. 

Install xvfb on ubuntu:

`sudo apt-get install xvfb`

Lets run the Xvfb service in a display number which is less likely to clash even if you add a display at later stage. Display 10 will do fine.

`sudo Xvfb :10 -ac`

The parameter -ac makes xvfb run with access control off. The server should be running now.

### Start firefox headlessly in Ubuntu

Before you can run a browser, you need to set the environment variable DISPLAY with the display number at which xvfb is running.

Open new tab in terminal and set the DISPLAY variable:

`export DISPLAY=:10`

and start mozilla firefox:

`firefox`

Now you run firefox headlessly in Ubuntu, and you can run your cucumber scenarios with `@javascript` flag.

### Or use init script to start X server automatically

To run your X server automatically, after installing Xvfb, you will need to:

+ put content of [this gist](https://gist.github.com/basti/2db0b71e893ee4d6d015) in `/etc/init.d/xvfb` (hint use `sudo wget` command to do that)
+  make it executable `sudo chmod a+x /etc/init.d/xvfb`
+  start xvfb on display number 10 `export DISPLAY=:10`
+  run X server `sudo /etc/init.d/xvfb start`
+  when you want to stop X server `sudo /etc/init.d/xvfb stop`

This is my way to run firefox headlessly in Virtual box Ubuntu, and to run cucumber scenarios with `@javascript` flag.

References:

* [Selenium Headless Automated Testing in Ubuntu](http://www.installationpage.com/selenium/how-to-run-selenium-headless-firefox-in-ubuntu/)
* [Xvfb init script for Ubuntu](https://gist.github.com/jterrace/2911875)
