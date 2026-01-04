
# NVIDIA Optimus running smoothly on Hyprland
#### ✨Check out my pre-built Hyprland .config: [Repository](https://github.com/terminal-index/hyprland-dotfiles) ✨

#### Guide works on every Linux distribution with Hyprland. Tested on many distros (mainly Arch and Debian) screens aren't stuttering and V-Sync is working along with VRR on multiple monitors (Thanks to newest NVIDIA driver update 570!)

This guide was written by ThinkPad and Linux passionate.  The only mistake I've ever done in my entire life was buying a laptop with NVIDIA Mobile Graphics Card - I mean, its performance is awesome, ~~but Linux drivers still sucks~~ apparently ever since I've wrote this guide NVIDIA decided to cook with drivers???
Driver 590 is already pretty damn good - HDR works, VRR works with no flickering screen... I'm shocked. However, drivers are drivers, they still suck in terms of multiple monitors on mobile GPUs.
**That's why I've decided to share my solution for that with you**

## Results of my guide:
- Smooth **Wayland** experience on as many external monitors as you wish. I'm running 4 monitors on my ThinkPad and I didn't had a single stutter or issue with it.
- **No more tearing and stuttering** pain that existed on X11 session,
- NVIDIA GPU is **forced** to do everything. Best solution, everything works fine.
- **Quality of Life improvement** (it no longer runs like ass!!!) and getting rid of using old X11 for that shiny dwindle tiling compositor.

## The configuration I've used to achieve this
- **Lenovo ThinkPad P53 (20QQ)**
-- **Intel UHD Graphics 630**, 512MB Graphics Memory (Enabled 512M mode in BIOS)
-- **NVIDIA Quadro RTX 5000**, 16GB VRAM (Turing)
-- **newest available BIOS from Lenovo** 
-- 4 external monitors

## Required tweaks:
Add few environment variables to your Hyprland configuration, edit your bootloader arguments and you're fine.

## Steps:
- Enter your hyprland config folder:
```bash
cd ~/.config/hypr
```
- Open your file in any editor (vim / nvim / *nano*):
```bash
vim env.conf
# or
nvim env.conf
# or
nano env.conf
```
- Enter these Environment Variables:
```bash
env = LIBVA_DRIVER_NAME,nvidia
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = AQ_DRM_DEVICES,/dev/dri/card1:/dev/dri/card0
### It forces card1 (NVIDIA GPU) to be prioritized over card0 (Intel/AMD iGPU)
env = AQ_FORCE_LINEAR_BLIT,0 #OPTIONAL
### It is a 50/50 chance it'll work fine for you. Try with and without.
### Some people reported that it fixes a lot of things, but in my case
### it ran only worse. 
env = GBM_BACKEND,nvidia-drm
env = ELECTRON_OZONE_PLATFORM_HINT,auto
### Fixes screen flickering in Electron apps
env = NVD_BACKEND,direct
### Allows for VAAPI Hardware Acceleration
```
- Save and quit with **ESC + :wq! + ENTER** on vim/nvim, or **Ctrl+S + Ctrl+X** in nano
- Edit your GRUB bootloader configuration: (I don't use systemd-boot/limine, sorry :C )
```bash
sudo vim /etc/default/grub
# or
sudo nvim /etc/default/grub
# or
sudo nano /etc/default/grub
```
- Insert this command argument at the end of **GRUB_CMDLINE_LINUX**:
```bash
nvidia.NVreg_PreserveVideoMemoryAllocations=1
```
**Here's example of correct entry:**
```bash
GRUB_CMDLINE_LINUX="loglevel=3 quiet splash nvidia.NVreg_PreserveVideoMemoryAllocations=1"
```
- Save your config (**ESC + :wq! + ENTER** on vim/nvim, or **Ctrl+S + Ctrl+X** in nano), then rebuild GRUB configuration:
```bash
# Ubuntu/Debian:
sudo update-grub
# Fedora:
sudo grub2-mkconfig -o /boot/grub/grub.cfg
# Any other distro:
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
- Reboot your computer and use your smooth Hyprland desktop!

## Not working?
Report issues via GitHub.

## Known issues
#### Not everything is perfect, right? 
Actually - none.

#### Guide created, verified and tested by Karol from Terminal-Index. Star the repo if it helped you solve NVIDIA issues on Hyprland.
#### ✨ Screenshot comes from my Hyprland configuration which I share publicly here: [Repository](https://github.com/terminal-index/hyprland-dotfiles) ✨
<p>
  <img src="https://raw.githubusercontent.com/terminal-index/hyprland-dotfiles/refs/heads/main/assets/hyprland.png" />
</p>
