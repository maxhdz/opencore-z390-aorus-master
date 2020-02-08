# BIOS config

Before you start, make sure to upgrade your BIOS to the latest version (**F11c**) and **Load Optimized Defaults**.

## DISABLE

- Fast Boot
- VT-d (can be enabled if you set `DisableIoMapper` to YES)
- CSM
- Thunderbolt
- Intel SGX
- Intel Platform Trust
- CFG Lock (MSR 0xE2 write protection)
- Serial Port
- Parallel Port
- LED lighting
- Legacy USB

## ENABLE

- VT-x
- Above 4G decoding
- Hyper-Threading
- Execute Disable Bit
- EHCI/XHCI Hand-off
- OS type: Windows 8.1/10 UEFI Mode
- Legacy RTC Device

## M.I.T

### Advanced Frequency Settings

* Extreme Memory Profile (XMP): Profile1

# Overclocking

These are the additional settings used by the original guide author to overclock the CPU to 5.1GHz.

## M.I.T

### Advanced Frequency Settings

* CPU Clock Ratio: 51
* Extreme Memory Profile (XMP): Profile1

#### Advanced CPU Core Settings
* Uncore Ratio: 47
* Package Power Limit1 - TDP (Watts): 4090
* Package Power Limit2 (Watts): 4090
* Platform Power Limit1 (Watts): 4090
* Platform Power Limit2 (Watts): 4090
* CPU Enhanced Halt (C1E): Disabled
* C3 State Support: Disabled
* C6/C7 State Support: Disabled
* C8 State Support: Disabled
* C10 State Support: Disabled

### Advanced Memory Settings
  * Extreme Memory Profile (XMP): Profile1

### Advanced Voltage Settings

#### Advanced Power Settings
* CPU Internal AC/DC Load Line: Turbo
* CPU Vcore Loadline Calibration: Turbo
* VAXG Loadline Calibration: Turbo
* CPU Vcore Current Protection: Turbo
* VAXG Current Protection: Turbo

#### CPU Core Voltage Control
* CPU Vcore: 1.370V
