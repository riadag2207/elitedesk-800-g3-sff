# GitHub Copilot Instructions for OpenCore EFI Project

This repository contains the OpenCore EFI configuration for a Hackintosh build on an **HP EliteDesk 800 G3 SFF** (Intel Kaby Lake i3-7100).

## Project Structure & Architecture

- **Root**: `EFI/OC` is the main working directory.
- **Configuration**: `EFI/OC/config.plist` is the central configuration file (XML format). It controls boot behavior, hardware patches, and driver loading.
- **ACPI**: `EFI/OC/ACPI` contains SSDT patches (`.aml`).
- **Kexts**: `EFI/OC/Kexts` contains macOS kernel extensions (drivers).
- **Drivers**: `EFI/OC/Drivers` contains UEFI drivers (`.efi`).
- **Tools**: `EFI/OC/Tools` contains UEFI tools (Shell, CleanNvram, etc.).

## Critical Workflows & Conventions

### 1. Modifying Configuration (`config.plist`)
- **Format**: The file is a standard XML Property List. Ensure valid XML syntax when editing.
- **Schema Compliance**: The structure must strictly adhere to the OpenCore Configuration schema for the specific version in use (approx. v1.0.3).
- **Registration**: Simply adding a file to a folder is **not sufficient**. You must register it in `config.plist`:
    - **ACPI**: Add entry to `ACPI -> Add`.
    - **Kexts**: Add entry to `Kernel -> Add`. **Order is critical** (e.g., `Lilu.kext` must be first, `VirtualSMC.kext` usually second).
    - **Drivers**: Add entry to `UEFI -> Drivers`.
    - **Tools**: Add entry to `Misc -> Tools`.

### 2. Kext Management
- **Dependencies**: Respect dependency chains.
    - `AppleALC`, `WhateverGreen`, `VirtualSMC` depend on `Lilu`.
    - Plugins (e.g., `SMCProcessor`, `SMCSuperIO`) depend on `VirtualSMC`.
- **Wi-Fi/BT**: This build uses `AirportItlwm.kext` (Intel Wi-Fi) and `IntelBluetoothFirmware.kext`.
- **USB**: USB mapping is handled by `USBToolBox.kext` and `UTBMap.kext`. Avoid generic USB injectors if these are present.

### 3. ACPI Patching
- **SSDTs**: Custom SSDTs (e.g., `SSDT-PLUG.aml`, `SSDT-EC-USBX.aml`) are used to patch ACPI tables.
- **Quirks**: Ensure `ACPI -> Quirks` match the Kaby Lake desktop recommendations (e.g., `RebaseRegions` is usually false).

## Specific Hardware Details
- **Model**: HP EliteDesk 800 G3 SFF
- **CPU**: Intel Core i3-7100 (Kaby Lake)
- **Graphics**: Intel HD Graphics 630 (Headless or Drive-driving depending on dGPU presence, usually requires `WhateverGreen` framebuffer patching).
- **Ethernet**: Intel I219-LM (uses `IntelMausi.kext`).
- **Audio**: Conexant CX20632 (requires `AppleALC` with specific layout-id, often `20` or `28` for HP).

## Common Tasks
- **Update OpenCore**: Requires replacing `OpenCore.efi`, `BOOTx64.efi`, `Drivers`, and validating `config.plist` against the new `Sample.plist`.
- **Change Boot Args**: Edit `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`.
