# MILO · Nexys A7 DOS PC

### A small FPGA board. A real x86 machine. Your own DOS installation.

An ao486-based PC built for the **Digilent Nexys A7-100T**, with DDR2-backed memory, VGA output, sound, SD storage and a hardware POST display. This share kit brings together the accepted firmware, its matching source and the tools needed to prepare your own DOS card.

**Share kit:** October 13, 2026 · **Accepted firmware:** `20261011-A7-AWARD-POST`

> **Bring your own DOS.** Microsoft MS-DOS 6.22, installation floppies, installed disk images and commercial applications are not included. Use your own licensed copy to create `DOS622.VHD`.

---

## The machine

| Component | Accepted configuration |
| :--- | :--- |
| Board | Digilent Nexys A7-100T · Artix-7 `xc7a100t-csg324-1` |
| Processor | ao486 x86 core at **57.142857 MHz** |
| Guest memory | **64 MiB**, backed by onboard DDR2 |
| L2 cache | **128 KiB**, write-through |
| Video | VGA output · 256 KiB video memory · 800 × 600, 16 colors at 56.25 Hz |
| Storage | Fixed DOS VHD stored contiguously on a FAT16 microSD card |
| Audio | Sound Blaster-compatible implementation and OPL3 synthesis; tested paths recorded in the qualification evidence |
| Keyboard | Compatible wired USB Boot-HID keyboard through the onboard USB-to-PS/2 bridge |
| Indicators | Two-digit BIOS POST codes, three-digit A7 fault codes and blue disk activity |

The BIOS is the locally modified **MiSTer/Bochs BIOS**. Its POST checkpoints follow the Award 4.51PG convention; this project does not install Award BIOS.

## Start here

You will need:

