# ProDesk600G1SFF-Hackintosh-10.9-EFI
## based on https://github.com/1alessandro1/HP-Prodesk-600-G1-SFF-macOS

A repo containing the OpenCore EFI setup to run OS X Mavericks 10.9.5 on an HP ProDesk 600 G1 SFF.

All items contained herein remain property of their original creators.

**This repo will remain static at OpenCore v0.6.9, do not expect these files to be up to date!**

# You should read the [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
By using this repo's files you are agreeing that **you will not be able to get help on the /r/Hackintosh Discord.**

If you would like help, please follow the guide and create your own OpenCore configuration from scratch.

## Specs

| Component      | Brand                                     |
|----------------|-------------------------------------------|
| **CPU**        | `Intel Core i5-4590 @ 3.3 GHz`            |
| **Chipset**    | `Intel Q85`                               |
| **iGPU**       | `Intel HD Graphics 4600 - Haswell`        |
| **Audio**      | `Realtek ALC221 - layout 11`              |
| **Ethernet**   | `Intel I217LM`                            |
| **OS**         | `OS X Mavericks 10.9.5 (13F1911)`           |
| **BIOS**       | `2.78 - 29 April 2020`                    |

## Important notes
- In the `config.plist`, section `PlatformInfo > Generic`, the following fields are currently edited with "CHANGEME" in order to force the user to generate his own serials. Refer to this guide to [know how](https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#platforminfo). 
  - `MLB`
  - `ROM`
  - `SystemSerialNumber` 
  - `SystemUUID`

- If you are planning an NVMe device on the PCI-E x16 or x1 slots, note that on Aptio IV firmware, NvmeExpressDxe is not present, check the `Drivers/` folder under `EFI/OC/` path of this repository, and don't forget to add `NvmeExpressDxe.efi` that to the `ConnectDrivers` section in OpenCore's `config.plist`. If you don't plan to use an NVMe, you can skip this and remove NvmeExpressDxe from the `Drivers/` folder and from the `config.plist`

- This repo assumes your machine has the current BIOS from HP, version 2.78. Install it from https://support.hp.com/us-en/drivers/selfservice/hp-prodesk-600-g1-small-form-factor-pc/5387447

## Additional hidden BIOS settings
The tool `setup_var` from [datasone](https://github.com/datasone/grub-mod-setup_var/releases/latest) can help you to setup this additional parameters to successfully boot macOS:

Instructions: copy `modGRUBShell.efi` to a FAT32 partition and save it under the directory `ESP/EFI/BOOT/` as `BOOTx64.efi`, your firmware will target that `.efi` driver to boot, and once you reached the GRUB command line, type the following (these offsets apply to this firmware in particular with this BIOS version, 2.78)

- CFG Lock: `setup_var 0x4A3 0x00` [Disabled]
- DVMT GFX total mem: `setup_var 0x234 0x3` [MAX]
- EHCI Hand-off: `setup_var 0x2 0x1` [Enabled]
- Serial ports: [Disable All]
  - (Serial Port A) `setup_var 0x13 0x00`
  - (Serial Port B) `setup_var 0x15 0x00`
  - (Serial Port C) `setup_var 0x17 0x00`
  - (Serial Port D) `setup_var 0x19 0x00`


## Important note about Ethernet
**Security Update 2016-004 is supposedly *required* for working Ethernet, though I had it working without. Find it at https://support.apple.com/kb/dl1886?locale=en_US**

## Important note about Graphics output
**VGA output will not function under any version of Mac OS X, you need to use the DisplayPort output. The machine will not boot if a VGA cable is connected.**

## [MountEFI](https://github.com/corpnewt/MountEFI) errors out! What do I do?
Update Python to version 2.7.18 at https://www.python.org/ftp/python/2.7.18/python-2.7.18-macosx10.9.pkg
