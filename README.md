# MILO · Nexys A7 DOS PC

### A small FPGA board. A real x86 machine. Your own DOS installation.

An **ao486-based PC built for the Digilent Nexys A7-100T**, featuring DDR2-backed memory, VGA video output, sound support, SD card storage, and a hardware POST display.

This project brings a classic DOS PC experience to modern FPGA hardware. The release package includes the accepted firmware, matching source code, and the tools needed to prepare your own DOS installation.

The goal is to create a functional, standalone FPGA-based x86 computer capable of running real DOS software, classic applications, and games.

## Current Performance and Compatibility

The system is operational and successfully running DOS applications and games. Although this is still a relatively new project, **many hours of development, testing, debugging, and optimization have already been invested** in getting it to its current state.

There is still plenty of work ahead, but the progress so far demonstrates what this FPGA platform is capable of.

### DOOM (1993)

**DOOM runs reasonably well, with decent overall performance.** The game is playable with audio disabled, and much of the gameplay experience works as expected.

However, performance can still slow down in certain areas, particularly during more graphically demanding scenes or at higher graphics settings. Additional optimization is needed to improve frame rates and maintain consistent gameplay.

Sound support is also still being worked on.

### DOOM II (1994)

**DOOM II is running, but performance still needs improvement.** The game currently works with audio disabled, although it runs noticeably slower than the original DOOM.

The system is not yet where it needs to be for consistently smooth DOOM II gameplay. Additional performance improvements and optimization are needed.

Getting both games running at all on this FPGA implementation is an encouraging milestone, but neither is being presented as fully optimized or completely finished.

### Sound Support

Sound works very well in other DOS programs, demonstrating that the audio implementation is functional.

However, both DOOM and DOOM II currently require audio to be disabled for successful gameplay. Resolving this compatibility issue remains an active development priority.

## Project Status

This is a working FPGA-based DOS PC, not simply a software simulation. It uses the ao486 x86-compatible processor core implemented in FPGA hardware.

The project has reached several important milestones:

- DOS boots and runs successfully.
- Classic DOS applications execute.
- VGA video output is operational.
- Sound works in supported DOS applications.
- SD card storage provides the DOS disk environment.
- Hardware POST information is displayed.
- DOOM and DOOM II both run, with known performance and audio limitations.

**This project is still in its early stages.** Despite its relatively recent development, a significant amount of time and effort has already gone into the hardware design, firmware integration, troubleshooting, testing, and overall system improvements.

It is a working foundation with considerable potential, but there are still compatibility issues, performance limitations, and improvements to address.

The aim is to continue refining the design rather than presenting it as a finished product.

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

This project is being shared with FPGA enthusiasts, retrocomputing hobbyists, and developers who want to experiment with a hardware-based x86 DOS computer.

Whether you're interested in building your own system, exploring the source, testing DOS compatibility, or helping improve performance, the project is available for others to explore and build upon.

**MILO · Nexys A7 DOS PC**

*An ambitious little FPGA DOS computer, built through many hours of hands-on development — still young, still improving, and already running real DOS software and classic games.*
