# Repo will be archived till I'll find a way to optimize NVIDIA Optimus on Wayland Session of KDE Plasma 6 (Fedora 40 Release). The old way does not work.
<p align="center">
  <img src="https://i.imgur.com/izi33Tg.png" />
</p>

# NVIDIA Optimus running smoothly on Wayland - detailed guide.

#### Guide works only on Fedora <=39 KDE Plasma 5.X. Tested on Plasma 6 via solopasha COPR Repository - does not work properly - computer is stuttering and V-Sync is broken. Do NOT install, wait for full Fedora 40 KDE Spin release.

This guide was written by ThinkPad and Linux passionate.  The only mistake I've ever done in my entire life was buying a laptop with NVIDIA Mobile Graphics Card - I mean, its performance is awesome, but Linux drivers still sucks.
I was so desperate about that so I discovered a way to use smoothly Intel/NVIDIA laptop on Wayland without swapping laptop to one with AMD Graphics Unit (blessing for Linux user).
## Results of my guide:
- Smooth **Wayland** experience on:
-- External monitors - **first 1920x1080@60Hz** monitor connected _to HDMI port_ of laptop and second one - **2560x1440@144Hz** connected _via Thunderbolt 3 to DisplayPort adapter_ *(Two monitors, both with different resolutions - X11 would V-Sync both of these screens to lowest refresh rate of 60Hz)*
-- Internal ThinkPad laptop monitor **(1920x1080@60Hz)**
-- 2 external screens and laptop screen continously,

- **No more tearing and stuttering** pain that existed on X11 session,
- Intel is used to render UI and everything, NVIDIA is working in "on-demand" mode (Launch games, demanding applications, etc.),
- **Quality of Life improvement** and getting rid of using old X11.

## The configuration I've used to achieve this
- **Lenovo ThinkPad P53 (20QQ)**
-- **Intel Core i7-9850H** @ 2.6GHz, 6 Cores, 12 Threads,
-- **Intel UHD Graphics 630**, 512MB Graphics Memory (Enabled 512M mode in BIOS)
-- **NVIDIA Quadro RTX 5000**, 16GB VRAM (Turing)
-- 64GB DDR4 2666MHz
-- (if necessary) 230W Power supply
-- **BIOS v1.43** 
-- 1920x1080 60Hz monitor, 350 nits, manufacturer - BOA
- **LG ULTRAGEAR 27GP850-B Monitor** (2560x1440@144Hz) connected via DisplayPort
- **EIZO EV2316W Monitor** (1920x1080@60Hz) connected via HDMI
- **Fedora 39 KDE Spin** *(None GNOME, or Sway worked correctly)*

## Required files and systems:

