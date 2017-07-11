---
redirect_to: https://www.axiomq.com/blog/sproutcore-javascript-framework/
date: 2008-07-03 09:00:00 +0100
title: 'SproutCore &#8211; a javascript framework'
author: Slobodan Kovačević
layout: post
permalink: /posts/sproutcore-javascript-framework
comments: true
categories:
  - Javascript
  - Ruby and Rails
tags:
  - Javascript
  - rails
  - ruby
---
[SproutCore][1] is a javascript framework which tries to enable developers to build web apps that look and act more like a desktop apps.

It steps away from a classic web app model by moving a lot of app into the browser itself, which then interacts with server via AJAX. As it says on the [SproutCore site][2]:

> After lots of testing, we have found that the most efficient way to server a SproutCore application is as a …. static web page!

This means that a &#8220;simple&#8221; static HTML page (which is easily served by Apache) makes browser do most of the work (i.e. server doesn&#8217;t have to generate the pages) which frees up server to respond only to AJAX initiated requests.

[SproutCore][3] is written in Ruby, but once you build the app it will generate a set of HTML, JS and CSS files, so you don&#8217;t need to know Ruby in order to use it. As the site says:

> The code you write with SproutCore will resemble a desktop app written in Cocoa more than it will a web application written in Rails.

<img class="alignright size-full wp-image-6" title="SproutCore Photos Demo" src="http://icebergist.com/wp-content/uploads/2008/07/sprout-photos-demo.jpg" alt="SproutCore Photos Demo Screenshot" width="500" height="236" />
Another great thing about SproutCore is that it can be hooked up with any backend as long as it can communicate with it using HTTP. It can be anything: Rails, PHP, Perl, Java, ASP&#8230;

Actions speak louder than words, so take a look at the SproutCore demos which shows you exactly what it&#8217;s all about.

*   [SproutCore based photo gallery][4] &#8211; iPhoto anyone? <img src='http://icebergist.com/wp-includes/images/smilies/icon_smile.gif' alt=':)' class='wp-smiley' />
*   [SproutCore sample controls][5] &#8211; demonstrates what kind of controls are already available in SproutCore.

In next few days I will try to build a sample application powered by SproutCore and Rails to see how it goes. I will post my impressions here. After all if it&#8217;s something Apple used for Mobile.me &#8211; well, it can&#8217;t be that bad. <img src='http://icebergist.com/wp-includes/images/smilies/icon_wink.gif' alt=';)' class='wp-smiley' />

[1]: http://www.sproutcore.com "SproutCore - Javascript Framework"
[2]: http://www.sproutcore.com/about/ "About SproutCore"
[3]: http://www.sproutcore.com "SproutCore - Javascript Framework written in Ruby"
[4]: http://www.sproutcore.com/static/photos/ "SproutCore demo photo gallery"
[5]: http://www.sproutcore.com/static/sample_controls/ "SproutCore demo sample controls"
