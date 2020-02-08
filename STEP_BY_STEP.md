# Step by step instructions

The following assume you will be installing macOS Catalina.

## SETUP UEFI/BIOS

* Update the BIOS to the latest version (as of this writing, **F11c**)
* Load Optimized Default Settings
* Go through [the BIOS settings document](BIOS_SETTINGS.md) and make all the changes listed.

## CREATING THE INSTALLER

Follow the [OpenCore Vanilla guide](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/installer-guide/opencore-efi.html). It outlines instructions that can be followed in both macOS and Windows.

## CONFIGURING OPENCORE

### EASY

1. Mount the flash disk's EFI partition using `MountEFI`.
2. Dump the contents of [preinstall](./preinstall) on there.
3. Follow the macOS setup steps.
4. After install, mount the internal disk's EFI partition.
5. Dump the contents of [postinstall](./postinstall) on there.

The only change needed with the above files is to add SMBIOS information to the `config.plist`. Use `GenSMBIOS` to generate this information, validate it on the Apple Service & Coverage portal and edit the files using `ProperTree`.

### DIY

1. Read through the [OpenCore Vanilla Desktop Guide.](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/)
2. Read through [this guide](https://khronokernel.github.io/Getting-Started-With-ACPI/) to understand how to patch your SSDTs.