- A **Nexys A7-100T**, its programming USB cable and a suitable power source.
- A VGA monitor and a compatible wired USB keyboard.
- A **4–32 GB microSDHC card** and a computer with an SD-card reader.
- Your own licensed **MS-DOS 6.22** installation media.
- **Python 3.10 or later**, with the Windows Python launcher for the `.cmd` helpers.
- A VM supporting fixed VHD, legacy BIOS, IDE and floppy installation if you need to install DOS.
- [AMD Vivado](https://www.amd.com/en/products/software/adaptive-socs-and-fpgas/vivado.html) with Artix-7 support to program the FPGA, and [balenaEtcher](https://etcher.balena.io/) to write the SD image.

The packaged firmware targets the **100T board**. It is not an A7-50T image.

### 1. Prepare your licensed DOS disk

If you already have a compatible, bootable fixed DOS 6.22 VHD, copy it here:

```text
media/DOS622.VHD
```

The name must be exactly **`DOS622.VHD`**, including the extension. Enable filename extensions in Windows Explorer so it does not become `DOS622.VHD.txt`.

If you need to create the disk, double-click **`CREATE_BLANK_VHD.cmd`**. It creates an empty fixed VHD with the expected geometry; it contains no DOS files and is not yet bootable. Attach it as an existing IDE hard disk in your VM and install DOS from your licensed media.

Boot the installed VHD in the VM, run `VER` to confirm MS-DOS 6.22, then shut the VM down completely. Read [DOS-AND-SD-CARD.txt](DOS-AND-SD-CARD.txt) for the complete procedure.

> The file in [media/DOS622.VHD.PLACEHOLDER.txt](media/DOS622.VHD.PLACEHOLDER.txt) is an explanation, not a disk image. Renaming a placeholder, floppy image, ISO, VDI or VHDX to `.VHD` will not make it work.

### 2. Build the SD image

Double-click **`BUILD_SD_CARD.cmd`**, or run this in PowerShell from the extracted kit folder:

```powershell
py -3 tools\build_sd_card.py build --vhd media\DOS622.VHD --output A7-SD.img
```

The builder checks the fixed-VHD footer, disk geometry, DOS partition and presence of the boot files. It then creates a **2 GiB SD image** containing:

```text
SD image: A7-SD.img
└── FAT16 partition · label A7DOS
    └── DOS622.VHD · contiguous allocation
```

It reads the embedded VHD back and checks its SHA-256 against the copied data. It writes a new image file on your computer, **not a physical drive**, and refuses to overwrite an existing output.

The DOS VHD needs an active primary FAT16 partition, installed DOS boot code, `IO.SYS`, `MSDOS.SYS` and `COMMAND.COM`. The supplied blank creator chooses a compatible size and 16-head/63-sector geometry. Copying boot-file names onto an unbootable disk is not a DOS installation.

### 3. Write your card

In Etcher, select **A7-SD.img → your removable SD card → Flash**. Writing the image replaces that card's contents. Let verification finish, then safely eject it.

Do not copy `A7-SD.img` onto the card as an ordinary file. Do not reformat the written partition as FAT32/exFAT or expand it to fill the card. Spare capacity on larger cards is intentional. Rebuild the image when replacing the VHD so its required contiguous allocation is preserved.

### 4. Load the FPGA and boot

For the first test, use Vivado Hardware Manager to program the board with:

```text
firmware/boot_diag.bit
```

This is a volatile JTAG load. Once your board and DOS card work, follow [FPGA-PROGRAMMING.txt](FPGA-PROGRAMMING.txt) to install the persistent QSPI image:

```text
firmware/A7-AWARD-POST.mcs
```

Power the board off before inserting the card. Connect the monitor and supported keyboard, select the appropriate board configuration mode and boot. **The SD card holds DOS; onboard QSPI holds the FPGA configuration.**

## Reading the POST display

Normal byte codes appear on the **two rightmost digits**. Unused digits stay off; A7 hardware faults use a third digit.

| Code | Stage |
| :---: | :--- |
| `09` | Cache initialization |
| `0C` | Keyboard-controller initialization |
| `0D` | VGA BIOS initialization |
| `31` | Conventional RAM quick check |
| `42` | Hard-disk initialization and ATA detection |
| `63` | Boot attempt |
| **`FF`** | Handoff to a validated boot vector |

`FF` remains visible after DOS starts unless software writes a new code to port `80h`. It marks the boot handoff; it does not certify every peripheral or disk health. See the [complete POST-code guide](docs/A7-POST-CODES.md) for all stages and hardware fault meanings.

## What's inside

```text
A7-Share-20261013/
├── README.md                   This guide
├── BUILD_SD_CARD.cmd           Windows SD-image builder launcher
├── CREATE_BLANK_VHD.cmd         Empty fixed-VHD creator launcher
├── DOS-AND-SD-CARD.txt          Licensed DOS installation and card preparation
├── FPGA-PROGRAMMING.txt         JTAG and persistent QSPI instructions
├── SOURCE-BUILD.txt             FPGA and ROM rebuild instructions
├── firmware/                   Accepted bitstreams, MCS and routed checkpoint
├── source/                     Matching RTL, constraints, donor source and tests
├── roms/                       Selected BIOS/VGA ROMs and their modified source
├── scripts/                    FPGA build wrapper and ROM rebuild helper
├── tools/                      VHD checker, SD builder and checksum verifier
├── tests/                      Synthetic SD-builder tests
├── diagnostics/                Optional DOS diagnostic and benchmark sources
├── qualification/              Timing, hardware, flash and packaging evidence
├── docs/                       Architecture, POST guides and development history
├── licenses/                   License texts and share-tool license
├── media/                      DOS-image placeholder; supply your own VHD
├── SOURCE-EXPORT.json          Source hashes, provenance and recorded omissions
└── SHA256SUMS.txt               Per-file checksums
```

The source comes from the protected accepted release. Older disk images, upstream prebuilt releases, private development metadata and generated AMD memory-IP outputs were excluded. Text `.vhd` files in the donor tree are **VHDL hardware source**, not virtual hard disks.

## Verification and current limits

The original board's acceptance records include FPGA timing checks, exact flash readback, boot from QSPI, five warm reboots across pre/postflash qualification, and CPU, graphics, audio and binary disk-copy checks. Display simulation covered all 256 byte codes, and the owner confirmed the two-digit `FF` display.

For this share kit, the SD builder passed six synthetic tests, both ROMs rebuilt byte-for-byte from the supplied source, and the DDR2 memory IP regenerated using Vivado 2026.1. Archive CRC and per-file hash checks are also recorded. **These records describe the original board and package checks; test your own board and DOS installation.** The Windows launchers were checked, but the builder's runtime tests were performed on Linux.

Known limits remain:

- **Storage:** an intermittent long SD write-stall is unresolved. Keep a backup of your VHD. This kit does not contain the rejected disk-reliability firmware or claim a filesystem repair.
- **Wireless input:** the inspected Logi Bolt receiver does not work through the A7's onboard PIC24 bridge. Standalone wireless keyboard support is not included; use a compatible wired keyboard.
- **Mouse:** the physical mouse input is not connected in this image.
- **CPU compatibility:** an inherited far TSS `CALL` probe stall remains unresolved; see the [CPU note](qualification/BASELINE-CPU-TSS-NOTE.md).
- **Coverage:** POST checks conventional RAM, not all extended memory. Games have not been qualified, and graphics checks do not constitute blanket tear-free certification.

Read [KNOWN-LIMITATIONS.json](qualification/KNOWN-LIMITATIONS.json) and [FLASH-ACCEPTANCE.json](qualification/FLASH-ACCEPTANCE.json) for the precise scope of the evidence.

## Rebuild from source

Rebuilding is optional—the accepted binaries are supplied. The recorded FPGA build used **Vivado 2026.1 on Linux**. From the top-level kit folder:

```bash
# Regenerate the DDR2 memory interface using your Vivado installation.
vivado -mode batch -source source/scripts/generate_mig.tcl

# Build into a new directory using the supplied qualified ROMs.
vivado -mode batch -source scripts/rebuild_scanout.tcl -tclargs roms build-share
```

With GCC, make and bin86/dev86 tools installed, rebuild the actual modified ROM sources:

```bash
python3 scripts/rebuild_roms.py --output rebuilt-roms
```

The ROM helper performs a clean rebuild and checks the result against the accepted ROM bytes. The FPGA wrapper applies the recorded timing gates. A fresh FPGA build needs its own hardware qualification. See [SOURCE-BUILD.txt](SOURCE-BUILD.txt) for details and the distinction between current build helpers and historical lab scripts.

## Download from Milo to Windows

Run in **Windows PowerShell**:

```powershell
scp milo@192.168.1.84:/home/milo/share/A7-Share-20261013.zip "$env:USERPROFILE\Downloads\A7-Share-20261013.zip"
```

To download the archive checksum and calculate your local hash:

```powershell
scp milo@192.168.1.84:/home/milo/share/A7-Share-20261013.zip.sha256 "$env:USERPROFILE\Downloads\A7-Share-20261013.zip.sha256"
Get-FileHash "$env:USERPROFILE\Downloads\A7-Share-20261013.zip" -Algorithm SHA256
```

Compare the hash with the downloaded checksum file. Extract the ZIP with **Extract All**. To verify the extracted kit from its top-level folder:

```powershell
py -3 tools\verify_package.py
```

The address above is Milo's LAN address when the kit was prepared. Use the host's current address if it changes, and your existing SSH credentials. No credentials are included in the kit.

## Credits and licenses

This project builds on **Aleksander Osman's ao486**, **MiSTer contributors**, the **Bochs/LGPL VGA BIOS projects**, **Greg Taylor and the OPL3 contributors**, and **Digilent's board constraints and documentation**. Original headers, attribution and applicable license texts are retained.

This is a collection with multiple licenses, not a single relicensed codebase. See [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt), the [license directory](licenses/) and individual source headers. Generated AMD memory IP is rebuilt under the recipient's applicable tool/IP terms.

**Share the project kit. Supply and retain your own licensed DOS installation separately.**
