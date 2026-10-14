# MILO · Nexys A7 DOS PC

### A small FPGA board. A real x86 machine. Your own DOS installation.

An **ao486-based PC built for the Digilent Nexys A7-100T**, featuring DDR2-backed memory, VGA video output, sound support, SD card storage, and a hardware POST display.

This project brings a classic DOS PC experience to modern FPGA hardware. The release package includes the accepted firmware, matching source code, and the tools needed to prepare your own DOS installation.

The goal is to create a functional, standalone FPGA-based x86 computer capable of running real DOS software, classic applications, and games.

## Current Performance and Compatibility

The system is operational and successfully running DOS applications and games. Development is ongoing, with continued improvements to performance, sound compatibility, and overall stability.

### DOOM (1993)

**DOOM runs great!** Gameplay is smooth, and overall performance is excellent with audio disabled. The game is fully playable, and the remaining challenge is getting sound working properly.

### DOOM II (1994)

**DOOM II is running, but performance still needs improvement.** The game currently works with audio disabled, although it runs noticeably slower than the original DOOM.

The system is not quite where it needs to be yet for consistently smooth DOOM II gameplay. Further optimization is needed to improve performance and playability.

### Sound Support

Sound works great in other DOS programs, demonstrating that the audio hardware and software implementation are functioning.

However, both DOOM and DOOM II currently require audio to be disabled for successful gameplay. Resolving this compatibility issue remains an active development priority.

## Project Status

This is a working FPGA-based DOS PC, not just a demonstration or simulation. It uses the ao486 x86-compatible processor core implemented in FPGA hardware.

The project has reached the point where DOS boots, software executes, video output works, sound is functional in supported applications, and classic games are playable.

There is still room for improvement, particularly with demanding games like DOOM II, but the system continues to make excellent progress.

## Release Information

**Share Kit Date:** October 13, 2026  
**Accepted Firmware:** `20261011-A7-AWARD-POST`  
**Target Hardware:** Digilent Nexys A7-100T  
**CPU Core:** ao486  
**Operating System:** MS-DOS 6.22 (user supplied)

The release package brings together the accepted firmware, its matching source, and the supporting tools needed to prepare an SD card and build your own DOS installation.

## Bring Your Own DOS

Microsoft MS-DOS 6.22, installation floppy images, preinstalled disk images, and commercial applications are **not included** in this release.

You must supply your own licensed DOS software and create your own `DOS622.VHD` disk image.

This keeps the project focused on the FPGA hardware implementation and the tools needed to run your own legitimately obtained software.

## Download the Project

**GitHub Releases:**

https://github.com/KD5VMF/KD5VMF-KD5VMF-Nexys-A7-100T-ao486/releases

The project is being shared so that other FPGA enthusiasts, retrocomputing hobbyists, and developers can explore the design, build their own system, test software, and contribute improvements.

**MILO · Nexys A7 DOS PC**

*Bringing the classic DOS PC experience to FPGA hardware — one improvement at a time.*
