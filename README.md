# OpenCore-based macOS Catalina Guide
## Gigabyte Z390 AORUS MASTER / Intel Core i9-9900K

This is a vanilla build using [this blog post](https://infinitediaries.net/my-2020-hackintosh-hardware-spec/) as a hardware reference and following the [OpenCore guide.](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide)

I've forked [this repo](https://github.com/cmer/gigabyte-z390-aorus-master-hackintosh) in order to contribute a guide back and to have a reference for myself. All the thanks in the world to **cmer** for his guide and for the USBMap. It has saved me a ton of time!

## STATUS

**WORKING**. Please read through for details.

In the future I will add a dGPU and will update all files accordingly.

## HARDWARE

See the [hardware list.](HARDWARE.md)

### WORKING

- Built-in Audio
- Built-in Ethernet
- iMessage / Handoff / Continuity / Apple Watch Unlock / AirDrop / AirPlay

### NOT WORKING

- Built-in WiFi. This will likely never work and is disabled from BIOS.
- Onboard BT (HS14 in the USB map) is disabled alongside the built-in WiFI as it conflicts with the fenvi add-in card.

Please use an add-in card for these.

#### WON'T TEST

- FileVault
- Sleep

### STEP BY STEP INSTRUCTIONS

See [the step-by-step document.](STEP_BY_STEP.md)

### USB PORT MAP & SSDT

See [this document](USB_MAP.md) for a map of all the ports on the AORUS Z390 MASTER made by **cmer**.

### LAZY?

You are welcome to use my setup in its entirety, but set the needed SMBIOS values in `config.plist`.
