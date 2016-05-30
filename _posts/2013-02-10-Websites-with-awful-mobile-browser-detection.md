---
layout: post
title: Websites with awful mobile browser detection
---

<hgroup>
<h3>Please note that this page is outdated and does not represent best practice.</h3>
<h4>You should be using responsive design instead of the advice given below.</h4>
</hgroup>

As the world adopts mobile devices are their computers of choice I find that a lot of websites struggle to display their content properly. One of the more annoying problems is that websites are currently treating users as "desktop" or "mobile" users. Since many mobile devices have a variety of screen sizes and resolutions simply detecting platforms is not efficient. Speaking as someone who owns a tablet I hate being treated like I'm on a phone by many websites; every time this occurs I have to reload the page in another browser (Firefox for Android along with the <a href="https://addons.mozilla.org/en-us/android/addon/desktop-by-default/">Desktop by Default plugin</a>).

I decided that I should avoid running into the same problem myself when creating solutions for the web! I found a few decent examples online here and there but nothing was comprehensive enough. After some researching and testing on a few platforms I wrote up <a href="https://github.com/code-for-coffee/isMobileDevice" target="_blank">isMobileDevice.js</a> (a really simple, small javascript snippet). The prior link will take you to the Github repo for the code.

The benefit of this script is that it first tests your browser window's size (and then platform) to make sure you're actually on a mobile browser. If you're on an iPad or Android tablet your resolution will be passed on as a "real" browser. The downside to this script is that if a user has a window sized very small it'll think you're on a mobile platform (though, with the benefit of testing what a mobile view may look like).

I've been updating it with new platforms as often as I can. If you would like to add to it feel free! Now, go forth and stop annoying tablet users with tiny mobile sites!