- **Fedora 39 KDE Spin** ([Download from Fedora website, ~2.3GB](https://fedoraproject.org/pl/spins/kde/)),
- *Only for ThinkPad P53:* [BIOS v1.43+](https://support.lenovo.com/us/pl/downloads/ds540999-bios-update-utility-bootable-cd-for-windows-10-64-bit-linux-thinkpad-p53-p73)
-- *For other laptops, **newest available BIOS** from manufacturer.*

## First steps:
### (If you have already Fedora 39 KDE, go to next section)
- Download Fedora 39 KDE Spin from link above,
- Burn ISO on USB Stick by using [Rufus](https://rufus.ie), [GNOME Disk Utility](https://gitlab.gnome.org/GNOME/gnome-disk-utility), [Balena Etcher](https://etcher.balena.io) or anything that can burn ISO on USB,
- Reboot your computer, enter Boot Menu and select your USB-ISO drive,
- Follow the GUI installation of Fedora 39
-- Your drive should contain a: *EFI Partition (can be new or existing) mounted at /boot/efi mountpoint*, *and main partition (ext4 or brtfs) mounted at  / mountpoint.*
- Reboot and enter freshly installed system.

## Next steps:

- **Go to Terminal and enter:**
 ``sudo grubby --update-kernel=ALL --remove-args='quiet'``
 - **Enable RPMFusion Repositories via terminal:**
 -- Free:
  ``sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm``
  -- Nonfree:
   ``sudo dnf install \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm``
  - **Upgrade your entire system by using** ``sudo dnf upgrade --refresh`` (*MAKE SURE IT UPGRADES/DOWNLOADS THE **intel-media-driver** PACKAGE*), **wait for it to end and reboot.**
  - After reboot, **install newest NVIDIA Drivers**:
   ``sudo dnf install gcc kernel-headers kernel-devel akmod-nvidia xorg-x11-drv-nvidia xorg-x11-drv-nvidia-libs xorg-x11-drv-nvidia-libs.i686 htop``
   - **Wait for NVIDIA modules to build**, if successfully built, the command ``modinfo -F version nvidia`` should return driver version. ***If built, reboot.***
     #### Why do we need to install htop?
     Well, htop is not a required package, but it is very useful to monitor if akmod NVIDIA driver is building. If you don't need to monitor that, just don't install htop package.

## Final steps:
- **You'll need to edit the */etc/environment* file:**
`` sudo nano /etc/environment ``
and enter the parameter:
``KWIN_DRM_NO_AMS=1``
At the end, **save the file** *(CTRL key + O, Enter, and CTRL key + X)*
- Restart SDDM by using:
``sudo systemctl restart sddm``
- **At the bottom left, select Plasma (Wayland) session**, then login,
- Your desktop should be butter smooth and work perfectly.


## Still not enough smooth?

- **Edit again your** ``/etc/environment`` **file**:
``sudo nano /etc/environment``,
**add the parameter:**
``KWIN_FORCE_SW_CURSOR=1``
At the end, **save the file** *(CTRL key + O, Enter, and CTRL key + X)*
- Restart SDDM by using:
``sudo systemctl restart sddm``
and login again to system.
## Verify if your laptop is not running always on NVIDIA GPU
Just enter the ``glxinfo | grep "OpenGL"`` command, if it returns the Intel / Mesa driver, *then you're ready to go.*

## How to run my apps with laptop dGPU now? There are two methods
#### First option: use DRI_PRIME=1 variable
- In terminal: use ``DRI_PRIME=1`` before command (example: ``DRI_PRIME=1 telegram-desktop`)
- Any app:  If app offers entering your own Environment Variables, just enter **DRI_PRIME=1** (If using it for Steam, make sure you put **DRI_PRIME=1** before %command%).
#### Second option: use ``__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __GL_SYNC_TO_VBLANK=0`` variable
- In terminal: use **__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __GL_SYNC_TO_VBLANK=0** before command (example: ``__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __GL_SYNC_TO_VBLANK=0 glxgears``)
- Any app: Enter **__NV_PRIME_RENDER_OFFLOAD=1 __GLX_VENDOR_LIBRARY_NAME=nvidia __GL_SYNC_TO_VBLANK=0** in Environment Variables (If using Steam, make sure it is put before %command% in app/game launch options).

## Known issues
#### Not everything is perfect, right? 
- Plasma 6 enabled via solopasha's COPR Repository. It just stutters. Right now finding a way to actually use Plasma 6 already, stay tuned. (Prodably caused by 60hz monitor, will test it later).
- Since NVIDIA's v550.47 driver, system cannot boot normally. Workaround (but considered as issue) - boot without external monitor, then plug it to use it.
-  ~~Cannot wake up from sleep  - "Deadlock" - caused by *Fedora 39's 6.7.X kernel*, we are waiting for next, updated and fixed kernel~~ (Can be bypassed by clicking physical sleep button again)
- ***You tell me (Open issue if there's something wrong or not working).***
- ~~One Reddit user reported very high power usage with this guide, I'm not clearly sure about it, because my ThinkPad P53 normally runs in on-demand mode and does not have any higher power rate, NVIDIA GPU does not work if I don't need it.~~ Tested at 21.03.2024, confirmed that this issue does not exist.
- ~~Long boot time, gets over 1,5 minute~~ Fixed by disabling NetworkManager-wait service.

#### Guide created, verified and tested by Karol from Terminal-Index. Star the repo if it helped you solve the X11 issues.
<p>
  <img src="https://i.imgur.com/jHmBvPX.png" />
</p>
Fedora System Logo is the trademark of Red Hat Inc. Top image font is the Bryant Font, made by Eric Oslon. 
