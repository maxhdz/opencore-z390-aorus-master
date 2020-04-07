# OpenCore-based macOS Catalina Guide
## Gigabyte Z390 Aorus Master / Intel Core i9-9900K

This is a vanilla build using [this blog post](https://infinitediaries.net/my-2020-hackintosh-hardware-spec/) as a hardware reference and following the [OpenCore guide.](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide)

I've forked [cmer's Clover guide](https://github.com/cmer/gigabyte-z390-aorus-master-hackintosh) in order to contribute a guide back and to have a reference for myself. All the thanks in the world to **cmer** for his work and for the USBMap. It has saved me a ton of time!

## STATUS

**WORKING** 

Please read through for details.

In the future I will add a dGPU and will update all files accordingly.

## HARDWARE

- Fractal Design Meshify C case  
- Seasonic Prime Ultra Platinum 1000W PSU  
- Noctua NH-D15 (CPU cooler)  
- 3x Noctua NF-14 PWM Chromax 140 mm (2x intake on the front, 1x top)  
- 1x Noctua NF-S12A PWM Chromax 120 mm (case rear)  
- Gigabyte Z390 Aorus Master (BIOS F11c)  
- Intel Core i9-9900K  
- 64GB Kingston HyperX Fury DDR4 CL16 3200MHz  
- Samsung 970 EVO Plus NVMe M.2 1TB  
- fenvi T919 (BCM94360CD) WiFi & BT 4.0  

### ASSEMBLY TIPS

- I recommend the case coolers be mounted first. Try to picture the layout of the motherboard and how you would connect the coolers to it.  
- If using the same case, try to think in advance how to route the PSU cables, since it sits at the bottom.  
- I would also add the CPU heatsink / cooler as the very last step.  

### WORKING

- CPU Power Management  
- Built-in Audio  
- Built-in Ethernet  
- USB ports on the top of the case and most on the back  
- iMessage / Handoff / Continuity / Apple Watch Unlock / AirDrop / AirPlay  

### NOT WORKING

- Built-in WiFi. This will likely never work and is disabled from BIOS.
- Onboard BT (HS14 in the USB map) is disabled alongside the built-in WiFI as it conflicts with the fenvi add-in card.

Please use an add-in card for these.

### WON'T TEST

- FileVault
- Sleep (has been reported working on r/Hackintosh)

## STEP BY STEP INSTRUCTIONS

The following assume you will be installing macOS Catalina 10.15.3, but should be compatible with 10.15.4.

### SETUP UEFI/BIOS

* Update the BIOS to the latest version (as of this writing, **F11c**).
* Follow [this guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/msr-lock) to unlock CFG Lock.
* In the BIOS, first *Load Optimized Default Settings*
* Go through the following and make the same changes:

- Tweaker
  - Advanced CPU Settings  
    - Hyper-Threading Technology - Enabled  
    - VT-d - Disabled  
  - Extreme Memory Profile - Profile 1

- Settings
  - Platform Power  
    - Platform Power Management - Disabled  
    - ErP - Enabled  

  - IO Ports  
    - Initial Display Output - IGFX  
    - Internal Graphics - Enabled  
    - DVMT Pre-allocated - 64M  
    - DVMT Total Gfx Mem - MAX  
    - WiFi - Disabled  
    - Above 4G Decoding - Enabled  
    - USB Configuration  
      - Legacy USB Support - Disabled  
      - XHCI Hand-off - Enabled  
      - Port 60/64 Emulation - Disabled  
    - Network Stack Configuration  
      - Network Stack - Disabled  

  - Miscellaneous  
    - LEDs in Power On - Off  
    - LEDs in Power Off - Off  
    - Intel Platform Trust Technology - Disabled  
    - Software Guard Extensions - Disabled  
    - Trusted Computing  
      - Security Device Support - Disable  

- Boot
  - Fast Boot - Disabled  
  - Windows 8/10 Features - Windows 8/10  
  - CSM Support - Disabled  
  - Secure Boot  
    - Secure Boot Enable - Disabled  

* Save & Exit Setup.

### OVERCLOCK?

I followed [this guide](https://forums.bit-tech.net/index.php?threads/9900k-5ghz-1-2v-guide-gigabyte-z390-master.353729/) to reach 5GHz and can recommend these tweaks.

### CREATING THE INSTALLER

Follow the [OpenCore Vanilla guide](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/installer-guide/opencore-efi.html). It has instructions that can be followed in both macOS and Windows.

### CONFIGURING OPENCORE

#### EASY WAY

1. Mount the flash disk's EFI partition using `MountEFI`.
2. Dump the contents of [EFI](./EFI) on there.
3. Follow the macOS setup steps from the OpenCore Vanilla guide.
4. After install, mount the internal disk's EFI partition.
5. Dump the same contents there.

If your setup is identical to mine, odds are you won't need to mess with KALSR slide values, as the one in `config.plist` will work. If boot stops with an error like `Couldn't allocate runtime area`, [please follow this guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/kalsr-fix) to understand what you need to do.

If the iGPU isn't outputting anything, you may need to follow Headkaze's [Intel Framebuffer Patching Guide](https://www.insanelymac.com/forum/topic/334899-intel-framebuffer-patching-using-whatevergreen/?tab=comments#comment-2626271). It took me 14 reboots until I found the right combo, so don't stress.

The only change needed with the `EFI` files is to add SMBIOS information to the `config.plist`. Use `GenSMBIOS` to generate `iMac19,1` information, validate it on the Apple Service & Coverage portal and edit the files using `ProperTree`. The three values that need editing are under `PlatformInfo -> Generic`: `MLB`, `SystemSerialNumber` and `SystemUUID`. 

#### HARD WAY

1. Read through the [OpenCore Vanilla Desktop Guide.](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/)
2. Read through [this guide](https://khronokernel.github.io/Getting-Started-With-ACPI/) to understand how to patch your SSDTs.

#### VERSIONS USED AT TIME OF WRITING

- OpenCore w/ OpenCanopy **0.5.7 RELEASE**
- AppleSupportPkg **2.1.6 RELEASE** (`ApfsDriverLoader.efi`)
- VirtualSMC **1.1.2 RELEASE** (`VirtualSMC.kext`, `SMCProcessor.kext` & `SMCSuperIO.kext`)
- Lilu **1.4.3 RELEASE**
- WhateverGreen **1.3.8 RELEASE**
- IntelMausi **1.0.2 RELEASE**
- AppleALC **1.4.8 RELEASE**
- custom USBMap.kext created by [cmer](https://github.com/cmer)

## USB PORT MAP & SSDT

See [this document](USB_MAP.md) for a map of all the ports on the Z390 Aorus Master made by **cmer**.

## LAZY?

You are welcome to use my setup in its entirety, but set the needed SMBIOS values in `config.plist`.
