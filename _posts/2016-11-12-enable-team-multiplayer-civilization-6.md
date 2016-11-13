---
layout: post
title: "Enabling Team Multiplayer in Civilization 6"
---

Civilization 6 did not ship with theability to team up during multiplayer. However, various internet searches yielded a few results in luascript files. I've decided to document this process and provide screenshots of what to expect. Before you begin, make sure you've closed Civilization 6.

<img class="responsive-img" src="/images/civ6-team-multiplayer.png">

First, find where the copy of Civilization was installed to. Here's a sample Windows path (note: Mac OS users, find the `.app` for Civilization and then ctrl/right-click to `View Package Contents` to see the internal folder structure): `D:\Steam\steamapps\common\Sid Meier's Civilization VI\`. Inside are a set of folders that will contain a script for the multiplayer game staging room: `Base\Assets\UI\FrontEnd\Multiplayer\StagingRoom.lua`. Open this file in a plain text editor (Sublime Text, Atom, TextMate, Notepad, ..).

Next, you'll need to perform a search (or jump to line **1132** as of writing) for the following:

<script src="https://gist.github.com/code-for-coffee/80eda025a0369085ac8c471ba5412457.js"></script>

Finally, edit the boolean value of `playerEntry.TeamPullDown:SetHide(false);` to `true` and save. That's it. Restart Civlization 6 and head to the multiplayer lobby.

Some notes:
* You'll have teams enabled but the first team started with `0`, not `1`. This is likely due to array logic.
* You can only specify between team `0` or team `1`.
* There is no column header that states you will be selecting a team (see screenshot above).
* This _should_ not get you banned from Steam.
* This will not make your multiplayer game incompatible with someone who has not made this edit.
* Players without this enabled may not know they are teamed if only the host has this modification enabled (we have not tested this).

During team game, my friend and I noticed we were able to immediately see each other on the diplomacy screen. The entire game worked fine and we played a good 50-100 turns into our game. However, like the rest of Civilization 6's gameplay, many features are not unlocked immediately. We noticed that we could declare friendship between our civilizations earlier than expected. However, we had to perform deals to open borders and so forth. We were unable to share our maps, either (though we suspect that would be unlocked in a later era). As of writing, our multiplayer game had just reached the medieval era so there could be a lot left to discover. I'll update this post if/as we discover more. 
