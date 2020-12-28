This guide is specifically for configuring the [Game Capture Source](https://obsproject.com/wiki/Sources-Guide#game-capture) properly, and goes over basic troubleshooting if things may not be working. Follow the Game Capture link for an overview of Game Capture and some basic info about it, come back to this page if you are having any issues with it working. There are several issues which you can run into getting a Game Capture working properly.

Sections of this guide:

* [How to Set up Game Capture](https://github.com/obsproject/obs-studio/wiki/Game-Capture-Guide#how-to-set-up-game-capture)
* [Common Issue Troubleshooting/Resolution](https://github.com/obsproject/obs-studio/wiki/Game-Capture-Guide#common-resolutions-for-game-capture-issues)

***
# How To Set Up Game Capture

In your Sources Box, select the + and choose Game Capture, it will bring up the window below 

![](https://i.imgur.com/lFfCgtd.png)

Add a new source named after your game, or select one from the list if you have previously added game captures from other scenes in the current scene collection. Note that it is recommended to only have one game capture source per scene if you are specifying each game individually, otherwise use the methods outlined in the [Sources Guide](https://obsproject.com/wiki/Sources-Guide#game-capture) for hotkey mode.

Once you have the properties window open, decide which Mode you would like, remember that Fullscreen will simply capture any fullscreen game, capture specific window (as shown below) lets you specify the exact game you want. If you choose that method, the game must be on, then select it's name in the Window dropdown

![](https://i.imgur.com/jx9NPHA.png)

For the options you can check, it's generally a good idea to leave them on the default as shown. SLI Capture often causes game capture to break if it's not a specific hardware setup.

If you change games often, try out the hotkey mode, which lets you press a key to select your active game. Otherwise create individual scenes with the single game capture set to specified window to keep scenes clean.

Most of the time, your game capture should work once you apply the changes in the Game Capture Properties window, and it will show up in your OBS preview if you have the source selected and the game active. If it doesn't, continue on for troubleshooting and common resolutions.


***

# Common Resolutions for Game Capture Issues

Here are some common things to look for or do if you are running into problems getting a game capture source to show up.

* Run OBS as admin. A lot of games requires this for them to be captured, such as Call of Duty, Valorant, and others. This will also help with performance in some cases. 
* Check for [Known Conflicts](https://obsproject.com/wiki/Known-Conflicts), such as overlays, FPS counters, Anti-virus programs, etc.
* Certain games, like [Destiny 2](https://www.bungie.net/en/Help/Article/46101) and [CS:GO](https://blog.counter-strike.net/index.php/2020/07/30736/) do not allow OBS to hook in with game capture. We recommend running the game in either windowed or borderless fullscreen and using a Window Capture source instead. Emulators and older games may also need window capture.
* Certain DX12 games, like Fortnite when running on DX12 mode, may cause the game to crash if run with OBS, and DX12 capture may sometimes have weird frame issues, due to the graphics API. If the game has DX11 API it is recommended to try that option to see if problems resolve. 
* If you are using a laptop, make sure your OBS and the game you are running are using the High-performance GPU, per the instructions [here](https://obsproject.com/wiki/Laptop-GPU-Selection-Windows-10). OBS and most games should do this by default, but certain games, like [Minecraft Java](https://obsproject.com/wiki/Minecraft-Not-Working-With-Game-Capture) and osu! default to the iGPU on multi-gpu systems. Use the method linked to change games to the high-performance GPU, then restart the game and OBS.
* Sometimes you may see the red dot in the upper left corner of your preview with a game capture source, select the source in your list and press Ctrl+R then Ctrl+f to reset than fit the source to your preview screen. 

If at this point you are still having issues, please use either the [OBS Discord](https://obsproject.com/discord) or the [Forums](https://obsproject.com/forum/), explain your issue/troubleshooting steps in detail, and provide a current OBS log.