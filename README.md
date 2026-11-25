<div align="center">
  <img src="assets/stela-icon.png" alt="Stela Intel Edition" width="96">
  <h1>Stela — Intel Edition</h1>
  <p><strong>An arm64-to-x86-64 Apple Silicon compatibility layer built for Intel hardware.</strong></p>

  <p>
    <img src="https://img.shields.io/badge/Architecture-Intel_x86--64-0071C5?style=flat-square&logo=intel&logoColor=white" alt="Intel x86-64">
    <img src="https://img.shields.io/badge/Graphics-Intel_Arc_%26_Xe-0071C5?style=flat-square&logo=intel&logoColor=white" alt="Intel Arc & Xe">
    <img src="https://img.shields.io/badge/Target-M2_Mac_mini-111111?style=flat-square&logo=apple&logoColor=white" alt="M2 Mac mini">
    <img src="https://img.shields.io/badge/License-GPL--2.0--or--later-blue?style=flat-square" alt="GPL-2.0-or-later">
  </p>
</div>

---

Stela — Intel Edition is a UEFI boot environment that runs ARM64 macOS on Intel x86-64 PCs. It combines a high-coverage `arm64e`-to-`x86-64` binary translator, an Apple Silicon hardware model (currently presenting as an **M2 Mac mini**), and a native Intel graphics and networking stack.

> [!WARNING]
> This project is experimental and under active development. Performance and hardware compatibility may vary across Intel laptops and desktops.

## What Works

### Binary Translator
- Ahead-of-time (AOT) and just-in-time (JIT) translation from `arm64e` to `x86-64`.
- Covers over **3,000 AArch64 instruction encodings**, direct guest-to-host register mapping, and persistent translation caching.
- Leverages **AVX2** and **AVX-512** when available on the host processor.

### Intel Arc & Xe Graphics
- Native hardware acceleration for integrated and discrete **Intel Arc** (Alchemist, Meteor Lake, Arrow Lake, Battlemage, Lunar Lake) and **Intel Xe (Gen 12+)** GPUs.
- Backed by the Linux **`i915`** and **`xe`** kernel drivers paired with **Mesa ANV** (Vulkan).
- Direct Metal shader compilation from **AIR** to **NIR**, targeting the **Metal3** and **Apple7** GPU families.
- Built-in laptop display support (including high-refresh-rate eDP and OLED panels) via ACPI OpRegion VBT discovery.

### Networking & Peripherals
- **Native Intel Wi-Fi** support via `iwlwifi` (including modern Intel AX211 and other CNVi/PCIe adapters) with full DMA support, configured locally with `--wifi`.
- Wide **Ethernet** support across Intel, Realtek, Broadcom, Aquantia, Qualcomm Atheros, and USB network adapters.
- **Storage & USB:** NVMe, SATA (AHCI), xHCI (USB 3), EHCI, USB hubs, and USB storage devices.
- **Input:** USB keyboards and mice, PS/2 keyboards/mice, and Synaptics/Elantech touchpads.

---

## Hardware Requirements

| Component | Requirement |
| :--- | :--- |
| **CPU** | Intel `x86-64-v2` (Nehalem or newer; **Intel Core 12th Gen+** / **Intel Core Ultra** recommended). |
| **Memory** | 8 GB minimum (**16 GB recommended**). |
| **Firmware** | 64-bit UEFI with **Secure Boot disabled**. |
| **GPU** | Intel Arc (Alchemist, Meteor Lake, Arrow Lake, Battlemage, Lunar Lake) or Intel Xe (Gen 12+) integrated/discrete GPU. |
| **Storage** | NVMe or SATA (AHCI) with at least **60 GB** free (dedicated GPT partition or whole disk). |
| **USB Drive** | 8 GB for the Online installer, or **32 GB** for the Offline installer. |
| **Wi-Fi** | Intel Wi-Fi adapters (`iwlwifi`, configured via `--wifi`). |
| **Ethernet** | **Intel:** I210, I211, I217, I218, I219, I225, I226, I350, 82574L, 82575, 82576, 82579, 82580.<br>**Realtek:** RTL8111, RTL8168, RTL8169, RTL8411, RTL8125, RTL8126, Killer E2600/E3000.<br>**Broadcom:** BCM5720, BCM5761, BCM5762.<br>**Aquantia:** AQC100, AQC107–AQC116C.<br>**Qualcomm Atheros:** AR8131, AR8132, AR8151, AR8152, AR8161, AR8162, AR8171, AR8172, Killer E2200/E2400/E2500.<br>**USB:** Realtek RTL8152/8153/8156, ASIX AX88179/88178A/88772, CDC ECM/NCM. |
| **USB** | xHCI (USB 3) and EHCI controllers, hubs, and mass storage. |
| **Input** | USB keyboards and mice; PS/2 keyboards, mice, and Synaptics/Elantech touchpads. |
| **Bluetooth** | Not supported yet. |
| **Audio** | Not supported yet. |

---

## Build

### Prerequisites
- [Rust](https://rustup.rs) toolchain
- `git`
- Python 3.10 or newer
- LLVM 21, 22, or 23 with Clang, LLD, and `libclang` *(on macOS, Xcode's Apple Clang 21+ also works)*

### Compiling from Source

```sh
git clone [https://github.com/OlegFlow4/Stela-Intel](https://github.com/OlegFlow4/Stela-Intel)
cd Stela-Intel
cargo xtask dist