# Step by step instructions

The following assume you will be installing macOS Catalina.

## SETUP UEFI/BIOS

* Update the BIOS to the latest version (as of this writing, **F11c**).
* Follow [this guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/msr-lock) to unlock CFG Lock.
* In the BIOS, first *Load Optimized Default Settings*
* Go through [the BIOS settings document](BIOS_SETTINGS.md) and make all the changes listed.
* Save & Exit Setup.

## CREATING THE INSTALLER

Follow the [OpenCore Vanilla guide](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/installer-guide/opencore-efi.html). It has instructions that can be followed in both macOS and Windows.

## CONFIGURING OPENCORE

### EASY

1. Mount the flash disk's EFI partition using `MountEFI`.
2. Dump the contents of [EFI](./EFI) on there.
3. Follow the macOS setup steps from the OpenCore Vanilla guide.
4. After install, mount the internal disk's EFI partition.
5. Dump the same contents there.

If your setup is identical to mine, odds are you won't need to mess with KALSR slide values, as the one in `config.plist` will work. If boot stops with an error like `Couldn't allocate runtime area`, [please follow this guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/kalsr-fix) to understand what you need to do.

If the iGPU isn't outputting anything, you may need to follow Headkaze's [Intel Framebuffer Patching Guide](https://www.insanelymac.com/forum/topic/334899-intel-framebuffer-patching-using-whatevergreen/?tab=comments#comment-2626271). It took me 14 reboots until I found the right combo, so don't stress.

The only change needed with the `EFI` files is to add SMBIOS information to the `config.plist`. Use `GenSMBIOS` to generate `iMac19,1` information, validate it on the Apple Service & Coverage portal and edit the files using `ProperTree`. The three values that need editing are under `PlatformInfo -> Generic`: `MLB`, `SystemSerialNumber` and `SystemUUID`. 

### DIY / HARD

1. Read through the [OpenCore Vanilla Desktop Guide.](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/)
2. Read through [this guide](https://khronokernel.github.io/Getting-Started-With-ACPI/) to understand how to patch your SSDTs.
