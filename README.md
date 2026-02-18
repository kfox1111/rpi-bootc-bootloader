## Raspberry Pi bootloader for bootc

This project enables bootc based images to be booted on Raspberry Pi based hardware to boot bootc based OSes without needing UEFI chainloaders.

### Why

* The chainloaders don't usually have support for proper devicetree initialization on the pi's without someone self assembling single devicetree files from the overlays for specific boards making them hard to support.
* Support for the newest hardware often lags significant amounts of time. PI5's still don't have reliable support years after release.
* They also lack support for some of the special features of the firmware such as the RPI tryboot mechanism and hardware buttons on the PI header

### Usage

Run `rpi-bootc-bootloader sync` after every `bootc switch` or `bootc update`.

Warning: For now, do not use the --apply flag or the updating cronjob as it could leave the bootloader in the wrong state.

Tryboot:

The tryboot feature is enabled with the `--tryboot` flag, or writing `TRYBOOT=1` to `/etc/rpi-bootc-bootloader/tryboot.conf`

When syncing in tryboot mode, the PI will stay configured to boot the current image. When executing `reboot tryboot`

This will cause the RPI to boot the currently staging bootc image on the next boot. Failure to boot or a power cycle should return it automatically to the current, working image.

### Circuit

To support manually selecting the previous image to boot, we need to add a push button to the RPI header. Connect it to GPIO pin 6 (PI header pin 31), and ground, easiest is PI header pin 39. No resisters are needed.

To fall back to the previous image, simply hold in the button while booting.
