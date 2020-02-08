# Step by step instructions

The following steps assume you will be installing macOS Catalina.

## SETUP UEFI/BIOS

* Update the BIOS to the latest version (as of this writing, F11c)
* Load Optimized Default Settings
* Go through [BIOS SETTINGS](BIOS_SETTINGS.md) and make all the changes listed.

## CREATING THE USB FLASH DRIVE

Follow the [OpenCore Vanilla guide](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/installer-guide/opencore-efi.html). It outlines instructions that can be followed in both macOS and Windows.

## Configuring Clover

You need to create your own `config.plist` file. Start with the sample file I included in `assets/coffeelake_sample_config.plist`. This file was taken from /r/hackintosh's Vanilla Guide. Its latest version can always be found [on GitHub](https://github.com/corpnewt/Hackintosh-Guide/blob/master/Configs/CoffeeLake/config.plist)

Here's a [great explanation of the Clover settings for Coffee Lake](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/config.plist-per-hardware/coffee-lake) if you want to better understand what's going on.

1. Open `coffeelake_sample_config.plist` with Clover Configurator (right click → Open With → Clover Configurator)


 In **SMBIOS**:
    - Click the button with an up/down arrow (middle right). Chose `iMac19,1`. This is important since we'll be connecting our monitor to the RX580. The HDMI port on our motherboard should NOT be used on your Hackintosh.
    - Make sure the serial number generated is an iMac (Retina 5k, 2019) by clicking `Model Lookup`.
    - Ensure that `Check Coverage` reports that the serial is **NOT** valid. You don't want to use somebody else's serial number.
    - While you're here, copy your Board Serial Number to your clipboard. You'll need it soon.
* In **Rt Variables**:
    - Paste your Board Serial Number in the `MLB` field.
    - <del>Set `CsrActiveConfig` to `0x0` which enables SIP for extra security. This should work just fine for a Vanilla Hackintosh install and is how genuine Macs ship.</del>
    - Set `CsrActiveConfig` to `0x3e7` to disable SIP. This is unfortunately required as of 10.14.5 in order to load unsigned KEXTs.
* In **Boot**:
    - Make sure you have the following Arguments:
        - `debug=0x100` -- show debug screen instead of standard kernel panic screen.
        - `keepsyms=1` -- works with debug=0x100, show symbols on debug screen.
        - `dart=0` -- disables Intel Virtual Technology.
        - `slide=0` -- kernel address management.
        - `darkwake=8` -- sleep management (IgnoreDiskIOAlways).
        - `shikigva=32` -- video decoding stuff.
        - `alcid=16` -- this enables your onboard audio. Use `-alcoff` to disable audio.
        - `-v` -- boot in verbose mode. I personally remove this after my system is stable.

* Copy your newly generated `config.plist` to `/EFI/CLOVER/` on your bootable USB key.

## Kexts

All Kexts should be copied to `/EFI/CLOVER/kexts/Other`. Whenever copying kexts from an online source, always make sure to copy the **Release** version (as opposed to Debug) if both are included in your download.

We need a few Kexts to get our installation working as it should:

* [IntelMausiEthernet.kext](https://bitbucket.org/RehabMan/os-x-intel-network/downloads/)
    -  This is for our onboard Ethernet/LAN adapter
* [Lilu.kext](https://github.com/acidanthera/Lilu/releases)
    -  Arbitrary kext and process patching on macOS
* [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases)
    -  Various patches necessary for certain ATI/AMD/Intel/Nvidia GPUs
* [VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC/releases)
    - Advanced Apple SMC emulator in the kernel
    - In addition to `VirtualSMC.kext`, I use `SMCProcessor.kext` (enables temperature monitoring) and `SMCSuperIO.kext` (enables fan speed reading).

## Other Potential Kexts

* [NoVPAJpeg.kext](https://github.com/vulgo/NoVPAJpeg/releases)
    - ~~Use this only if you haven't enabled your iGPU. I recommend enabling the iGPU instead of this.~~
    - ~~Note: this kext is **no longer required under Catalina**, but was required with Mojave. If you're on Catalina, do not install it.~~
    - This kext has been deprecated. Use `shikigva=32 shiki-id=Mac-7BA5B2D9E42DDD94` in your boot arguments instead.

* [USBInjectAll.kext](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/)
    - This is necessary to create your own USB SSDT (see below). Otherwise, do not use.


## How to fix all your USB issues.

[Here I made a quick video explaining how to generate your own USB SSDT/kext](https://youtu.be/j3V7szXZZTc). This will fix all your USB issues such as slow speed, drives getting ejected on wake, etc. You should **absolutely** either use my `USBMap.kext` file (from my EFI folder), or generate your own. My file will only work with **exactly** the same motherboard model and revision as mine. In doubt, just make your own.



## Credit where credit is due

* [/r/hackintosh's awesome Vanilla Install guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/)
* MacPeet at [hackintosh-forum.de](https://www.hackintosh-forum.de) for a modified `AppleALC.kext`. (Now released as version 1.3.4)
* See [RESOURCES.md](RESOURCES.md) for other threads, sites, posts I used to get this to work.

## Status

I consider this guide complete and finished. Many have used it successfully already. I will continously update these documents as needed.
