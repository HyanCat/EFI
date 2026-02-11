# Hackintosh EFI Configuration for macOS Tahoe

Latest update: 2026-02-11
OpenCore Version: 1.0.6
Supported OS: macOS Ventura (13.x) - macOS Tahoe (26.x)

## Hardware Specifications

- **SMBIOS**: iMac20,1
- **CPU**: Intel Core i5/i7 (Comet Lake / Coffee Lake)
- **iGPU**: Intel UHD Graphics 630 (`0x3E9B0007`)
- **Ethernet**: Realtek RTL8125 2.5GbE
- **WiFi/Bluetooth**: Broadcom BCM4331 (Legacy Airport Card)
- **Storage**: WD Blue SN550 NVMe SSD

## Key Features

- **Bootloader**: OpenCore 1.0.6
- **Audio**: AppleALC (Layout ID: 1)
- **Network**: LucyRTL8125Ethernet (2.5GbE)
- **WiFi**: Legacy Broadcom support (Patched for Sonoma/Tahoe)
- **USB**: Custom mapped (USBMap.kext)

## Installed Kexts

| Kext | Version | Description |
|------|---------|-------------|
| **Lilu** | 1.7.1 | Arbitrary kext and process patching |
| **VirtualSMC** | 1.3.7 | SMC emulator (includes Processor & SuperIO) |
| **WhateverGreen** | 1.7.0 | Graphics patching |
| **AppleALC** | 1.9.6 | Native audio patching |
| **LucyRTL8125Ethernet** | 1.2.2 | Realtek 2.5GbE driver |
| **USBMap** | 1.0 | Custom USB port mapping |
| **NVMeFix** | 1.1.3 | NVMe power management (Disabled on Sequoia/Tahoe) |

### Bluetooth & WiFi (Broadcom Legacy)
| Kext | Version | Notes |
|------|---------|-------|
| **BrcmPatchRAM3** | 2.7.1 | Bluetooth firmware patching |
| **BrcmFirmwareData** | 2.7.1 | Firmware data |
| **BlueToolFixup** | 2.7.1 | Bluetooth fix for Monterey+ |
| **AirportBrcmFixup** | 2.2.0 | WiFi fixup |
| **IOSkywalkFamily** | 1.0 | Legacy WiFi support (MinKernel: 23.0.0) |
| **IO80211FamilyLegacy** | 12.0 | Legacy WiFi support (MinKernel: 23.0.0) |
| **AirPortBrcmNIC** | 1.0 | BCM4331 Driver (MinKernel: 23.0.0) |

## BIOS Settings

- **Secure Boot**: Disabled
- **VT-d**: Enabled (Required for some features)
- **Fast Boot**: Disabled
- **CSM**: Disabled
- **SATA Mode**: AHCI
- **XHCI Hand-off**: Enabled
- **4G Decoding**: Enabled

## Special Configurations

### macOS Sequoia / Tahoe Compatibility
- **SecureBootModel**: `Disabled` (Required for booting installer)
- **XhciPortLimit**: `False`
- **NVMeFix**: Disabled on kernel 24.x+ to prevent installation issues with WD SN550
- **WiFi**: Relies on `IOSkywalkFamily` + `IO80211FamilyLegacy` injection for Sonoma/Tahoe

### Boot Arguments
`-v igfxfw=2 igfxonln=1 alcid=1 gfxrst=1 alcdelay=500 shikigva=80 -cdfon -igfxmpc`

## Known Issues

1. **Audio**: Analog audio via AppleALC may not work on Tahoe due to removal of AppleHDA.
2. **WiFi**: Broadcom legacy cards require `amfi_get_out_of_my_way=1` or AMFIPass if root patching is needed.
3. **DRM**: iGPU DRM might have limitations in Safari.

## Installation Notes

1. Copy `EFI` folder to the EFI partition of your USB drive.
2. Boot from USB and reset NVRAM.
3. Install macOS.
4. For Tahoe: If WiFi doesn't work, apply OCLP root patches or check SIP settings.
