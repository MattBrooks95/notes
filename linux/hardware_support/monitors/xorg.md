# XORG (deprecated knowledge, Wayland is the future)

an example script to set up two monitors for a desktop computer with one monitor on a display port cable, and another using HDMI
```bash
xrandr \
	--output DP-0 --primary --mode 1920x1080 \
	--output HDMI-0 --mode 1920x1080 --right-of DP-0
```
