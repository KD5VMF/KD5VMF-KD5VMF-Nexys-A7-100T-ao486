# MILO · Nexys A7 DOS PC

### A small FPGA board. A real x86 machine. Your own DOS installation.

An **ao486-based PC built for the Digilent Nexys A7-100T**, featuring DDR2-backed memory, VGA video output, sound support, SD card storage, a hardware POST display, a blue HDD activity LED, and USB serial connectivity for communicating with an external computer and AI-assisted development tools such as OpenAI Codex.

This project brings the classic DOS PC experience to modern FPGA hardware. The release package brings together the accepted firmware, matching source code, and tools needed to prepare your own DOS installation.

The goal is to create a functional, standalone FPGA-based x86 computer capable of running real DOS software, classic applications, and games.

## Current Performance and Compatibility

The system is operational and successfully running DOS applications and games. Although this is still a relatively new project, **many hours of development, testing, debugging, and optimization have already been invested** in getting it to its current state.

There is still plenty of work ahead, but the progress so far demonstrates what this FPGA platform is capable of.

### DOOM (1993)

**DOOM runs reasonably well, with decent overall performance.** The game is playable with audio disabled, and much of the gameplay experience works as expected.

However, performance can still slow down in certain areas, particularly during more graphically demanding scenes. Additional optimization is needed to improve frame rates and maintain consistent gameplay.

Sound support is also still being worked on.

### DOOM II (1994)

**DOOM II is running, but performance still needs improvement.** The game currently works with audio disabled, although it runs noticeably slower than the original DOOM.

The system is not yet where it needs to be for consistently smooth DOOM II gameplay. Additional performance improvements and optimization are needed.

Getting both games running on this FPGA implementation is an encouraging milestone, but neither is being presented as fully optimized or completely finished.

### Sound Support

Sound works very well in other DOS programs, demonstrating that the audio implementation is functional.

However, both DOOM and DOOM II currently require audio to be disabled for successful gameplay. Resolving this compatibility issue remains an active development priority.

## Hardware Features

- **FPGA Platform:** Digilent Nexys A7-100T
- **Processor:** ao486 x86-compatible CPU core
- **Memory:** DDR2-backed system memory
- **Video:** VGA output for DOS applications and games
- **Audio:** Sound support for compatible DOS software
- **Storage:** SD card-based DOS disk environment
- **POST Display:** Hardware display for system startup and diagnostic information
- **Blue HDD Activity LED:** A blue LED that indicates disk activity, providing visual feedback whenever the system accesses storage
- **USB Serial Interface:** Connectivity to a host computer for serial communication, troubleshooting, and AI-assisted development using Codex

The blue HDD activity LED adds a small but useful touch of authenticity, much like the hard drive activity lights found on classic desktop PCs.

## USB Communication and OpenAI Codex Integration

One particularly useful part of this project is the ability to connect the Nexys A7-100T to another computer through its USB serial interface.

This makes it possible to use **OpenAI Codex as an external development and troubleshooting assistant**, allowing developers to work with the FPGA system through a serial connection rather than relying entirely on manual interaction.

Codex runs on the connected host computer, not on the FPGA itself.

### How It Works

The Nexys A7-100T has a built-in USB-to-UART interface that allows serial communication with a Windows or Linux computer.

The general communication path is:

**OpenAI Codex → Host Computer → USB Serial Connection → Nexys A7 FPGA → DOS System**

When the installed firmware exposes a DOS-compatible serial interface or command console, the host can exchange information with the running system.

This can help with development, diagnostic output, testing, and troubleshooting.

The exact commands available depend on the firmware and DOS-side software installed.

### How to Connect Your Own Board to Codex

**Step 1 — Connect the FPGA**

Connect the Nexys A7-100T to your Windows or Linux computer using its USB-UART/JTAG programming connection.

Make sure the appropriate Digilent USB drivers are installed and the board is powered and running the project firmware.

**Step 2 — Identify the Serial Port**

On Windows, open Device Manager and look under **Ports (COM & LPT)** for the USB serial connection.

It will normally appear as a numbered COM port, such as `COM3` or `COM5`.

On Linux, identify the USB serial interface using:

```bash
ls /dev/ttyUSB* /dev/ttyACM* 2>/dev/null
```

A port such as `/dev/ttyUSB0` may appear.

**Step 3 — Test the Serial Connection**

Use a serial terminal program such as PuTTY, Tera Term, Minicom, or another compatible terminal.

Select the board's serial port and configure the baud rate and serial settings to match the firmware.

Once connected, verify that the installed firmware or DOS software supports serial communication. A USB connection alone does not automatically provide a DOS command prompt.

**Step 4 — Install and Start OpenAI Codex**

Install OpenAI Codex on the host computer using the official instructions:

https://developers.openai.com/codex/

Start Codex in your development environment, where it can access the project files and, when properly configured, the serial port.

**Step 5 — Give Codex Access to the Serial Interface**

Codex can use host-side serial utilities, scripts, or other supported tools to communicate with the FPGA.

For example, a Python program using `pyserial` can open the detected serial port, send supported commands, and capture responses.

Serial access must be configured for the actual firmware interface. The baud rate, command protocol, and DOS-side serial support should be verified before enabling automated interaction.

Do not allow multiple terminal programs to open the same serial port simultaneously.

### Example Instructions for Codex

The following is an example prompt to give Codex after connecting the board:

> I have a Digilent Nexys A7-100T running my MILO ao486 DOS PC FPGA project.
>
> The board is connected to this host computer through its USB serial interface.
>
> Find the correct serial port and identify the communication settings from the project source or documentation.
>
> Verify communication without changing firmware, disk contents, or hardware configuration.
>
> Determine which serial commands and diagnostic functions are available, and explain how we can use them for DOS development, testing, and troubleshooting.
>
> Do not assume the serial port provides a DOS shell. Confirm the supported interface before sending commands.

### Why This Is Useful

Connecting the FPGA to a computer running Codex provides another way to develop and troubleshoot the system.

Codex can help with source-code inspection, firmware changes, diagnostic scripts, serial communication, and analysis of test results.

It can also assist with building software, reviewing logs, and investigating compatibility issues.

**Important:** The USB connection is a communication interface, not an AI processor. The Nexys A7 continues running its own FPGA-based x86 hardware independently, while Codex operates on the external host computer.

This creates opportunities for AI-assisted retrocomputing development without requiring modern AI software to run inside DOS.

## Project Status

This is a working FPGA-based DOS PC, not simply a software simulation. It uses the ao486 x86-compatible processor core implemented in FPGA hardware.

The project has reached several important milestones:

- DOS boots and runs successfully.
- Classic DOS applications execute.
- VGA video output is operational.
- Sound works in supported DOS applications.
- SD card storage provides the DOS disk environment.
- The hardware POST display provides startup information.
- The blue HDD activity LED indicates storage access.
- The board provides a USB serial connection for host-side development and diagnostics.
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

https://github.com/KD5VMF/Nexys-A7-100T-ao486/releases

This project is being shared with FPGA enthusiasts, retrocomputing hobbyists, and developers who want to experiment with a hardware-based x86 DOS computer.

Whether you're interested in building your own system, exploring the source, testing DOS compatibility, experimenting with AI-assisted development, or helping improve performance, the project is available for others to explore and build upon.

**MILO · Nexys A7 DOS PC**

*An ambitious little FPGA DOS computer, built through many hours of hands-on development — still young, still improving, and already running real DOS software and classic games.*
