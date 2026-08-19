
# NVIDIA Optimus running smoothly on swaywm / wlroots-based compositors - detailed guide (Currently made for swaywm only, more WMs coming soon).

#### Guide works on any Linux distribution. Tested on few distros (nixOS, Debian 13, Fedora, Artix Linux with dinit)

This guide was written by ThinkPad and Linux passionate.  The only mistake I've ever done in my entire life was buying a laptop with NVIDIA Mobile Graphics Card - I mean, it's performance is awesome, but Linux drivers still sucks (and they always will). I was so desperate about that, so I've discovered a way to use smoothly Intel+NVIDIA laptop on Wayland without swapping laptop to one with AMD/Intel Graphics Unit (blessing for Linux users).

## Expected behavior:
- Smooth **Wayland** experience on:
-- Three external monitors, two with 1920x1080@60Hz, one with 2560x1440@144Hz resolution
-- NVIDIA GPU. That's unbelievable.
-- ~~Internal ThinkPad laptop monitor **(1920x1080@60Hz)**~~  *That's the `wlroots` limitation. Internal display will NOT work when you're running dGPU as a primary GPU.*

- **No more tearing and stuttering** compared to X11 session,
- NVIDIA is used to render UI and everything, it is working in priority mode, Intel GPU isn't working unless you switch to default "sway" session in Login Manager,
- **Quality of Life improvement** and getting rid of X11 (always a win).

## The configuration I've used to achieve this
- **Lenovo ThinkPad P53 (20QQ)**
-- **Intel Core i7-9850H** @ 2.6GHz, 6 Cores, 12 Threads,
-- **Intel UHD Graphics 630**, 512MB Graphics Memory (Enabled 512M mode in BIOS)
-- **NVIDIA Quadro RTX 5000 Max-Q, 80W**, 16GB VRAM (Turing)
-- **Newest available BIOS from Lenovo** 
-- ~~1920x1080 60Hz monitor, 350 nits, manufacturer - BOA~~
- **LG ULTRAGEAR 27GP850-B Monitor** (2560x1440@144Hz) connected via DisplayPort
- 2x **EIZO EV2316W Monitor** (1920x1080@60Hz) connected via HDMI

## Required files and systems:
- Just any Linux distro with `sway` available in repositories (or built manually, it's your choice)
- NVIDIA Drivers (At least version `555`, can be propertiary, preferrably open-source modules via dkms)
## Steps:
- Download both files: `sway-nvidia` and `sway-nvidia.desktop`
- Copy `sway-nvidia.desktop` to /usr/share/wayland-sessions folder:
-- `sudo cp sway-nvidia.desktop /usr/share/wayland-sessions/sway-nvidia.desktop`
- Edit sway-nvidia file if you need to change anything (read it, please)
- Copy `sway-nvidia` file to `~/.local/bin`
-- `cp sway-nvidia ~/.local/bin/sway-nvidia`
- Restart your Login Manager (gdm, sddm, ly, etc.) and switch session to "Sway (NVIDIA)" and you're ready to go.

## Not working?
Report issues via GitHub.

## Known issues
#### Not everything is perfect, right? 
[#2](https://github.com/terminal-index/NVIDIA-Optimus-Wayland/issues/2), but it's fixed. Thanks you for your support!

#### Guide created, verified and tested personally by me. Star the repo if it helped you solve the X11 issues.