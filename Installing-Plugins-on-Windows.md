## Intro
Before you do anything, you should do your best to thoroughly read the installation instructions the Plugin developer has provided. Not all plugins are identical, and there are mostly likely going to be small variations between all the different plugins.

The instructions from the plugin developer might say something regarding which versions of OBS is supported. If it hasn't been updated for a long time, there is a decent chance it will no longer work on the latest release.

Before you start, make sure obs is **closed**. If you get messages about files being in use, obs is most likely running. Either just restart (dont start any other software), og end obs64.exe in the task manager.

TLDR / im a video learner: 

_insert video link questionmark_

## Installation types

Plugins come usually come in two different installation types.
* Installer / Executable (exe)

**Recommended when the option is presented.** Mostly automatic, might require ensuring that the path to OBS is correct, as the installers can get confused by previous installations or if an installation has been moved.

Exceptions would be when its a portable OBS, or you dont have admin access on the machine.

* Files (often compressed zip)

Common with smaller and/or newer plugins. You are presented with a compressed file (zip or other archive) containing folders and files.
The contents (the files and folders inside) needs to be extracted or copied into the obs-studio folder

**Make sure you download the correct file. If you got the source code, that is the wrong one.**

***

### Installation of the files/archive type

**Source**: Our downloaded archive (zip).   

**Target**: The root folder of OBS (obs-studio), by default located in:
> C:\Program Files\obs-studio

If you are not sure whether this is your OBS install locations, or you can't find it, the best way to check is to use the OBS shortcut. Right clicking it, and choosing "open file location" will take you to where its installed.   

Keep in mind that it will take you a few levels into the folder structure (in `bin\64bit` (or 32bit)). The location we want is two steps up (the obs-studio location).


_Insert pic of what the folder looks like_

Now that we have our source and our target, we can get to copying/extracting the contents into our `obs-studio` folder.

***

When you open the archive (zip) you should be able to see folders named "obs-plugins" and "data". Sometimes you will also see "bin" or other folder names. Its important to extract/copy all of the folders. If you dont see any folders with those names, you need to enter the next folder, often called "windows" or `<plugin name>`.

At this point we are just playing the matching game. Drag or copy the folders into the `obs-studio` folder which should have matching folder names.


_Insert pic of illustration of dragging folders from zip to obs-studio folder_   

This will cause Windows to try to merge the folders. You will most likely get a prompt asking for admin permission in order to continue. Hit "continue"


_insert pic of UAC prompt_

If windows asks if you would like to merge the folders, you should answer "yes".

You are *not* supposed to move or delete any of the folders in the `obs-studio` folder, they should automatically merge.
If you've messed up, and delete or renamed folders, you should be able to install the latest version of OBS again, and get those folders back.

We're now done, unless the installation instructions from the plugin developer mentioned other extra/special steps. You should now be able to launch OBS Studio and confirm that you have the plugin installed.

Make sure you read any "ReadMe" files or "how to use" steps, in order to understand how the plugin is supposed to be used and/or how to set it up, once installed.
