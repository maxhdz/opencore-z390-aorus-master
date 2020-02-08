# OpenCore-based macOS Catalina Guide
## Gigabyte Z390 AORUS MASTER / Intel Core i9-9900K

This is a vanilla build using [this blog post](https://infinitediaries.net/my-2020-hackintosh-hardware-spec/) as a hardware reference and following the [OpenCore guide.](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide)

I've forked [this repo](https://github.com/cmer/gigabyte-z390-aorus-master-hackintosh) in order to contribute a guide back and to have a reference for myself.

### HARDWARE

See the [hardware list.](HARDWARE.md)

#### WORKING

TBD

#### NOT WORKING

- Built-in WiFi. This will likely never work.
- Onboard BT is disabled (HS14 in the USB map).

Please use an add-in card for these.

#### WON'T TEST

- FileVault

### STEP BY STEP INSTRUCTIONS

See [the step-by-step document.](STEP_BY_STEP.md)

### USB PORT MAP & SSDT

See [this document](USB_MAP.md) for a map of all the ports on the AORUS Z390 MASTER.

### LAZY?

You are welcome to use my setup in its entirety, but set the needed SMBIOS values in `config.plist`.
