# NetHunter Kernel 4.19 — Xiaomi Redmi Note 7 (lavender)

---

> ⚠️ **DISCLAIMER — READ BEFORE FLASHING**
>
> - **I am not responsible for bricked devices**
> - **Flash at your own risk**
> - **Always take a full backup before flashing**
> - This kernel modifies core system components — proceed only if you know what you are doing

---

## Overview

A **retrofit Linux 4.19** kernel for Xiaomi Redmi Note 7 (codename: **lavender**) with full Kali NetHunter support including WiFi injection, external adapter support, USB arsenal, HID attacks, Bluetooth/RFCOMM, CAN bus, SDR, and systemd chroot compatibility fixes.

> ⚠️ **This is a retrofit kernel.**
> Only compatible with **Android 13 retrofit dynamic partition ROMs** that support kernel 4.19.
> This will **NOT** work on stock or non-retrofit ROMs.

---

## Device Info

| Property | Value |
|----------|-------|
| Device | Xiaomi Redmi Note 7 |
| Codename | lavender |
| Kernel Version | 4.19.288 |
| Supported Android | 13 (Retrofit Dynamic Partition ROMs only) |
| Defconfig | `nethunter_defconfig` |

---

## Variants

| Variant | Description |
|---------|-------------|
| **Non-EROFS** | For ROMs using standard EXT4 system partition |
| **EROFS** | For ROMs using EROFS compressed read-only filesystem |

---

## Features

### ✅ WiFi Packet Injection
- Internal WiFi (qcacld-3.0) packet injection with channel fix enabled
- Monitor mode supported on both internal and external WiFi adapters

### ✅ External WiFi Adapter Support

#### Realtek
| Driver | Supported Chipsets |
|--------|--------------------|
| 88XXAU | RTL8812AU, RTL8821AU, RTL8814AU |
| RTL8187 | RTL8187L/B |
| RTL8192CE | RTL8192CE |
| RTL8188EE | RTL8188EE |
| RTL8192EE | RTL8192EE |
| RTL8821AE | RTL8821AE |
| RTL8192CU | RTL8192CU |
| RTL8XXXU | RTL8723BU, RTL8188EU, RTL8192EU and more |
| RTL8150 | RTL8150 USB ethernet |
| RTL8152 | RTL8152/RTL8153 USB ethernet |

#### Ralink / MediaTek
| Driver | Supported Chipsets |
|--------|--------------------|
| RT2500USB | RT2500USB |
| RT2800USB | RT2800, RT3300, RT3500, RT3573, RT5300, RT5500 and unknown variants |
| MT7601U | MT7601U |
| MT76x0U | MT7610U |
| MT76x2U | MT7612U, MT7602U, MT7662U |

#### Atheros
| Driver | Supported Chipsets |
|--------|--------------------|
| ATH9K_HTC | AR9271, AR7010 |
| ATH6KL USB | AR6003, AR6004 |

#### Zydas
| Driver | Supported Chipsets |
|--------|--------------------|
| ZD1211RW | ZD1211, ZD1211B |
| USB_ZD1201 | ZD1201 |

---

### ✅ USB Arsenal
- USB HID keyboard/mouse emulation enabled
- USB Mass storage gadget enabled
- USB ACM / Serial gadget enabled
- USB RNDIS / ECM / NCM / EEM network gadgets enabled
- USB MIDI gadget enabled
- USB FunctionFS enabled
- USB MTP / PTP enabled
- USB OTG support enabled
- USB Serial adapters: CH341, FTDI SIO enabled

---

### ✅ HID Support
- Full USB HID support enabled
- UHID (userspace HID) enabled
- HID raw access enabled
- HID multitouch enabled
- HID battery strength reporting enabled
- HID over Bluetooth (HIDP) enabled
- Xbox controller with force feedback enabled

---

