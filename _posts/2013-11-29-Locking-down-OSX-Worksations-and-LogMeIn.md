---
layout: post
title: Locking down OS X Workstations (and LogMeIn)
---

<p style="text-align: left;">One of the great things about Mac OS X is Darwin, the Unix back-end of the OS. This article discusses a variety of shell (Terminal) commands and tricks that you can use to lock an account's abilities/actions. Finally, the source code can be viewed/forked on <a href="https://github.com/code-for-coffee/OSX-Linux_Bash_Scripts" target="_blank">github</a>. Now, go ahead and open Terminal on a test account.</p>
<p style="text-align: left;">Disable use of Dashboard</p>

<blockquote>
<div id="LC22"><em>defaults write com.apple.dashboard mcx-disabled -bool true</em></div></blockquote>
<p style="text-align: left;">Disable Dock manipulation (adding/removing items)</p>

<blockquote>
<div id="LC22"><em>defaults write com.apple.dock contents-immutable -bool true</em></div></blockquote>
<p style="text-align: left;">Disable Dock manipulation (adding/removing items)</p>

<blockquote>
<div id="LC22"><em>defaults write com.apple.dock contents-immutable -bool true</em>

<em>killall dock #kills the dock; changes will take effect upon reload</em></div></blockquote>

<p style="text-align: left;">Hide System Files</p>

<blockquote>
<div id="LC22"><em>defaults write com.apple.finder AppleShowAllFiles FALSE # TRUE will show System files.</em></div></blockquote>

<p style="text-align: left;">Run Simple Finder</p>

<blockquote>
<div id="LC22"><em>defaults write com.apple.finder InterfaceLevel simple<br>
killall Finder</em></div></blockquote>

<h3>LogMeIn</h3>
If you use LogMeIn for remote administration, you will notice that the application will load in your Dock when running. This causes a slight security risk due to how this will show end users some account information. To rid ourselves of this problem, we can remove the following line /Applications/LogMeIn.app/info.plist (right-click the application and select "Show Contents"). You will need to add the <a href="https://github.com/code-for-coffee/OSX-Linux_Bash_Scripts/blob/master/logmeinRevision.xml" target="_blank">these lines</a> to the first child element in the XML file.

You can also remove MenuBar.app from /applicationsupport/launchagents/logmein/ - this will remove the Menu Bar icon.