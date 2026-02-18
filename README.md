## Raspberry Pi bootloader for bootc

This project enables bootc based images to be booted on Raspberry Pi based hardware to boot bootc based OSes without needing UEFI chainloaders.

### Why

* The chainloaders don't usually have support for proper devicetree initialization on the pi's without someone self assembling single devicetree files from the overlays for specific boards making them hard to support.
* Support for the newest hardware often lags significant amounts of time. PI5's still dont have reliable support years after release.
* They also lack support for some of the special features of the firmware such as the RPI tryboot mechanism and hardware buttons on the PI header

### Usage

Run `rpi-bootc-bootloader sync` after every bootc switch or bootc update.

Warning: For now, do not use the --apply flag or the updating cronjob as it could leave the bootloader in the wrong state.
