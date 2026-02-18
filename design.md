## Bootloader limitations

* must be a variant of a FAT filesystem. Atomic updates are not supported.
* Kernel and initrd need to be on that filesystem
* If supporting all 64bit supporting pi's, then tryboot_a_b=1 doesnt work.
* autoboot.txt does not support includes
* if not using a/b partitions, firmware can not be switched between partitions for all pi models
* if an include points at a file that doesnt exist, it continues as if it was blank

## Requirements

* As atomic as possible
* kernel, initrd, cmdline, device tree files and overlays follow the image
* image provided config.txt options also included if possible
* Falling back to previous image on failed update should be either automatic or very simple without a head or debugging port
* Simple hardware for a failback button/option
* Support hardware watchdoging if supported

## Other bits

* GPIO pin 6 (Physical Pin 31) is pulled high by default so can be connected directly to ground through a switch safely
* GPIO pin 6 (Physical Pin 31) is not used in the GeeekPi TPM header which uses the first 26 Physical Pins.
* sudo vcmailbox 0x00030064 4 4 0 # Get tryboot current flag state for next boot. ends in 1 for true, 0 for false.
* cat /proc/device-tree/chosen/bootloader/tryboot # 1 for currently in a tryboot
* sudo vcmailbox 0x00038064 4 4 1 # Set tryboot to 1

## We manage 4 files

* tryboot.txt
* config.txt
* config-bootc-default.txt
* config-bootc-fallback.txt

* User manages config-boot-common.txt and rpi-config.txt 

## Tryboot Configuration

The tryboot setting controls how staged updates are handled:

* **TRYBOOT=0** (default): Staged updates are automatically applied on next boot
* **TRYBOOT=1**: Staged updates use the tryboot mechanism for safer rollback

### Configuration Methods

The tryboot setting can be configured in three ways (in order of precedence):

1. **Command-line flag** (highest priority):
   ```
   rpi-bootc-bootloader sync --tryboot    # Enable tryboot mode (TRYBOOT=1)
   rpi-bootc-bootloader sync              # Use config file or default
   ```

2. **Configuration file** at `/etc/rpi-bootc-bootloader/tryboot.conf`:
   ```
   TRYBOOT=0
   ```
   or
   ```
   TRYBOOT=1
   ```

3. **Default value**: If neither the command-line flag nor config file is present, defaults to `TRYBOOT=0`

