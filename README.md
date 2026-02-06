# TwentiOS

TwentiOS is an internal, company-focused fork of Bazzite, built on Fedora Atomic and Universal Blue. It delivers a consistent, secure, and easily managed Linux workstation for employees, with a container-first development workflow and reliable system updates.

## Table of Contents
- [About \& Features](#about--features)
  - [Desktop](#desktop)
  - [KDE Plasma](#kde-plasma)
  - [GNOME](#gnome)
  - [Features from Upstream](#features-from-upstream)
    - [Universal Blue](#universal-blue)
    - [Features from Fedora Linux (Kinoite \& Silverblue)](#features-from-fedora-linux-kinoite--silverblue)
- [Why](#why)
- [Documentation](#documentation)
- [Verification](#verification)
- [Secure Boot](#secure-boot)
- [Build Your Own](#build-your-own)

---

## About & Features

TwentiOS is a custom Fedora Atomic image built with cloud-native technology from Universal Blue. It is designed for employee productivity, stability, and maintainability across laptops and desktops.

TwentiOS is built from `ublue-os/main` and `ublue-os/nvidia` using Fedora technology, which means expanded hardware support and built-in drivers are included. Additionally, TwentiOS provides:

- Automatic updates for the OS and Flatpaks, powered by `ublue-update` and `topgrade`.
- Multi-media codecs out of the box.
- Full hardware-accelerated codec support for H264 decoding.
- Full support for AMD's ROCm OpenCL/HIP run-times.
- Full support for DisplayLink.
- Distrobox preinstalled for container-based development workflows.
- Simplified Davinci Resolve installation with `davincibox` (`ujust install-resolve`).
- Ptyxis Terminal used as the default in all images, optimized for container workflows.
- Automated `duperemove` service for reducing the disk space used by duplicated data.
- Support for HDMI CEC via `libCEC`.
- Uses Google's BBR TCP congestion control by default.
- Input Remapper preinstalled and enabled.
- Waydroid preinstalled for running Android apps.
- Manage applications using Flatseal, Warehouse, and Gear Lever.
- OpenRGB i2c-piix4 and i2c-nct6775 drivers for controlling RGB on certain motherboards.
- OpenRazer drivers built in, installable via `ujust install-openrazer`.
- OpenTabletDriver udev rules built in, with the full software suite installable via `ujust install-opentabletdriver`.
- Out-of-the-box support for Wooting keyboards.
- Built-in support for Southern Islands (HD 7000) and Sea Islands (HD 8000) AMD GPUs under the `amdgpu` driver.
- XwaylandVideoBridge is available for Discord screensharing on Wayland.
- Webapp Manager is available for creating applications from websites for a variety of browsers, including Firefox.

### Desktop

Common variant available as `twentios`, suitable for desktop computers.

Rebase from an existing upstream Fedora Atomic to this image:

```bash
rpm-ostree rebase ostree-unverified-registry:ghcr.io/kevcops/twentios:stable
```

or for devices with Nvidia GPUs:

```bash
rpm-ostree rebase ostree-unverified-registry:ghcr.io/kevcops/twentios-nvidia:stable
```

**For users with Secure Boot enabled:** Follow our [secure boot documentation](#secure-boot) prior to rebasing.

### KDE Plasma

KDE Plasma is the default desktop variant. It is provided as its own image, so the choice between KDE and GNOME is made by rebasing to the corresponding image tag.

Rebase from an existing upstream Fedora Atomic to the KDE Plasma image:

```bash
rpm-ostree rebase ostree-unverified-registry:ghcr.io/kevcops/twentios:stable
```

or for devices with Nvidia GPUs:

```bash
rpm-ostree rebase ostree-unverified-registry:ghcr.io/kevcops/twentios-nvidia:stable
```

**For users with Secure Boot enabled:** Follow our [secure boot documentation](#secure-boot) prior to rebasing.

### GNOME

Builds with the GNOME desktop environment are available in desktop flavors. These builds come with the following additional features:

- Variable refresh rate support and fractional scaling enabled under Wayland.
- GSConnect preinstalled and ready to use.
- Hanabi extension included for animated wallpapers.
- Numerous optional extensions pre-installed, including important user experience fixes.
- Automatic updates for the Firefox GNOME theme and Thunderbird GNOME theme (if installed).

Rebase from an existing upstream Fedora Atomic to this image:

```bash
rpm-ostree rebase ostree-unverified-registry:ghcr.io/kevcops/twentios-gnome:stable
```

To rebase an existing ostree system to the GNOME release with proprietary NVIDIA drivers:

```bash
rpm-ostree rebase ostree-unverified-registry:ghcr.io/kevcops/twentios-gnome-nvidia:stable
```

**For users with Secure Boot enabled:** Follow our [secure boot documentation](#secure-boot) prior to rebasing.

### Features from Upstream

#### Universal Blue

- Proprietary Nvidia drivers pre-installed (only for Nvidia images).
- Flathub is enabled by default.
- `ujust` commands for convenience.
- Roll back to any build within the last 90 days.

#### Features from Fedora Linux (Kinoite & Silverblue)

- A stable base.
- System packages stay relatively up to date.
- Can layer Fedora packages to the image without losing them between updates.
- Security focused with SELinux preinstalled and configured out of the box.
- The ability to rebase to different Fedora Atomic images without losing user data.
- Printing support thanks to CUPS being preinstalled.

## Why

TwentiOS exists to provide a reliable, consistent workstation experience for our team. It keeps the base system immutable and easy to manage, while still allowing developers to layer packages, use Flatpaks, and work inside containers without breaking the host OS.

Updates are delivered frequently from upstream Fedora and Universal Blue, and rollbacks are straightforward with `rpm-ostree`.

## Documentation

Internal documentation should live alongside this repository. If you maintain external docs, link them here.

Repository:

```text
https://github.com/kevcops/twentiOS
```

### App Store Toggle

You can switch the default Flatpak app store between Bazaar and KDE Discover:

```bash
ujust switchappstore
```

## Verification

Images are signed with sigstore's `cosign`. You can verify a signature by downloading the public key from this repo and running:

```bash
cosign verify --key cosign.pub ghcr.io/kevcops/twentios
```

## Secure Boot

Secure boot is supported with our custom key. The pub key can be found in the root of this repository in `secure_boot.der`.
If you'd like to enroll this key prior to installation or rebase, run the following:

```bash
sudo mokutil --timeout -1
sudo mokutil --import secure_boot.der
```

For users already on a Universal Blue image, you may instead run `ujust enroll-secure-boot-key`.

If asked for a password, use `universalblue`.

## Build Your Own

TwentiOS is built in GitHub. Creating your own custom version is as simple as forking this repository, adding a private signing key, and enabling GitHub Actions.

You'll need to generate a new keypair with cosign. The public key can be in your public repo (your users need it to check the signatures), and you can paste the private key in `Settings -> Secrets -> Actions` with the name `SIGNING_SECRET`.

We also ship a config for the `pull` app if you'd like to keep your fork in sync with upstream. Enable this app on your repo to keep track of upstream changes while also making your own modifications.
