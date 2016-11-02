---
layout: post
title: Disabling Java update notifications in Windows
---

Java updates - everyone knows about them because you're prompted to update them all the time. While it is important to keep your system up to date due to zero day exploits and other bugs, most people tend to ignore these updates and just close the notification window. At work, we've recently begun automatically pushing Java updates out on our Windows workstations through WSUS with <a href="http://wsuspackagepublisher.codeplex.com/" target="_blank">Wsus Package Publisher</a>; however, the Java update notifications can still appear from time to time. To alleviate this annoyance for our employees, I've put together a registry tweak that can be pushed out to any machine that'll disable the notifications once and for all. It is a simple registry script; just run and close (or script it to run with Powershell once). You can grab the source from my <a href="https://github.com/code-for-coffee/DisableJavaUpdate" target="_blank">Github repo</a>. Alternatively, here are the entries in the registry to update if you want to handle it yourself. The entries you need to add/update are in bold.

<code>[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\JavaSoft\Java Update\Policy]
"Method"="jchrome"
"PostStatusUrl"="https://sjremetrics.java.com/b/ss//6"
"<strong>EnableJavaUpdate"=dword:00000000</strong>
"<strong>NotifyDownload"=dword:00000001</strong>
"UpdateSchedule"=dword:00000010
"Frequency"=dword:01010000
"UpdateMin"=dword:00000010
"VisitorId"="2f22e551-8912880f"
"<strong>NotifyInstall"=dword:00000000</strong></code>