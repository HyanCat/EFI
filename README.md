# Hackintosh EFI Configuration for MSI B460m (Dual Boot: Ventura & Sequoia)

Latest update: 2026-02-13
OpenCore Version: 1.0.6
Supported OS: 
- **macOS Ventura (13.x)** - Fully Functional
- **macOS Sequoia (15.x)** - Fully Functional (Requires OCLP Root Patch)

## Hardware Specifications

- **Motherboard**: MSI B460m
- **CPU**: Intel Core i5 10500 (Comet Lake)
- **iGPU**: Intel UHD Graphics 630 (Headless Mode)
- **dGPU**: AMD RX 570 4GB (Primary)
- **Ethernet**: Realtek RTL8125 2.5GbE
- **WiFi/Bluetooth**: Broadcom BCM94360CS2 (Modern Wireless Card)
- **Storage**: WD Blue SN550 NVMe SSD

## Key Features & Fixes

- **Universal Boot**: Configured to boot **BOTH** Ventura and Sequoia from a single EFI.
- **WiFi (Sequoia)**: Patched via OCLP + `IOSkywalkFamily` + `AirPortBrcmNIC` (MinKernel=23.0.0).
- **WiFi (Ventura)**: Native support (Patches automatically disabled via MinKernel).
- **Bluetooth**: Working on both OS versions via `BlueToolFixup`, `BrcmFirmwareData`, `BrcmPatchRAM3`.
- **AMFI Bypass**: Modern approach using `AMFIPass.kext` and `-amfipassbeta` instead of `amfi=0x80` for better system stability and secure features (TouchID, local auth).

## Installed Kexts Status

| Kext | Status | OS Scope | Description |
|------|--------|----------|-------------|
| **Lilu** | Enabled | All | Core patching |
| **VirtualSMC** | Enabled | All | SMC emulation |
| **WhateverGreen** | Enabled | All | Graphics patching |
| **AppleALC** | Enabled | All | Audio patching (Layout 1) |
| **LucyRTL8125Ethernet** | Enabled | All | 2.5GbE Ethernet |
| **USBInjectAll** | Enabled | All | USB Port Injection (Safer for Install) |
| **USBMap** | Disabled | - | Disabled in favor of InjectAll for broad compatibility |
| **NVMeFix** | Disabled | - | Disabled to prevent panic with WD SN550 |

### Wireless Networking (Broadcom BCM94360CS2)

| Kext | Status | OS Scope | Notes |
|------|--------|----------|-------|
| **BrcmPatchRAM3** | Enabled | All | Bluetooth Firmware |
| **BrcmFirmwareData** | Enabled | All | Bluetooth Firmware Data |
| **BlueToolFixup** | Enabled | All | Bluetooth Fix for Monterey+ |
| **IOSkywalkFamily** | Enabled | Sonoma+ | MinKernel: 23.0.0 (Sequoia Only) |
| **IO80211FamilyLegacy** | Enabled | Sonoma+ | Core legacy stack |
| **AirPortBrcmNIC** | Enabled | Sonoma+ | Specifically for BCM94360CS2 |
| **AMFIPass** | Enabled | Sonoma+ | Allows OCLP patches without full AMFI disable |

## BIOS Settings (MSI B460m)

**Disable:**
- **Secure Boot**
- **Fast Boot**
- **CSM** (Compatibility Support Module)
- **Intel SGX**
- **VT-d** (Recommended for BBCM94360CS2 stability unless AppleVTD is specifically required)

**Enable:**
- **Above 4G Decoding**
- **EHCI/XHCI Hand-off**
- **IGPU Multi-Monitor**: Enabled

## Post-Install Guide (Sequoia)

1. **Boot**: Select macOS Sequoia.
2. **WiFi**: If WiFi is missing/broken:
   - Ensure `-amfipassbeta` is in boot-args.
   - Open **OpenCore Legacy Patcher (OCLP)** version 2.0.0+.
   - Select "Post-Install Root Patch".
   - The app should detect "Modern Wireless" (Broadcom).
   - Select "Start Root Patching" and follow prompts.
   - Reboot.
3. **Ventura**: Should boot natively without any action. WiFi legacy logic is automatically skipped via MinKernel filters.
