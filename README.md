# ChDroid rootfs

Clean aarch64 chroot releases for [ChDroid](https://github.com/Mafiadan6/ChDroid). Every ZIP is a plain rootfs tarball plus the chroot tools, with no module packaging and no boot dependency.

## What ships

Each release ZIP has the same clean layout:

```
ChDroid-ubuntu-rootfs-20261029.zip
ChDroid-arch-rootfs-minimal-20261029.zip
ChDroid-debian-13-rootfs-20261029.zip
ChDroid-fedora-45-rootfs-20261029.zip
ChDroid-alpine-rootfs-20261029.zip
ChDroid-artix-rootfs-20261029.zip
ChDroid-azurelinux-rootfs-20261029.zip
ChDroid-kali-rootfs-20261029.zip
ChDroid-openwrt-rootfs-20261029.zip
├── rootfs.tar.gz / rootfs.tar.xz  plain distro rootfs tarball
├── chroot.sh                     namespace, mount, DNS manager and CLI
├── post_exec.sh                  upstream startup hook, skipped by ChDroid Start
├── start-hotspot                 terminal utility
├── sparsemgr.sh                  terminal utility
├── forward-nat.sh                terminal utility
├── bin/busybox                   static busybox
├── manifest.json                 legacy manifest path
└── manifest-v2.json              app-facing manifest
```

Explicitly absent: `META-INF/`, `module.prop`, `install.sh`, `service.sh`, `uninstall.sh`, `update.json`, `webroot/`, `update-status.sh`, `ota/`, any `/data/adb/modules` reference, and any root-method detection.

## v4.4 catalog

All builds in the [v4.4 release](https://github.com/Mafiadan6/ChDroid-rootfs/releases/tag/v4.4) are aarch64.

| Rootfs | ZIP | Size | Package manager | Note |
|--------|-----|------|-----------------|------|
| Ubuntu 24.04 CLI rootfs | `ChDroid-ubuntu-rootfs-20261029.zip` | 562 MB | apt | Fresh Ubuntu 24.04 base |
| Arch Linux Minimal | `ChDroid-arch-rootfs-minimal-20261029.zip` | 112 MB | pacman | Kernel 5.10 or newer |
| Debian 13 Minimal | `ChDroid-debian-13-rootfs-20261029.zip` | 100 MB | apt | Stable Debian base |
| Fedora 45 Minimal | `ChDroid-fedora-45-rootfs-20261029.zip` | 56 MB | dnf | From the official Fedora minimal base |
| Alpine Minimal | `ChDroid-alpine-rootfs-20261029.zip` | 11 MB | apk | Smallest general-purpose rootfs |
| Artix Minimal | `ChDroid-artix-rootfs-20261029.zip` | 169 MB | pacman | OpenRC init |
| AzureLinux 3.0 Minimal | `ChDroid-azurelinux-rootfs-20261029.zip` | 88 MB | tdnf | AzureLinux base |
| Kali Linux Minimal | `ChDroid-kali-rootfs-20261029.zip` | 117 MB | apt | Minimal Kali base |
| OpenWrt 24.10 Minimal | `ChDroid-openwrt-rootfs-20261029.zip` | 4 MB | opkg | Smallest build in the set |

## Install

ChDroid downloads a ZIP from this repo's releases, or imports one from local storage. The installer extracts the tools and rootfs into a directory named from the setup field, for example:

```
/data/local/arch-linux-chroot/
├── chroot.sh
├── post_exec.sh
├── bin/busybox
└── rootfs/             extracted Arch Linux
```

`/data/local/chdroid-active-rootfs` records the active base. Older installs at `/data/local/ubuntu-chroot` keep working. The same distro under the same name keeps rootfs data. Installing a different distro or name stops the old rootfs and replaces it.

ChDroid applies its own identity after extraction. The prompt uses the setup name and there is no hostname setting. SELinux stays enforcing, and no root method is detected or required.

From any root shell, the installed manager stays usable:

```sh
BASE=$(cat /data/local/chdroid-active-rootfs)
sh "$BASE/chroot.sh" status
sh "$BASE/chroot.sh" start root
sh "$BASE/chroot.sh" stop
```

Works with any root solution that provides `su`. No module flash, no boot script and no reboot.

## Manifest

[`manifest-v2.json`](manifest-v2.json) is the app-facing manifest. ChDroid fetches it to populate the built-in download cards. `manifest.json` holds the legacy metadata read by the installer, including the distro id and version code. The schema for `manifest-v2.json` is a flat JSON array, one object per release.

## Provenance

- Ubuntu 24.04: repackaged from upstream 24.04 base asset in release `v20261029-152515`.
- Arch Linux minimal: upstream `Arch-Minimal-Kernel-5.10-and-up-20261029.tar.xz`, sha256 `e6f205afe7fc10b4682bdd8fc72bcf349308676bd9940c4dd7b9caecfe5b889f`. Requires kernel 5.10 or newer.
- Debian 13, Alpine, Artix, AzureLinux 3.0, Kali and OpenWrt 24.10: minimal aarch64 variants from the same upstream release.
- Fedora 45: rootfs layer extracted from the official `Fedora-Container-Base-Generic-Minimal-45-1.4.aarch64.oci.tar.xz`.

All release ZIPs bundle the clean chroot tools. No module packaging, boot scripts or root-method detection.
