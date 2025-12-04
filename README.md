# Windows VM Setup on Apple Silicon Mac with UTM

A complete guide for setting up Windows 11 ARM virtual machines on Apple Silicon Macs using UTM. This setup enables cross-platform testing and Windows app access while leveraging native ARM performance.

## Table of Contents
- [Overview](#overview)
- [What's in This Repository](#whats-in-this-repository)
- [Quick Start](#quick-start)
- [Running the VM](#running-the-vm)
- [Prerequisites](#prerequisites)
- [Detailed Setup Instructions](#detailed-setup-instructions)
- [Steps Completed to Date](#steps-completed-to-date)
- [Notes and Troubleshooting](#notes-and-troubleshooting)
- [Resources](#resources)
- [License](#license)

## Overview

This repository documents setting up a Windows 11 ARM virtual machine on a Mac with Apple Silicon (e.g., M4 Max, 16GB RAM, macOS 15.6.1) using free tools like UTM. The goal is cross-platform access for testing or apps, leveraging native ARM performance.

**Date Started:** November 29, 2025
**ISO:** 26100.4349.250607-1500.ge_release_svc_refresh_CLIENTCONSUMER_RET_A64FRE_en-gb.iso
(Windows 11 24H2 Insider Preview, ARM64, ~5.6GB)
**Tools:** UTM (v4.7.4), CrystalFetch (v2.2.0) via Homebrew

## What's in This Repository

- `.gitignore` - Configured to exclude large ISO files from version control
- `README.md` - This comprehensive setup guide
- `*.iso` - Windows 11 ARM ISO images (excluded from git, download separately)

## Quick Start

**Already have the ISO?** Jump straight to creating the VM:
1. Install UTM: `brew install --cask utm`
2. Open UTM → "+" → "Virtualize" → "Windows"
3. Allocate 8GB RAM, 4-8 cores, 64GB+ storage
4. Attach your ISO, enable "Install Windows 10 or higher"
5. Boot and follow Windows installation prompts

**Starting from scratch?** See [Detailed Setup Instructions](#detailed-setup-instructions) below.

## Running the VM

Once you've completed the initial setup, running your Windows VM is simple:

1. **Open UTM**: Launch the UTM application from your Applications folder or via Spotlight
2. **Select Your VM**: Click on your Windows 11 VM from the list of virtual machines
3. **Start the VM**: Click the play button (▶️) in the toolbar, or right-click and select "Run"
4. **Wait for Boot**: The VM will boot up - this may take 30-60 seconds on first boot after setup
5. **Interact**: Click inside the VM window to use Windows. Press `Control+Option` to release the mouse/keyboard

### Useful Commands

- **Full Screen**: Click the full-screen button or `Control+Command+F`
- **Shutdown**: Use Windows Start menu → Power → Shut down (recommended)
- **Force Stop**: UTM toolbar → Stop button (use only if VM is unresponsive)
- **Pause/Resume**: UTM toolbar → Pause button to suspend the VM

### Tips

- The VM state is saved between runs - you can shut down Windows normally and restart later
- For best performance, close other resource-intensive apps while running the VM
- If the VM doesn't start, check UTM's console output for error messages

## Prerequisites
- macOS Ventura (13) or later (tested on Sequoia 15.6.1).
- At least 16GB RAM (allocate 8GB to VM) and 64GB+ free storage.
- Homebrew installed (brew.sh).
- Valid Windows 11 license (Pro/Enterprise; ~$200 from Microsoft) for production—use free Insider Previews for testing.
- Git for version control (optional, with .gitignore to exclude ISO).

## Steps Completed to Date
1. **Install UTM via Homebrew**: Ran `brew install --cask utm` to set up the open-source VM tool for Apple Silicon.
2. **Install CrystalFetch via Homebrew**: Ran `brew install --cask crystalfetch` for easy ISO downloading.
3. **Download Windows 11 ARM64 ISO**: Used CrystalFetch to fetch Build 26100.4349 (24H2 Insider Preview); saved to ~/Downloads.
4. **Troubleshoot ISO Mounting**: Verified with `file` command (confirmed ISO 9660 bootable). Removed quarantine via `xattr -d com.apple.quarantine`. Mounted manually with `hdiutil attach` after resolving "Resource temporarily unavailable" (checked locks with `lsof`, unmounted via `hdiutil detach`).
5. **Organize Files**: Unmounted ISO, created ~/code/windows_vm, moved ISO there with `mv`, and added this README.md.

## Detailed Setup Instructions
Follow these steps to replicate the setup from scratch:

1. **Install Homebrew** (if not already): `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`.
2. **Install UTM**: `brew install --cask utm`.
3. **Install CrystalFetch**: `brew install --cask crystalfetch`.
4. **Download ISO**: Launch CrystalFetch, select Windows 11 ARM64, choose Build 26100.4349 (or latest 24H2), download to ~/Downloads.
5. **Verify ISO**: Run `file ~/Downloads/*.iso` (should show ISO 9660 bootable). Mount with `hdiutil attach` if needed.
6. **Organize**: Unmount if attached (`hdiutil detach /Volumes/26100*`), create folder (`mkdir ~/code/windows_vm`), move ISO (`mv ~/Downloads/*.iso ~/code/windows_vm/`), add README.md.
7. **Create VM in UTM**: Open UTM, click "+", select "Virtualize" > "Windows". Allocate 8GB RAM, 4-8 cores, 64GB+ storage. Attach ISO from ~/code/windows_vm, enable "Install Windows 10 or higher" and SPICE tools. Save and run.
8. **Install Windows**: Boot (press any key), follow prompts. If "Can't run Windows 11," bypass via Shift+F10 > regedit > add BypassTPMCheck=1 and BypassSecureBootCheck=1 under HKEY_LOCAL_MACHINE\SYSTEM\Setup\LabConfig.
9. **Post-Install**: Install SPICE tools from CD menu for drivers. Activate with license. Test shared folders/networking.
10. **Git Setup**: `git init` in ~/code/windows_vm, add .gitignore with "*.iso" to exclude large files, commit README.md and notes.

## Notes and Troubleshooting

### Performance Considerations
- **ARM limitations:** Some x86 apps need emulation (slower); test compatibility before relying on specific software.
- **Updates:** Check UTM GitHub for new versions; Microsoft releases (e.g., KB5060842 for this build) improve stability.

### Alternatives to UTM
- **VMware Fusion** (free) - More features and enterprise support
- **Parallels Desktop** (paid) - Seamless macOS integration and better performance

### Common Issues
- **Black screens?** Eject and re-mount the tools ISO from the CD menu
- **No network during setup?** Use `OOBE\BYPASSNRO` command during Windows installation
- **Resource temporarily unavailable?** Check for locked processes with `lsof` and unmount with `hdiutil detach`

## Resources
- [UTM Documentation](https://docs.getutm.app/guides/windows/)
- [Microsoft Windows Insider Preview (ARM64)](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewARM64)

## License

This documentation is provided as-is for educational and personal use. Windows 11 requires a valid license from Microsoft for production use. The setup process described here uses freely available Insider Preview builds for testing purposes.
