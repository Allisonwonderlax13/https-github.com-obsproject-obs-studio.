This guide is specifically for configuring the [Game Capture Source](https://obsproject.com/wiki/Sources-Guide#game-capture) properly, and goes over basic troubleshooting if things may not be working. Follow the Game Capture link for an overview of Game Capture and some basic info about it, come back to this page if you are having any issues with it working. There are several issues which you can run into getting a Game Capture working properly.

Sections of this guide:

* [How to Set Up Game Capture](#how-to-set-up-game-capture)
* [Common Issue Troubleshooting/Resolution](#common-resolutions-for-game-capture-issues)
* [Games with Known Issues](#games-with-known-issues)

***
# How to Set Up Game Capture

**Note**: Game Capture is only available as a source on Windows. macOS users should use Display Capture for the best performance instead.

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

Here are some common things to try or look out for if you are running into problems getting a game capture source to show up.

## Run as administrator

A lot of games require OBS Studio to run as administrator in order to be properly captured, such as Call of Duty, Valorant, and others. This will also help with performance in some cases.

## Conflicting software

Check for [Known Conflicts](https://obsproject.com/wiki/Known-Conflicts), such as overlays, FPS counters, anti-virus programs, etc.

As of 27.1, Rivatuner/MSI Afterburner are especially prevalent conflicts, even though they have always been listed as such. Turn those off before hooking into a game with OBS. If you have RTSS version 7.3.2 or newer, you can enable compatibility with OBS by selecting the "Use Microsoft Detours API hooking" option in RTSS' Setup. ![](https://i.imgur.com/Pf5Twk4.png)

## Issues with fullscreen games

If a game is set to run in full-screen mode, when you alt+tab out the game, it will stop rendering. This means that you will not see the game in OBS while it is minimized. Either make a test recording and check it to verify the capture worked properly, or move OBS to a second monitor if you have one.

Certain games, like [Destiny 2](https://www.bungie.net/en/Help/Article/46101) and [CS:GO](https://blog.counter-strike.net/index.php/2020/07/30736/) do not allow OBS to hook in with game capture. We recommend running the game in either windowed or borderless fullscreen and using a Window Capture instead.

Furthermore, emulators and older games are often not compatible with modern game hooking processes; if you are having issues with capturing programs like this with Game Capture, try capturing them with Window 

## DirectX 12 games

Certain DX12 games, like Fortnite when running on DX12 mode, may cause the game to crash if run with OBS, and DX12 capture may sometimes have weird frame issues, due to the graphics API. If the game has DX11 API it is recommended to try that option to see if problems resolve.

## Laptops and all-in-one computers with multiple GPUs

If you are using a laptop or all-in-one computer, make sure your OBS and the game you are running are using the High-performance GPU, per the instructions [here](https://obsproject.com/wiki/Laptop-GPU-Selection-Windows-10). OBS and most games should do this by default.

Certain games, such as Minecraft: Java Edition and osu! default to the integrated GPU on multi-GPU systems. Use the [Minecraft guide](https://obsproject.com/wiki/Minecraft-Not-Working-With-Game-Capture) to change these games to the high-performance GPU, then restart the game and OBS Studio.

## Game captured at the wrong size in OBS Studio

Sometimes you may see the red dot in the upper left corner of your preview with a Game Capture source. This means that the source's size has become stuck. Right-click on the source, select the Transform menu, then click Reset Transform (Ctrl-R). In the same menu, click Fit to Screen (Ctrl-F). This will fit the source to your preview screen so that the game takes up as much of the screen as possible.

***

# Games with known issues

| Game | Resolution(s) |
| ---- | ------------- |
| Call of Duty | Run OBS as administrator |
| Counter-Strike: Global Offensive (CS:GO) | Use windowed/borderless fullscreen or use Window Capture |
| Destiny 2 | Use windowed/borderless fullscreen and Window Capture instead of Game Capture |
| Fortnite | If you experience crashes or framerate issues in DX12 mode, switch to DX11 mode |
| League of Legends | Create two scenes. In the first scene, add a Window Capture of the LoL launcher/lobby. In the second scene, run the game to add a Game Capture of the game itself. Finally, configure the Scene Switcher to automatically swap between them. |
| Minecraft: Java Edition | For laptops and all-in-one computers, follow the [Minecraft guide](https://obsproject.com/wiki/Minecraft-Not-Working-With-Game-Capture) |
| Valorant | Run OBS as administrator |

If at this point you are still having issues, please use either the [OBS Discord](https://obsproject.com/discord) or the [Forums](https://obsproject.com/forum/), explain your issue/troubleshooting steps in detail, and provide a current OBS log.