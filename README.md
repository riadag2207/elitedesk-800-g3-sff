# Hackintosh HP EliteDesk 800 G3 SFF

Repositori ini berisi konfigurasi OpenCore EFI untuk menjalankan macOS pada HP EliteDesk 800 G3 Small Form Factor.

## Spesifikasi Perangkat Keras

| Komponen | Detail |
| --- | --- |
| **Model** | HP EliteDesk 800 G3 SFF |
| **CPU** | Intel Core i3-7100 (Kaby Lake) |
| **RAM** | 8GB DDR4 |
| **Grafis** | Intel HD Graphics 630 |
| **Ethernet** | Intel I219-LM |
| **Audio** | Conexant CX20632 |

## Perangkat Lunak

- **Bootloader**: OpenCore 1.0.3
- **macOS**: macOS Sequoia 15.2

## Pengaturan BIOS

Pastikan pengaturan BIOS berikut diterapkan sebelum booting:

1.  **Advanced -> Boot Options**:
    - Uncheck *Fast Boot*.
2.  **Advanced -> Secure Boot Configuration**:
    - Pilih *Legacy Support Enable and Secure Boot Disable*.
3.  **Advanced -> System Options**:
    - Check *Virtualization Technology (VTx)*.
    - Uncheck *Virtualization Technology for Directed I/O (VTd)* (Disarankan, meskipun config.plist mungkin menangani ini).
4.  **Advanced -> Built-in Device Options**:
    - Video memory size: *64MB* atau lebih besar.

## Fitur

- [x] Akselerasi Grafis (Intel HD 630)
- [x] Audio (Speaker Internal & Jack)
- [x] Ethernet (Intel Mausi)
- [x] USB Ports (Dipetakan)
- [x] Manajemen Daya (CPU Power Management)

## Catatan Penting

1.  **PlatformInfo**: Anda **harus** membuat Serial Number, MLB, dan UUID baru menggunakan [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) sebelum login ke iCloud. Gunakan SMBIOS `iMac18,1`.
2.  **Wi-Fi**: EFI ini menyertakan `AirportItlwm.kext`.

## Kredit

- [Acidanthera](https://github.com/acidanthera) untuk OpenCore dan Kexts.
- [Dortania](https://dortania.github.io/) untuk panduan OpenCore.

