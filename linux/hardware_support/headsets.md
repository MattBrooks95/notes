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

## Addin Udev rules
- without this some commands will fail
- onNixos:
```nix
  # udev rules for headsetcontrol
  services.udev.packages = with pkgs; [
    headsetcontrol
  ];
```
