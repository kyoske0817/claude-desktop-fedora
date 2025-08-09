# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Linux build system for Claude Desktop, specifically targeting Fedora-based distributions. The project creates an unofficial RPM package that allows Claude Desktop to run natively on Linux by extracting the Windows installer and replacing platform-specific components.

## Build Commands

- **One-line install**: `curl -sSL https://raw.githubusercontent.com/CaullenOmdahl/claude-desktop-fedora/main/install.sh | sudo bash`
- **Manual build**: `sudo ./build-fedora.sh`
- **Update**: `claude-desktop-installer`

## Architecture

The build system works by:

1. **Installer Extraction**: Downloads the Windows Claude Desktop installer (`Claude-Setup-x64.exe`) and extracts it using 7zip
2. **Resource Processing**: Extracts icons, application files, and the main `app.asar` archive
3. **Native Module Replacement**: Replaces the Windows-specific `claude-native` module with a Linux-compatible stub that provides the same API
4. **RPM Packaging**: Creates a proper RPM package with desktop integration, icons, and launcher scripts

### Key Files

- `build-fedora.sh` - Main build script that orchestrates the entire process
- `claude-desktop-*.rpm` - Pre-built RPM package available in releases

### Native Module Stub

The critical component is the replacement of `claude-native` module (`build-fedora.sh:245-344`) which provides:
- Keyboard key mappings using correct Linux keycodes
- System integration stubs (window effects, notifications, progress bar)
- Maintains API compatibility with the original Windows module

### Installation Paths

- Application: `/usr/lib64/claude-desktop/`
- Launcher: `/usr/bin/claude-desktop`
- Desktop entry: `/usr/share/applications/claude-desktop.desktop`
- Icons: `/usr/share/icons/hicolor/*/apps/claude-desktop.png`
- MCP config: `~/.config/Claude/claude_desktop_config.json`

## System Requirements

- Fedora 42+ (tested)
- x86_64 architecture
- Dependencies auto-installed: sqlite3, p7zip-plugins, icoutils, ImageMagick, nodejs, npm, rpm-build

## Version Management

This project uses independent semantic versioning (Major.Minor.Patch) in the `VERSION` file:

- **Major**: Breaking changes or significant architecture updates  
- **Minor**: New features, updated Claude Desktop versions
- **Patch**: Bug fixes, documentation updates

### Legal Compliance

This project builds from Anthropic's official installer rather than redistributing binaries:

- **No binary redistribution**: Users build locally from official sources
- **Installer-based approach**: Downloads and processes official Windows installer
- **Update detection**: Checks official sources for new versions
- **User-initiated builds**: All builds happen on user's system

The installer script handles version detection and update management automatically.