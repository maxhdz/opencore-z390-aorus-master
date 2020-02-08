# OpenCore-based macOS Catalina Guide
## Gigabyte Z390 Aorus Master / Intel Core i9-9900K

This is a vanilla build using [this blog post](https://infinitediaries.net/my-2020-hackintosh-hardware-spec/) as a hardware reference and following the [OpenCore guide.](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide)

I've forked [this repo](https://github.com/cmer/gigabyte-z390-aorus-master-hackintosh) in order to contribute a guide back and to have a reference for myself.

### HARDWARE

See the [hardware list.](HARDWARE.md)

![About My Mac](images/about.png)

#### WORKING

- Ethernet
- Onboard Audio (including digital audio)
- APFS
- Sleep/Wake
- All USB ports at 3.x speed
- iMessage
- App Store
- Facetime
- APFS
- Handoff
- Bluetooth & Wi-Fi (via Broadcom adapter. Also works in UEFI and Clover.)
- Unlock with Apple Watch
- Airdrop
- AirPlay
- Continuity
- Apple Music (iTunes)
- DRM-protected videos in TV app
- Power Nap

#### NOT WORKING

- Built-in WiFi. This will likely never work since it is the new Intel CNVi that MacOS doesn't support.
- Onboard Bluetooth is hit or miss. However, I disabled it (HS14) because I have a natively supported Broadcom BCM94360CS2 WIFI/BT adapter.

#### WON'T TEST

- FileVault

### STEP BY STEP INSTRUCTIONS

See [STEP_BY_STEP.md](STEP_BY_STEP.md)

### USB PORT MAP & SSDT

See [USB_MAP.md](USB_MAP.md) for a map of all the ports on the Aorus Z390 Master.

### LAZY WAY

You are welcome to use my setup in its entirety, but set the needed SMBIOS values in `config.plist`.
