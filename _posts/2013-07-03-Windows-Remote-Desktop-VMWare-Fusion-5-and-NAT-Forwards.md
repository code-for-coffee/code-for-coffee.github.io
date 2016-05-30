---
layout: post
title: Windows Remote Desktop Connection, VMWare Fusion 5.0, and NAT Forwards
---

<h3>Good news, everyone! This still works in VMWare Fusion 6.0!</h3>

I decided to put an older Macbook to use for hosting test lab virtual machines; a few of which are running various versions of Windows. After enabling Remote Desktop Connections locally on the virtual machines I made sure to set VMWare Fusion's network adapter to Bridged (Autodetect). With the built in OS X firewall disabled I should be able to remote into any Windows machine, right? Apparently not! I tried to look around the internet for help since Fusion doesn't have all of NAT settings forward facing that VMWare Workstation has but came up empty handed with tweaks for Fusion 3 and 4. Well, here is a step-by-step walkthrough on how to allow Remote Desktop through OS X and VMWare Fusion 5.
<ol>
	<li><span style="line-height: 13px;">Install <strong>VMWare Fusion 5</strong> and a Windows based operating system on a virtual machine.</span></li>
	<li>Check your virtual machine's IP settings (ipconfig) verify that DHCP is enabled.</li>
	<li>Shut down the virtual machine and select Network Adapter &gt; Bridged Networking &gt; Select <strong>Autodetect</strong>.</li>
	<li>Open Mac OS X <em>System Preferences. </em>Select <em>Security and Privacy</em> &gt; <i>Firewall.</i> <strong>Enable</strong> the firewall and select <em>Firewall Options</em>. Select the VMWare Fusion application and <strong>Allow incoming connections</strong>. Note: if this machine is in your network's DMZ, you should highly consider enabling stealth mode.<em><span style="text-decoration: underline;">
</span></em></li>
	<li>Now, we need to direct Remote Desktop Connections to VMWare. From your Mac (or via an SSH connection), browse to:
<em>/Library/Preferences/VMWare Fusion/</em></li>
	<li>Open the <strong>networking</strong> file: <i>sudo vi networking</i>. For an example of what the file should look like, <a href="http://pastebin.com/UJhK2DEn">here is a link</a> to the <code>Library/Preferences/VMWare Fusion/networking</code> file on pastebin.</li>
	<li>Look for the <em>[incomingtcp] </em>section. If you do not find one, create one.</li>
	<li>Add the following rule underneath: <em>3389=yourmacsipaddress:3389 </em>(tip: Insert will allow you to edit lines, <em>:quit</em> or <em>:exit</em> will close vi).</li>
	<li>Restart VMWare Fusion and launch your virtual machine. From your client machine, you'll want to connect via Remote Desktop Client to the static IP you set your virtual machine to. Enter in the virtual machine's credentials and you're in.</li>
	<li>That's it! You can now use the Windows RDC on various virtual machines hosted on your Mac as long as you give them each specific, static IPs.</li>
</ol>
Hope this helps. If you have any questions or comments feel free to post them.

<em>
<ul>
<li>Updated 11/29/13: This continues to work in VMWare Fusion 6.0.</li>
<li>Update 12/14/13: Added link to pastebin of the <strong>networking</strong> file.</li>
</ul></em>