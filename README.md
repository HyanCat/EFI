# Hackintosh EFI Configuration for MSI B460m (Dual Boot: Ventura & Sequoia)

Latest update: 2026-02-12
OpenCore Version: 1.0.6
Supported OS: 
- **macOS Ventura (13.x)** - Fully Functional
- **macOS Sequoia (15.x)** - Fully Functional (Requires OCLP for WiFi)

## Hardware Specifications

- **Motherboard**: MSI B460m
- **CPU**: Intel Core i5 10500 (Comet Lake)
- **iGPU**: Intel UHD Graphics 630 (Headless Mode)
- **dGPU**: AMD RX 570 4GB (Primary)
- **Ethernet**: Realtek RTL8125 2.5GbE
- **WiFi/Bluetooth**: Broadcom BCM4331 (Legacy Airport Card)
- **Storage**: WD Blue SN550 NVMe SSD

## Key Features & Fixes

- **Universal Boot**: Configured to boot **BOTH** Ventura and Sequoia from a single EFI.
- **WiFi (Sequoia)**: Patched via OCLP + `IOSkywalkFamily` (MinKernel=23.0.0).
- **WiFi (Ventura)**: Native support (Patches automatically disabled via MinKernel).
- **Bluetooth**: Working on both OS versions via `BlueToolFixup`, `BrcmFirmwareData`, `BrcmPatchRAM3`.
- **AMFI**: Weakened (`amfi=0x80`) to allow legacy WiFi patches on Sequoia.

## Installed Kexts Status

| Kext | Status | OS Scope | Description |
|------|--------|----------|-------------|
| **Lilu** | Enabled | All | Core patching |
| **VirtualSMC** | Enabled | All | SMC emulation |
| **WhateverGreen** | Enabled | All | Graphics patching |
| **AppleALC** | Enabled | All | Audio patching (Layout 1) |
| **LucyRTL8125Ethernet** | Enabled | All | 2.5GbE Ethernet |
| **USBInjectAll** | Enables | All | USB Port Injection (Safer for Install) |
| **USBMap** | Disabled | - | Disabled in favor of InjectAll for broad compatibility |
| **NVMeFix** | Disabled | - | Disabled to prevent panic with WD SN550 |

### Wireless Networking (Broadcom)

| Kext | Status | OS Scope | Notes |
|------|--------|----------|-------|
| **BrcmPatchRAM3** | Enabled | All | Bluetooth Firmware |
| **BrcmFirmwareData** | Enabled | All | Bluetooth Firmware Data |
| **BlueToolFixup** | Enabled | All | Bluetooth Fix for Monterey+ |
| **BrcmBluetoothInjector** | **Disabled**| - | **DO NOT ENABLE** (Incompatible with Monterey+) |
| **IOSkywalkFamily** | Enabled | Sonoma+ | MinKernel: 23.0.0 (Sequoia Only) |
| **IO80211FamilyLegacy** | Enabled | Sonoma+ | MinKernel: 23.0.0 (Sequoia Only) |
| **AMFIPass** | Enabled | Sonoma+ | MinKernel: 23.0.0 (Helper for AMFI) |

## BIOS Settings (MSI B460m)

**Disable:**
- **Secure Boot**
- **Fast Boot**
- **CSM** (Compatibility Support Module)
- **Intel SGX**

**Enable:**
- **VT-d**
- **Above 4G Decoding**
- **EHCI/XHCI Hand-off**
- **IGPU Multi-Monitor**: Enabled

## Post-Install Guide (Sequoia)

1. **Boot**: Select macOS Sequoia.
2. **WiFi**: If WiFi is missing/broken:
   - Ensure `amfi=0x80` is in boot-args (Check NVRAM).
   - Open **OpenCore Legacy Patcher (OCLP)**.
   - Select "Post-Install Root Patch".
   - Install "Modern Wireless" patch.
   - Reboot.
3. **Ventura**: Should boot natively without any action. WiFi logic is automatically skipped.
