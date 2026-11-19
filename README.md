# ChDroid rootfs

Clean Ubuntu 24.04 chroot releases for [ChDroid](https://github.com/Mafiadan6/ChDroid).

This is our own rootfs repo. The upstream `ravindu644/Ubuntu-Chroot` CLI ZIP is packaged as a
Magisk/KernelSU/APatch module: it carries `META-INF`, `module.prop`, `install.sh`, `service.sh`
and root-method detection. None of that is part of this build.

## What ships

Each release is one ZIP:

```
ChDroid-rootfs-v4.4-20260323.zip
├── rootfs.tar.gz       Ubuntu 24.04 rootfs (plain tarball)
├── chroot.sh           namespace/mount/DNS manager + CLI
├── post_exec.sh        startup hook called by chroot.sh
├── start-hotspot       terminal utility
├── sparsemgr.sh        terminal utility
├── forward-nat.sh      terminal utility
├── bin/busybox         static busybox
└── manifest.json       release metadata (not module.prop)
```

Explicitly **absent**: `META-INF/`, `module.prop`, `install.sh`, `service.sh`,
`uninstall.sh`, `update.json`, `webroot/`, `update-status.sh`, `ota/`, any
`/data/adb/modules` reference, any Magisk/KernelSU/APatch detection.

## Install

ChDroid downloads the ZIP from this repo's releases, or you can import it in the app.
The app extracts it straight to `/data/local/ubuntu-chroot`:

```
/data/local/ubuntu-chroot/
├── chroot.sh
├── post_exec.sh
├── bin/busybox
└── rootfs/             extracted Ubuntu 24.04
```

Then drive it from ChDroid's terminal:

```
sh /data/local/ubuntu-chroot/chroot.sh start root
```

Works with any root solution that provides a working `su` — Magisk, KernelSU, APatch,
DirtyFrag or plain `su`. No module flash, no boot script, no reboot.

## Manifest

[`rootfs.json`](rootfs.json) is the app-facing manifest. ChDroid fetches it to populate the
built-in download card. Schema matches the Droidspaces rootfs manifest (flat JSON array).

## Provenance

Repackaged from `ravindu644/Ubuntu-Chroot` v4.4 CLI (`Ubuntu-Chroot-v4.4-20260323-CLI.zip`).
Rootfs and chroot tools are upstream's; the module packaging, GUI webroot and OTA/module
status scripts are removed. The installer and distribution are ChDroid's.
