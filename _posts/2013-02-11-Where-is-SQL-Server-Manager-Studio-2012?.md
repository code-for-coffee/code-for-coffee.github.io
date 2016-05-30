---
layout: post
title: Where is SQL Server Manager 2012?
---

<em>...it is hiding in the depths of Microsoft's Evaluation site!</em>

For one of my projects I've been tasked at creating a development environment for a few ASP.NET projects. In one specific project we're tasked with installing an older version of Microsoft SQL Server (2008) but because we have been using Windows 8 for my development environment I run into runtime errors attempting to get the application to run. After a few reinstalls, I decided to go ahead and upgrade the development stack. After installing Visual Studio 2012 I installed SQL Server 2012 Express. To my dismay <strong>SQL Server Manager Studio</strong> (ssms.exe) was nowhere to be found!

Turns out you'll need grab the evaluation version with Service Pack 1 for it. Good news! You can get <a href="http://msdn.microsoft.com/en-us/evalcenter/hh230763.aspx" target="_blank">your copy from Microsoft</a>. You'll need to register (select "<em>Get Started Now</em>" at the bottom"). Once you're done you select "SQL Server Management Studio Express" (<a href="https://www.microsoft.com/betaexperience/pd/SQLEXPNOCTAV2/enus/default" target="_blank">direct link</a>) from the drop down menu and download what you need.

941mb later you're able to run the installer. Select the default installation method and then follow the prompts as need be. After you're finished SQL Server Manager Studio will be yours to use as you wish.