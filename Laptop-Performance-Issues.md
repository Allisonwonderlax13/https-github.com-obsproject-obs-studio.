Laptops are difficult to work with when it comes to capture applications like OBS Studio. Most modern laptops will come with two GPUs - typically an Intel GPU for 2D applications and a discrete graphics chip (either NVIDIA or AMD) for 3D apps and games. This means that different applications can be running on either GPU, whereas OBS can only run on a single GPU at a time - an application that is running on the Intel GPU will not be able to be captured by OBS running on the discrete (NVIDIA or AMD) GPU. Additionally, if OBS is not running on the discrete GPU, you might run into performance issues. This is not really an issue with OBS, but rather a design choice by laptop manufacturers in order to save power and there's little that can be done on our side. However, we do have several troubleshooting suggestions to try and assist with any issues.


### If you are getting a black screen with Window or Game Capture Sources or are otherwise having performance issues with OBS on your laptop, read the following:
#### If you have an NVIDIA discrete graphics card
- Close OBS if it is currently open
- Go to the NVIDIA Control Panel by right clicking on your desktop, and then clicking on "NVIDIA Control Panel"
- Click on "Manage 3D Settings" if it is not already selected
- Under the Program Settings tab, click the **Add** button under where it says "Select a program to customize:"
- Navigate to the .exe path for OBS and add it to the list
  - Default paths are: C:\Program Files (x86)\obs-studio\bin\32bit\obs32.exe and C:\Program Files (x86)\obs-studio\bin\64bit\obs64.exe)
- Make sure it is selected in the drop down list
- Then, under where it says "Select the preferred graphics processor for this program" open the drop down and select "High-performance NVIDIA processor"
- Re-open OBS and test again

Alternative directions - this sets the global default so all applications will run on the NVIDIA GPU (higher power consumption):
- Close OBS if it is currently open
- Go to the NVIDIA Control Panel by right clicking on your desktop, and then clicking on "NVIDIA Control Panel"
- Click on Manage 3D Settings on the left, and then the Global tab on the right
- Click the drop down box below that, select "High-performance NVIDIA processor" and click Apply, then OK
- Re-open OBS and test again

**NOTE:**
If you are planning on using QuickSync (QSV), Set QSVHelper.exe to run on the the Intel instead of the NVIDIA.

#### If you have an AMD discrete graphics card
A guide can be found here to set OBS to run on the proper GPU if you are experiencing issues:
- https://community.amd.com/docs/DOC-1581#jive_content_id_Configuring_Switchable_Graphics

### Window and Game Capture and "multi-adapter compatibility" mode
If you cannot set the preferred GPU (AMD laptops typically), or wish to cross-capture an image from the other GPU after that (for example, the League of Legends lobby window), use Window or Game Capture with the "multi-adapter compatibility mode" option enabled to force a capture. "Multi-adapter compatibility mode" requires a bit more CPU usage, however.
Compatibility mode is not recommended for capturing games, but it basically guarantees a capture.

### Special note from Jim
I know it's annoying. I'm not happy that this is the case either. Unfortunately, there's nothing anyone can really do about it. This is just the way laptops are designed in order to save power and battery life.