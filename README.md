# Hackintosh EFI Configuration for macOS Tahoe (Sequoia Ready)

Latest update: 2026-02-11
OpenCore Version: 1.0.6
Supported OS: macOS Ventura (13.x) - macOS Sequoia (15.x) / Tahoe (26.x)

## Hardware Specifications

- **SMBIOS**: iMac20,1
- **CPU**: Intel Core i5 10500 (Comet Lake)
- **iGPU**: Intel UHD Graphics 630 (`0x3E9B0007`) - Configured as Headless (`0x3E9B0003`)
- **Ethernet**: Realtek RTL8125 2.5GbE
- **WiFi/Bluetooth**: Broadcom BCM4331 (Legacy Airport Card) - **Disabled for Installation**
- **Storage**: WD Blue SN550 NVMe SSD
- **GPU**: AMD RX 570 4GB

## Key Features

- **Bootloader**: OpenCore 1.0.6
- **Audio**: AppleALC (Layout ID: 1)
- **Network**: LucyRTL8125Ethernet (2.5GbE)
- **WiFi/BT**: Temporarily disabled to ensure safe installation of Sequoia.
- **USB**: Custom mapped (USBMap.kext)

## Installed Kexts

| Kext | Version | Status | Description |
|------|---------|--------|-------------|
| **Lilu** | 1.7.1 | Enabled | Arbitrary kext and process patching |
| **VirtualSMC** | 1.3.7 | Enabled | SMC emulator |
| **WhateverGreen** | 1.7.0 | Enabled | Graphics patching |
| **AppleALC** | 1.9.6 | Enabled | Native audio patching |
| **LucyRTL8125Ethernet** | 1.2.2 | Enabled | Realtek 2.5GbE driver |
| **USBMap** | 1.0 | Enabled | Custom USB port mapping |
| **NVMeFix** | 1.1.3 | **Disabled** | Disabled for Sequoia stability (WD SN550) |

### Bluetooth & WiFi (Broadcom Legacy)
**NOTE**: All Wireless kexts are DISABLED for installation stability. Enable them post-install + OCLP.

| Kext | Status | Notes |
|------|--------|-------|
| **BrcmPatchRAM3** | Disabled | Bluetooth firmware patching |
| **BlueToolFixup** | Disabled | Bluetooth fix for Monterey+ |
| **AirportBrcmFixup** | Disabled | WiFi fixup |
| **IOSkywalkFamily** | Disabled | Legacy WiFi support |

## BIOS Settings (MSI B460m)

**Disable:**
- **Secure Boot**
- **Fast Boot**
- **CSM** (Compatibility Support Module)
- **Intel SGX**
- **CFG Lock** (If available, otherwise Quirk `AppleXcpmCfgLock` handles it)

**Enable:**
- **VT-d**
- **Above 4G Decoding**
- **Hyper-Threading**
- **EHCI/XHCI Hand-off**
- **OS Type**: Windows UEFI
- **IGPU Multi-Monitor**: Enabled (for Headless iGPU)

## Installation Guide (Sequoia)

1. **Prepare USB**: Copy `EFI` folder to the EFI partition of your USB drive.
2. **Reset NVRAM**: Boot from USB, select "Reset NVRAM" first.
3. **Install**: Boot "Install macOS Sequoia".
4. **Post-Install**:
   - Once installed, you can attempt to re-enable WiFi/BT kexts.
   - You will likely need OpenCore Legacy Patcher (OCLP) for Broadcom WiFi on Sequoia.
