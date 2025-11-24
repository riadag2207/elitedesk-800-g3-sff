# Instruksi GitHub Copilot untuk Proyek OpenCore EFI

Repositori ini berisi konfigurasi OpenCore EFI untuk build Hackintosh pada **HP EliteDesk 800 G3 SFF** (Intel Kaby Lake i3-7100).

## Struktur & Arsitektur Proyek

- **Root**: `EFI/OC` adalah direktori kerja utama.
- **Konfigurasi**: `EFI/OC/config.plist` adalah file konfigurasi pusat (format XML). Ini mengontrol perilaku boot, patch perangkat keras, dan pemuatan driver.
- **ACPI**: `EFI/OC/ACPI` berisi patch SSDT (`.aml`).
- **Kexts**: `EFI/OC/Kexts` berisi ekstensi kernel macOS (driver).
- **Drivers**: `EFI/OC/Drivers` berisi driver UEFI (`.efi`).
- **Tools**: `EFI/OC/Tools` berisi alat UEFI (Shell, CleanNvram, dll.).

## Alur Kerja Kritis & Konvensi

### 1. Memodifikasi Konfigurasi (`config.plist`)
- **Format**: File ini adalah Daftar Properti XML standar. Pastikan sintaks XML valid saat mengedit.
- **Kepatuhan Skema**: Struktur harus sangat mematuhi skema Konfigurasi OpenCore untuk versi yang digunakan (v1.0.3 sesuai README).
- **Registrasi**: Hanya menambahkan file ke folder **tidak cukup**. Anda harus mendaftarkannya di `config.plist`:
    - **ACPI**: Tambahkan entri ke `ACPI -> Add`.
    - **Kexts**: Tambahkan entri ke `Kernel -> Add`. **Urutan sangat penting** (misalnya, `Lilu.kext` harus pertama, `VirtualSMC.kext` biasanya kedua).
    - **Drivers**: Tambahkan entri ke `UEFI -> Drivers`.
    - **Tools**: Tambahkan entri ke `Misc -> Tools`.

### 2. Manajemen Kext
- **Ketergantungan**: Hormati rantai ketergantungan.
    - `AppleALC`, `WhateverGreen`, `VirtualSMC` bergantung pada `Lilu`.
    - Plugin (misalnya, `SMCProcessor`, `SMCSuperIO`) bergantung pada `VirtualSMC`.
- **Wi-Fi/BT**: Build ini menggunakan `AirportItlwm.kext` (Intel Wi-Fi) dan `IntelBluetoothFirmware.kext`.
- **USB**: Pemetaan USB ditangani oleh `USBToolBox.kext` dan `UTBMap.kext`. Hindari injektor USB generik jika ini ada.

### 3. Patching ACPI
- **SSDT**: SSDT kustom (misalnya, `SSDT-PLUG.aml`, `SSDT-EC-USBX.aml`) digunakan untuk mem-patch tabel ACPI.
- **Quirks**: Pastikan `ACPI -> Quirks` sesuai dengan rekomendasi desktop Kaby Lake (misalnya, `RebaseRegions` biasanya false).

## Detail Perangkat Keras Spesifik
- **Model**: HP EliteDesk 800 G3 SFF
- **CPU**: Intel Core i3-7100 (Kaby Lake)
- **RAM**: 8GB
- **OS Target**: macOS 15.2 (Sequoia)
- **Grafis**: Intel HD Graphics 630 (Headless atau Drive-driving tergantung keberadaan dGPU, biasanya memerlukan patching framebuffer `WhateverGreen`).
- **Ethernet**: Intel I219-LM (menggunakan `IntelMausi.kext`).
- **Audio**: Conexant CX20632 (memerlukan `AppleALC` dengan layout-id spesifik, seringkali `20` atau `28` untuk HP).

## Tugas Umum
- **Update OpenCore**: Memerlukan penggantian `OpenCore.efi`, `BOOTx64.efi`, `Drivers`, dan memvalidasi `config.plist` terhadap `Sample.plist` baru.
- **Ubah Boot Args**: Edit `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`.
