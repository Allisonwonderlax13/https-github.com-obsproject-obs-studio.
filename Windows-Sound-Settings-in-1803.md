An upcoming Windows 10 feature update contains changes to configuring sound. This is a WIP guide to help users use the full extent of its power.

> Sound Settings are moving to Settings: You can now change some of your common sound settings, such as switching devices and troubleshooting, in Settings > System > Sound. Head on over for a preview of how sound settings are moving out of the legacy Control Panel and into the Settings app. We still have some things to add and fix on this page, so stay tuned! [Source](https://blogs.windows.com/windowsexperience/2017/11/08/announcing-windows-10-insider-preview-build-17035-pc/#fAGsyK2Z5rg88zRD.97).

> Continuing our work to make the new Sound Settings page a one stop shop for your audio needs, we’ve made some more design tweaks and have added some more helpful links. We’ve also added a brand new “App volume and device preferences” page to help you customize your audio experience to best fit your needs and preferences! Please explore these pages and share your feedback as we continuously work on improving them. [Source](https://blogs.windows.com/windowsexperience/2018/01/11/announcing-windows-10-insider-preview-build-17074-pc/#775SUx07GWhZqywu.97).

***

# The Basics

* The Volume Mixer hasn't moved and it hasn't changed - it works exactly as it has since Windows Vista, allowing you to set volumes per-application for each audio device in a small, resizable window
* Everything related to the new options are available in the Windows 10 **Settings** app, found in the Start menu - Settings -> System -> Sound.

## The new capabilities

Windows 10 1803 allows you to set audio Input and Output devices for each running application directly via the system, even if the application's settings don't support it. It is a native, official way to route audio to individual devices. This replaces tools like AudioRouter.

## Usage

* Open your Start menu and click on Settings. If you're not sure which button that is, you can click the button on the top left with the 3 horizontal lines (the "hamburger button") and it'll show labels.
* Click on System, then in the left sidebar select Sound.
* To Do

![App volume and device preferences window](https://i.imgur.com/ew1zIA9.png)

## Why this is useful

The most common use case for routing application audio to different devices is in combination with virtual cables, allowing you to individually control volumes for Discord, your game audio, and music (for example) directly within OBS Studio. This can also be used to play certain audio just to your headset or speakers while everything else is sent to OBS.

## Do I still need virtual cables or Voicemeeter Banana?

Yes. Unfortunately, at this time Windows does not include ways to natively create virtual outputs or to route audio to **applications **specifically. The most commonly recommended (and free!) application for this is [Voicemeeter Banana](https://www.vb-audio.com/Voicemeeter/banana.htm), which provides 2 virtual inputs and an easy-to-use control panel. Alternatively, [VB-CABLE](https://www.vb-audio.com/Cable/) allows you to get one virtual outputs for free, and [two more if you donate](https://www.vb-audio.com/Cable/#DownloadCable) - but without the control panel.

Providing virtual cables like this natively via OBS Studio is often requested because some alternative streaming applications do this, however it would require a lot of work and is very low on the list of priorities compared to other features & fixes.

# Things to mention

* ~~Why this is useful for me as an OBS user?~~
* ~~What does it allow me to do?~~
* ~~Why do I still need VAC or Voicemeeter Banana? ("Why can't OBS just do that?")~~
* What are the alternatives if I'm not on the latest build of Windows 10?
* How do I reach the old audio settings?
* What are the oddities in this new system? (Duplicate entries)

![Sound Properties - App preferences link](https://i.imgur.com/Yf2CYKC.png)