### ✅ Bluetooth & RFCOMM
- Bluetooth BR/EDR + LE enabled
- RFCOMM (serial over Bluetooth) enabled
- RFCOMM TTY support enabled
- BNEP (Bluetooth network encapsulation) enabled
- HID over Bluetooth (HIDP) enabled
- Bluetooth High Speed (HS) enabled
- HCI over USB, UART enabled
- Broadcom, Realtek, Intel Bluetooth USB firmware support enabled
- Virtual HCI (VHCI) enabled
- Internal Qualcomm Bluetooth (hci0) enabled

---

### ✅ CAN Bus Support
- CAN RAW, BCM, Gateway, VCAN enabled
- CAN SLCAN, CAN ISOTP added as modules (see modules section below)
- USB CAN adapters enabled: EMS, ESD, GS_USB, KVASER, PEAK, 8DEV, SOFTING
- CAN controllers enabled: SJA1000, C_CAN, M_CAN, CC770, MCP251X, MCP25XXFD, HI311X, IFI CANFD
- CAN GRCAN, Xilinx CAN enabled

---

### ✅ NFC Support
- NFC core with NCI, HCI, SHDLC enabled
- PN533 USB, PORT100 USB NFC readers enabled
- NFC simulation support enabled

---

### ✅ Software Defined Radio (SDR)
- AirSpy enabled
- HackRF enabled
- RTL2832 SDR enabled

> **Note:** SDR support is enabled at the kernel level. Full functionality depends on compatible SDR software installed in your chroot or Android environment.

---

### ✅ Systemd / NetHunter Chroot Fix
Statx() attributes backported from upstream Linux — fixes the notorious systemd machine-id error when upgrading packages inside the Kali NetHunter chroot.
This error no longer occurs when upgrading systemd or any other package inside the NetHunter chroot. Tested by upgrading systemd from 260.1-1 to latest.

---

### ✅ Other Features
- WireGuard VPN enabled
- TCP BBR congestion control enabled (default)
- Full iptables / netfilter support enabled
- IPv4 + IPv6 enabled
- Tethering (RNDIS, ECM, NCM) enabled
- VPN support enabled (L2TP, PPP, WireGuard, IPSec)

---

## Loadable Kernel Modules

| Module | Modprobe Command | Description |
|--------|-----------------|-------------|
| RTL8812AU / RTL8821AU | `modprobe 88xxau` | Most popular external adapter for injection |
| CAN SLCAN | `modprobe slcan` | USB-to-CAN serial line adapter |
| CAN ISOTP | `modprobe can-isotp` | ISO 15765-2 transport protocol for CAN |
| USB-CAN-2 | `modprobe hlcan` | USB CAN v2 adapter |

```bash
# From root terminal on device:
modprobe 88xxau
modprobe slcan
modprobe can-isotp
modprobe hlcan

# Verify
lsmod | grep <module_name>
find /system/lib/modules -name "*.ko" 2>/dev/null
find /vendor/lib/modules -name "*.ko" 2>/dev/null
```

---

## How to Flash

> ⚠️ **Magisk must be installed before flashing this kernel.**
> Flash Magisk first, then flash the kernel zip on top.

1. Boot into custom recovery (TWRP or OrangeFox)
2. Flash your **Android 13 retrofit dynamic partition ROM** first
3. Flash **Magisk** for root access
4. Flash the kernel zip matching your ROM variant:
   - **Non-EROFS ROM** → flash non-erofs kernel zip
   - **EROFS ROM** → flash erofs kernel zip
5. Reboot

---

## Building from Source

```bash
git clone https://github.com/Cross988/Nethunter_4.19 -b arrow-13.1
git submodule update --init --recursive
make ARCH=arm64 nethunter_defconfig
make ARCH=arm64 CROSS_COMPILE=<your-toolchain-prefix> -j$(nproc)
```

---

## Credits

- **qcacld-3.0 injection patches** — original authors
- **CAN modules** — [V0lk3n](https://github.com/V0lk3n)
- **statx backport** — upstream Linux kernel / NetErnels
- **Base kernel** — [gyhxrr31/Nethunter_4.19](https://github.com/gyhxrr31/Nethunter_4.19)

---

## Disclaimer

This kernel is provided as-is for educational and security research purposes via Kali NetHunter.
Use responsibly and only on networks you own or have explicit permission to test.
