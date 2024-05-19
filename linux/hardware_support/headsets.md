## SteelSeries
### Arctis Pro 9
- use [headsetcontrol](https://github.com/Sapd/HeadsetControl)
`$ headsetcontrol --capabilities`
```
Found SteelSeries Arctis 9!
Supported capabilities:

* sidetone
* battery status
* inactive time
* chatmix
```
- for now, just use `$ nix-shell -p headsetcontrol`, I don't think I'll use this often enough to put it in the NixOS or Home Manager configuration
- `$ headsetcontrol --capabilities` to list what configurations the tool supports
- turn off the inactive time (to see if it will stop shutting down mid live stream)
`$ headsetcontrol -i 0`
Like others have reported, setting the inactive time to 0 causes the headset to shutdown almost immediately after being turned on. I might have to set it to 90 minutes and just fiddle with the volume to keep it active.

```
Found SteelSeries Arctis 9!
Successfully set inactive time to 0 minutes!
Success!
```

## Addin Udev rules
- without this some commands will fail
- onNixos:
```nix
  # udev rules for headsetcontrol
  services.udev.packages = with pkgs; [
    headsetcontrol
  ];
```
