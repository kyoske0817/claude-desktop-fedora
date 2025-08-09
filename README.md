


***UNOFFICIAL COMMUNITY PROJECT***

This is an independent community project to bring Claude Desktop to Linux. For issues with this installer, please report them here - this project is not affiliated with or supported by Anthropic.

# Claude Desktop for Linux

**Status: ✅ Fully Working** | **Supports: Fedora 42+** | **Source: Build from Official Installer**

An intelligent installer that builds Claude Desktop for Fedora Linux directly from Anthropic's official Windows installer, providing seamless Linux integration with automatic updates.

## 🚀 One-Line Installation

```bash
curl -sSL https://raw.githubusercontent.com/CaullenOmdahl/claude-desktop-fedora/main/install.sh | sudo bash
```

That's it! The installer will:
- ✅ Download Anthropic's official Claude Desktop installer
- ✅ Build a native Linux package with bundled Electron
- ✅ Install with full desktop integration
- ✅ Set up automatic update checking
- ✅ Create convenient update commands

**Note:** This script builds from Anthropic's official installer - no pre-built binaries are redistributed.

## System Requirements

### Requirements (Based on Testing)
- **OS**: Fedora 42 (other versions untested)
- **Architecture**: x86_64 (64-bit) - ARM untested
- **RAM**: 4GB minimum (8GB recommended for smooth operation)
- **Storage**: 200MB for installation, 2GB for building from source
- **Network**: Internet connection for initial download

### Software Dependencies (Auto-installed)
- **Runtime**: Electron 37.0.0+ (standalone installation)
- **System Libraries**: GTK3, GLib, standard X11/Wayland libraries
- **Build Dependencies** (if building from source): rpm-build, p7zip-plugins, icoutils, sqlite, ImageMagick

## ✨ Features

- **🔗 MCP Support**: Full Model Context Protocol integration
- **⌨️ Global Shortcuts**: Ctrl+Alt+Space popup functionality  
- **📍 System Tray**: Native system tray integration
- **🖥️ Desktop Integration**: Proper Linux desktop experience
- **🔄 Auto Updates**: Built-in update detection and management
- **📦 All-in-One**: Bundled with Electron - no separate installation needed

**MCP Configuration**: `~/.config/Claude/claude_desktop_config.json`

# Usage

## After Installation

```bash
# Launch Claude Desktop
claude-desktop

# Update to latest version
claude-desktop-installer

# Check version
claude-desktop-installer --version
```

## Manual Build (Advanced)

If you prefer to build manually:

```bash
# Clone this repository
git clone https://github.com/CaullenOmdahl/claude-desktop-fedora.git
cd claude-desktop-fedora

# Build and install
sudo ./build-fedora.sh
sudo dnf install $(uname -m)/claude-desktop-*.rpm
```


# Tested Environment

## ✅ Confirmed Working
- **OS**: Fedora Linux 42 (Workstation Edition)
- **Architecture**: x86_64 (64-bit)
- **Desktop Environment**: GNOME (Wayland session with X11 backend)
- **Build Script**: Automatically downloads and builds latest Claude Desktop
- **Features Confirmed**: 
  - Application launches successfully
  - System tray integration working
  - MCP support functional
  - Ctrl+Alt+Space global shortcut working
  - All desktop integration features working

## ❓ Untested (May Work)
- Other desktop environments (KDE, XFCE, etc.)
- Older Fedora versions
- Other architectures

# How it works

Claude Desktop is an Electron application packaged as a Windows executable. Our build script performs several key operations to make it work on Linux:

1. Downloads and extracts the Windows installer
2. Unpacks the app.asar archive containing the application code
3. Replaces the Windows-specific native module with a Linux-compatible implementation
4. Repackages everything into a proper RPM package

The process works because Claude Desktop is largely cross-platform, with only one platform-specific component that needs replacement.

## The Native Module Challenge

The only platform-specific component is a native Node.js module called `claude-native-bindings`. This module provides system-level functionality like:

- Keyboard input handling
- Window management
- System tray integration
- Monitor information

