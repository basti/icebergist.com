---
redirect_to: https://www.axiomq.com/blog/headless-firefox-in-ubuntu-on-virtual-box-for-cucumber-testing/
layout: post
title: "Headless Firefox in Ubuntu on VirtualBox for Cucumber testing"
date: 2015-12-03 11:12:56 +0200
comments: true
author: Ivica Lakatoš
categories:
  - Ruby and Rails
  - Testing
tags:
  - ruby
  - rails
  - vagrant
  - virtualbox
  - ubuntu
  - cucumber
  - selenium-webdriver
published: true
---

If you use [Vagrant](http://www.vagrantup.com/downloads.html), [VirtualBox](https://www.virtualbox.org/) and Ubuntu to build your Rails apps and you want to test it with Cucumber scenarios, this is the right post for you. By default Vagrant and VirtualBox use Ubuntu without an X server and GUI.

Everything goes well until you need `@javascript` flag for your cucumber scenario. `@javascript` uses a javascript-aware system to process web requests (e.g. Selenium) instead of the default (non-javascript-aware) webrat browser.

### Install Mozilla Firefox

Selenium WebDriver is flexible and lets you run selenium headless in servers with no display. But in order to run, Selenium needs to launch a browser. If there is no display to the machine, the browsers are not launched. So in order to use selenium, you need to fake a display and let selenium and the browser think they are running in a machine with a display.

Install latest version of Mozilla Firefox:

`sudo apt-get install firefox`

Since Ubuntu is running without a X server Selenium cannot start Firefox because it requires an X server.

### Setting up virtual X server

Virtual X server is required to make browsers run normally by making them believe there is a display available, although it doesn't create any visible windows.
<!--more-->
Xvfb (X Virtual FrameBuffer) works fine for this. Xvfb lets you run X-Server in machines with no display devices.

Install xvfb on ubuntu:

`sudo apt-get install xvfb`

Lets run the Xvfb service in a display number which is less likely to clash even if you add a display at a later stage. Display 10 will do fine.

`sudo Xvfb :10 -ac`

The parameter -ac makes xvfb run with access control off. The server should be running now.

### Headless Firefox

Before you can run a browser, you need to set the environment variable DISPLAY with the display number at which xvfb is running.

Open new tab in terminal and set the DISPLAY variable:

`export DISPLAY=:10`

and start mozilla firefox:

`firefox`

Now you run firefox headlessly in Ubuntu, and you can run your cucumber scenarios with `@javascript` flag.

### Start virtual X server automatically

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
