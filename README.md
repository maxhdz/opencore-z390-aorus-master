# OpenCore-based macOS Catalina Guide
## Gigabyte Z390 Aorus Master / Intel Core i9-9900K / iGPU or Navi dGPU

This is a vanilla build using [this blog post](https://infinitediaries.net/my-2020-hackintosh-hardware-spec/) as a hardware reference and following the [OpenCore guide.](https://desktop.dortania.ml/)

I've forked [cmer's Clover guide](https://github.com/cmer/gigabyte-z390-aorus-master-hackintosh) in order to contribute a guide back and to have a reference for myself. All the thanks in the world to **cmer** for his work and for the USBMap. It has saved me a ton of time!

## STATUS

Fully working.

## HARDWARE

- Fractal Design Meshify C case  
- Seasonic Prime Ultra Platinum 1000W PSU  
- Noctua NH-D15 (CPU cooler)  
- 3x Noctua NF-14 PWM Chromax 140 mm (2x intake on the front, 1x top)  
- 1x Noctua NF-S12A PWM Chromax 120 mm (case rear)  
- Gigabyte Z390 Aorus Master (BIOS F11c)  
- Intel Core i9-9900K  
- 64GB Kingston HyperX Fury DDR4 CL16 3200MHz  
- Gigabyte Aorus Radeon 5700XT Gaming 8GB
- Samsung 970 EVO Plus NVMe M.2 1TB & 500GB  

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
- Onboard BT (HS14 in the USB map) is disabled alongside the built-in WiFI as it conflicts with any add-in card.

Please use an add-in card for these. I no longer recommend the fenvi T919 as many users across reddit and in Amazon reviews have reported conflicts with Z390 motherboards.

### WON'T TEST

- FileVault
- Sleep (has been reported working on r/Hackintosh)

## STEP BY STEP INSTRUCTIONS

The following work with macOS Catalina up to 10.15.4.

### SETUP UEFI/BIOS

* Update the BIOS to the latest version (as of this writing, **F11c**).
* In the BIOS, first *Load Optimized Default Settings*
* Go through the following and make the same changes:

- Tweaker
  - Advanced CPU Settings  
    - Hyper-Threading Technology - Enabled  
    - VT-d - Disabled  

- Settings
  - Platform Power  
    - Platform Power Management - Disabled  
    - ErP - Enabled  

  - IO Ports  
    - Initial Display Output - IGFX (for iGPU) or PCIe 1/2 (whichever slot holds your dGPU)   
    - Internal Graphics - Enabled  
    - DVMT Pre-allocated - 64M  
    - DVMT Total Gfx Mem - 256M  
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
* Follow [this guide](https://desktop.dortania.ml/extras/msr-lock.html) to unlock CFG Lock. Remember that loading the default settings again will flip the CFG Lock back.

### CREATING THE INSTALLER

Follow the [OpenCore Vanilla guide](https://dortania.github.io/OpenCore-Desktop-Guide/installer-guide/). It has instructions that can be followed in both macOS and Windows.

### CONFIGURING OPENCORE

1. Mount the flash disk's EFI partition using `MountEFI`.
2. Dump the contents of [EFI](./EFI) on there.
3. From the [configs](./configs) folder, grab either the Intel iGPU or the Navi dGPU plist, depending on hardware.
4. Rename your selection to `config.plist` and place inside `EFI/OC/` on the flash disk.
5. Edit the `config.plist` to add the following values inside `PlatformInfo > Generic`: **MLB** (Board Serial), **ROM** (your built-in Ethernet's MAC address with no colons), **SystemSerialNumber** (Serial) and **SystemUUID** (SmUUID). These must be [generated with `GenSMBIOS`](https://dortania.github.io/OpenCore-Desktop-Guide/config.plist/coffee-lake.html#platforminfo)! The values inside brackets are the name you'll find them under in GenSMBIOS.
4. Follow the macOS setup steps from the OpenCore Vanilla guide.
5. After install, mount the internal disk's EFI partition.
6. Dump the same contents there.

If your setup is identical to mine, odds are you won't need to mess with KALSR slide values. If boot stops with an error like `Couldn't allocate runtime area`, [please follow this guide](https://desktop.dortania.ml/extras/kaslr-fix.html) to understand what you need to do.

If the iGPU isn't outputting anything, you may need to follow Headkaze's [Intel Framebuffer Patching Guide](https://www.insanelymac.com/forum/topic/334899-intel-framebuffer-patching-using-whatevergreen/?tab=comments#comment-2626271). It took me 14 reboots until I found the right combo, so don't stress.

### DUAL BOOTING

To dual boot macOS & Windows 10, I chose [rEFInd](https://www.rodsbooks.com/refind/) as a boot manager (note, that's **manager** and not **loader**).  

I had macOS installed when I added Windows 10 to a second SSD. Even though this was a completely separate drive to the one holding macOS, the Windows installer added its files to the EFI partition on the first drive and set itself as the default. This is actually helpful!

To set up rEFInd as your boot manager:
1. Mount the EFI partition.  
2. Copy [refind](./refind) inside the `/EFI/` folder on the partition, so that you see `/EFI/OC` and `/EFI/refind` on the same level.  
3. Depending on the OS that is currently booted:  
  a. **Windows 10**: Open a Command Prompt as Adminitrator and type: `bcdedit /set "{bootmgr}" path \EFI\refind\refind_x64.efi`  
  b. **macOS**: Open Terminal.app and type: `sudo bless --mount /Volumes/ESP --setBoot --file /Volumes/ESP/efi/refind/refind_x64.efi --shortform`  
4. Restart and you should see the rEFInd picker. The maCOS option will transfer you to OpenCore while the Windows option will boot Windows 10 directly.

For more in-depth installation instructions, please see the [rEFInd website.](https://www.rodsbooks.com/refind/installing.html)  

If your system clock is different when booting between the two operating systems, follow [these instructions](https://www.tonymacx86.com/threads/fix-incorrect-time-in-windows-osx-dual-boot.133719/) to have UTC time on both operating systems.

#### VERSIONS USED AT TIME OF WRITING

- OpenCore **0.5.7 DEBUG**  
- AppleSupportPkg **2.1.6 RELEASE** (`ApfsDriverLoader.efi`)  
- VirtualSMC **1.1.2 RELEASE** (`VirtualSMC.kext`, `SMCProcessor.kext` & `SMCSuperIO.kext`)  
- Lilu **1.4.3 RELEASE**  
- WhateverGreen **1.3.8 RELEASE**  
- IntelMausi **1.0.2 RELEASE**  
- AppleALC **1.4.8 RELEASE**  
- NVMeFix **1.0.2 RELEASE**  
- USBMap.kext, courtesy of [cmer](https://github.com/cmer)  

Note that I am using a DEBUG version of OpenCore and the `*.plist` files are configured to show useful debug information. This will be invaluable if the system has any issues down the line. 

## USB PORT MAP & SSDT

See [this document](USB_MAP.md) for a map of all the ports on the Z390 Aorus Master made by **cmer**.
