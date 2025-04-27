
# NVIDIA Optimus running smoothly on Wayland - detailed guide.

#### Guide works on every Linux distribution with GNOME 45+. Tested on many distros (Ubuntu, Fedora, Arch, openSUSE) - works as it should work - screens aren't stuttering and V-Sync is working along with VRR on multiple monitors (Thanks to newest NVIDIA driver update 570).

This guide was written by ThinkPad and Linux passionate.  The only mistake I've ever done in my entire life was buying a laptop with NVIDIA Mobile Graphics Card - I mean, its performance is awesome, but Linux drivers still sucks.
I was so desperate about that so I discovered a way to use smoothly Intel/NVIDIA laptop on Wayland without swapping laptop to one with AMD Graphics Unit (blessing for Linux user).
## Results of my guide:
- Smooth **Wayland** experience on:
-- External monitors - **first 1920x1080@60Hz** monitor connected _to HDMI port_ of laptop and second one - **2560x1440@144Hz** connected _via Thunderbolt 3 to DisplayPort adapter_ *(Two monitors, both with different resolutions - X11 would V-Sync both of these screens to lowest refresh rate of 60Hz)*
-- Internal ThinkPad laptop monitor **(1920x1080@60Hz)**
-- 2 external screens and laptop screen continously,

- **No more tearing and stuttering** pain that existed on X11 session,
- Intel is used to render UI and everything, NVIDIA is working in priority mode, intel GPU isn't working unless you switch to "GNOME (NVIDIA) session" in Login Manager,
- **Quality of Life improvement** and getting rid of using old X11.

## The configuration I've used to achieve this
- **Lenovo ThinkPad P53 (20QQ)**
-- **Intel Core i7-9850H** @ 2.6GHz, 6 Cores, 12 Threads,
-- **Intel UHD Graphics 630**, 512MB Graphics Memory (Enabled 512M mode in BIOS)
-- **NVIDIA Quadro RTX 5000**, 16GB VRAM (Turing)
-- **newest available BIOS from Lenovo** 
-- 1920x1080 60Hz monitor, 350 nits, manufacturer - BOA
- **LG ULTRAGEAR 27GP850-B Monitor** (2560x1440@144Hz) connected via DisplayPort
- **EIZO EV2316W Monitor** (1920x1080@60Hz) connected via HDMI

## Required files and systems:
- Just any Linux distro with GNOME 45+
- Newest NVIDIA Drivers (Can be propertiary, can be open-source moduled)
## Steps:
- Download "gnome-nvidia.desktop"
- Copy file to /usr/share/wayland-sessions
- Reboot and switch session to "GNOME (NVIDIA)" and you're ready to go.

## Not working?
Report issues via GitHub.

## Known issues
#### Not everything is perfect, right? 
Actually - none.

#### Guide created, verified and tested by Karol from Terminal-Index. Star the repo if it helped you solve the X11 issues.
<p>
  <img src="https://i.imgur.com/jHmBvPX.png" />
</p>
