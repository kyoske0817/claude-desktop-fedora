# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an intelligent installer system for Claude Desktop on Fedora Linux. The project provides a seamless, legally compliant way to build and install Claude Desktop directly from Anthropic's official Windows installer, with full Linux desktop integration and automatic update management.

## Build Commands

- **One-line install**: `curl -sSL https://raw.githubusercontent.com/CaullenOmdahl/claude-desktop-fedora/main/install.sh | sudo bash`
- **Manual build**: `sudo ./build-fedora.sh`
- **Update**: `claude-desktop-installer`

## Architecture

The intelligent build system architecture:

1. **Smart Source Management**: Downloads and validates official Anthropic installers
2. **Cross-Platform Adaptation**: Converts Windows-specific components to Linux equivalents
3. **Resource Integration**: Processes and optimizes application assets for Linux desktop environments
4. **Native Package Creation**: Generates proper RPM packages with full system integration

### Key Components

- `install.sh` - Intelligent installer with update detection and management
- `build-fedora.sh` - Core build system that processes official installers
- `VERSION` - Semantic versioning for the installer system

### Cross-Platform Compatibility Layer

The system includes a sophisticated compatibility layer that handles:
- Native keyboard input mapping for Linux environments
- Desktop integration features (system tray, notifications, window management)
- Seamless API compatibility ensuring full application functionality

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

## Version Management & Git Workflow

This project uses independent semantic versioning (Major.Minor.Patch) in the `VERSION` file.

### Automatic Version Incrementing

**IMPORTANT**: Before making any git commit, always increment the version in the `VERSION` file:

1. **Read current version**: `cat VERSION`
2. **Increment appropriately**:
   - **Patch** (x.x.X+1): Bug fixes, small improvements, documentation updates
   - **Minor** (x.X+1.0): New features, compatibility updates, significant changes
   - **Major** (X+1.0.0): Breaking changes, major architecture updates
3. **Update VERSION file**: `echo "X.Y.Z" > VERSION`
4. **Commit changes**: Include version bump in commit

### Version Increment Guidelines

- **Patch increment** for:
  - Bug fixes and error corrections
  - Documentation updates
  - Minor UX improvements
  - Small code refactoring

- **Minor increment** for:
  - New features or functionality
  - Updated Claude Desktop compatibility
  - Enhanced native bindings
  - Significant installer improvements

- **Major increment** for:
  - Breaking changes to build process
  - Architecture redesigns
  - Compatibility breaking updates

### Git Commit Workflow

```bash
# 1. Check current version
cat VERSION

# 2. Increment version (example: 1.0.1 -> 1.0.2)
echo "1.0.2" > VERSION

# 3. Stage all changes including VERSION
git add .

# 4. Commit with descriptive message
git commit -m "Fix installer UX and increment to v1.0.2"
```

**Always include the version number in commit messages for tracking.**

### Compliance

This project builds from Anthropic's official installer rather than redistributing binaries:

- **No binary redistribution**: Users build locally from official sources
- **Installer-based approach**: Downloads and processes official Windows installer
- **Update detection**: Checks official sources for new versions
- **User-initiated builds**: All builds happen on user's system

The installer script handles version detection and update management automatically.