Our build script replaces this Windows-specific module with a Linux-compatible implementation that:

1. Provides the same API surface to maintain compatibility
2. Implements keyboard handling using the correct key codes from the reference implementation
3. Stubs out unnecessary Windows-specific functionality
4. Maintains critical features like the Ctrl+Alt+Space popup and system tray

The compatibility layer maintains full API compatibility while providing native Linux functionality, ensuring seamless operation without application modifications.

## Technical Implementation

The intelligent build system works through these steps:

1. **Environment Validation**: Verifies Fedora compatibility and installs required tools
2. **Source Acquisition**: Downloads and validates Anthropic's official installer  
3. **Resource Extraction**: Intelligently unpacks application assets and dependencies
4. **Linux Adaptation**: Replaces Windows-specific components with cross-platform equivalents
5. **Integration Setup**: Configures desktop integration, icons, and system services
6. **Package Creation**: Builds a native RPM with proper metadata and dependencies
7. **Post-Install Configuration**: Sets up launchers, update checking, and user environment

## How It Works

The installer works by:

1. **Download**: Gets Anthropic's official Windows Claude Desktop installer
2. **Extract**: Unpacks the installer to access the application files  
3. **Adapt**: Replaces Windows-specific components with Linux equivalents
4. **Package**: Builds a native RPM with bundled Electron runtime
5. **Install**: Uses standard package management for clean installation
6. **Integrate**: Sets up desktop integration and update checking

**Compliance**: No pre-built binaries are distributed. Users build directly from Anthropic's official installer.

## Environment Variables

The launcher script sets these environment variables to ensure proper operation:

- `GDK_BACKEND=x11` - Forces X11 backend (prevents Wayland conflicts)
- `GTK_USE_PORTAL=0` - Disables GTK portal (prevents dialog issues)
- `ELECTRON_DISABLE_SECURITY_WARNINGS=true` - Reduces console noise

## File Locations

- **Application files**: `/usr/lib64/claude-desktop/`
- **Bundled Electron**: `/usr/lib64/claude-desktop/electron/`
- **Launcher script**: `/usr/bin/claude-desktop`
- **Desktop entry**: `/usr/share/applications/claude-desktop.desktop`
- **Icons**: `/usr/share/icons/hicolor/*/apps/claude-desktop.*`
- **MCP config**: `~/.config/Claude/claude_desktop_config.json`
- **Logs**: `~/claude-desktop-launcher.log`

## Troubleshooting

### Common Issues

1. **Installation fails**: Ensure you have sudo access and stable internet connection
2. **Application won't start**: Check the log file: `~/claude-desktop-launcher.log`
3. **GTK warnings**: These are cosmetic and don't affect functionality  
4. **System tray not working**: Ensure your desktop environment supports system tray
5. **Updates not detected**: Run `claude-desktop-installer` to force check

### Getting Help

- Check the log file: `~/claude-desktop-launcher.log`
- Run with debug: `ELECTRON_ENABLE_LOGGING=true claude-desktop`
- Update: `claude-desktop-installer`
- Report issues to this repository (not Anthropic)

### Update Issues

If automatic updates fail:
```bash
# Force reinstall
sudo claude-desktop-installer

# Or manual build
git clone https://github.com/CaullenOmdahl/claude-desktop-fedora.git
cd claude-desktop-fedora && sudo ./build-fedora.sh
```

# License

The build scripts in this repository, are dual-licensed under the terms of the MIT license and the Apache License (Version 2.0).

See [LICENSE-MIT](LICENSE-MIT) and [LICENSE-APACHE](LICENSE-APACHE) for details.

The Claude Desktop application, not included in this repository, is covered by [Anthropic's Consumer Terms](https://www.anthropic.com/consumer-terms).

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any
additional terms or conditions.

---

## About This Project

This is an independent community effort to bring Claude Desktop to Linux through an intelligent build system. The project focuses on providing a seamless, legally compliant installation experience while maintaining full compatibility with Claude Desktop's features.
