# Samsung Galaxy Note 10.1 (3G) — GT-N8000

Device configuration for **unofficial LineageOS 23.2 / Android 16**, maintained by [html6405](https://github.com/html6405). Device codenames include `p4noterf` / `p4noterfxx`.

## Downloads

- [2026-11-22 Enforcing experimental release](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-23.2-20261122)
- [ROM ZIP](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-23.2-20261122/lineage-23.2-20261122-HTML6405-n8000.zip)
- [SHA-256 checksum](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-23.2-20261122/lineage-23.2-20261122-HTML6405-n8000.zip.sha256)
- [Previous 2026-11-21 Permissive release](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-23.2-20261121)
- [Previous 2026-11-13 release](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-23.2-20261113)

2026-11-22 full build: **690,402,336 bytes**; incremental `1795369378`; SHA-256: `1f083b4ff63eaa0606b7243947323f1579a7cc5a0be0604edcc326fe29c28bd4`.

The 2026-11-21 package remains a separate Permissive baseline: 690,405,912 bytes, SHA-256 `443563c69c8e1ff5ead5d74f8af8210e3a8418833335f779e0b57074d44a13c5`.

This is a development preview. Current testing is on a **32 GB GT-N8000**; 16 GB units and other variants are not verified. **N8010/N8013 share the separate N8010 build; N8020 also requires its own build.**

## Required partition layout and installation

This build requires **2 GiB SYSTEM, 152 MiB CACHE and a 524,288,000-byte vendor partition**, with a compatible custom recovery. The ROM ZIP assumes this layout already exists; it does not change the partition table. Do not flash it onto an unprepared stock layout. For devices that need the layout changed, use the separate [N80xx System 2 GiB repartitioner](https://github.com/html6405/n80xx-repartition/releases) and read its device-specific support notes first. The repartitioner is a separate, potentially destructive operation and is not part of routine ROM updates. Its N8000 profile has been tested on 16 GB and 32 GB units; this ROM has only been tested on one 32 GB N8000.

Keep independent backups of personal data and EFS. Verify the ZIP with the supplied checksum before installing it in recovery. Updating a compatible existing LOS 23 test installation without wiping data was tested; stock-ROM migration and clean-install combinations have not been validated. The recovery partition was checked byte-for-byte before installation and again after installation in recovery; it remained unchanged.

## Bring-up status

The complete 2026-11-22 ZIP was flashed over the existing LOS 23 installation without wiping or repartitioning. Its packaged kernel was independently matched against installed readback; Android booted SELinux **Enforcing**. The original keyboard driver is retained: experimental dock wake/lifecycle changes were reverted after a long-sleep freeze. Automatic software-keyboard handling remains unresolved.

| Feature | Evidence and limits |
| --- | --- |
| New full-ZIP checks | Enforcing boot, Wi-Fi internet (2/2 ping replies), running audio/phone/SurfaceFlinger services, Maps map rendering and actual YouTube playback through Samsung `OMX.SEC.AVC.Decoder`. This bounded check does not establish long-term stability. |
| Audio / Bluetooth | Earlier Enforcing runtime tests passed system/media sound, Prime playback and bidirectional calls through the tested Dots, followed by media audio. These full routing tests were not repeated on the new complete ZIP. |
| AVC / HEVC | Earlier Prime playback/forward and backward seeks passed. Brief green bars in a small Prime preview and one self-recovering partial-image freeze after seeking remain unresolved. Platform HEVC decoding remains disabled. |
| RIL | Calls, SMS and 2G/EDGE data passed earlier runtime tests. SIM was absent both before and after today's full-ZIP update; no new-ZIP outbound call/SMS/mobile-data pass is claimed. 3G remains untested. |
| GPS / input | Earlier Enforcing runtime tests covered satellite acquisition/2D fix, S Pen and sensors; older bring-up acquired outdoor 3D fixes. These were not comprehensively repeated on the new ZIP. |
| Dock keyboard | Original Samsung keyboard driver retained. Ordinary keyboard operation was tested earlier; boot detection, automatic software-keyboard behavior and long dock sleep/wake remain open. |
| KernelSU | Optional manager recognized kernel 32676/UAPI 5 and Enforcing on the new ZIP. Earlier runtime tests exercised legitimate Ex4 root access and reboot persistence; recognition alone is not a new-ZIP root/security test. |
| Recovery / data | Compatible-layout update preserved user apps/data; no wipe or repartition was performed. Recovery is excluded from automatic restoration. |

Known stability issues remain: historical Mali/SystemUI graphics crashes, intermittent USB/ADB transport and `ueventd` crashes in `UeventListener::ReadUevent`. The latter crash class is also documented on N8010 across earlier November builds and is **not fixed** by this release. Do not infer a new-ROM long-sleep pass from earlier runtime observations. An earlier Enforcing overnight observation retained responsive wake and about 8 h 14 min suspend-accounted time; battery fell 100% to 93% over about nine hours. This is neither a controlled battery benchmark nor validation of this complete ZIP.

Google apps are not bundled. Individually installed standard microG Services **0.3.18.252432** and Companion survived **one actual Vanilla update** without wiping: signatures/spoofing self-check, retained UID and compared permissions, four registered GCM apps, YouTube hardware playback and Maps rendering passed. Incoming push delivery, Play Integrity, genuine Play Store, privileged add-on installation and two consecutive OTA cycles are not validated. Separate microG/Minimal-GApps installers remain TEST artifacts and are not included here.

This remains an experimental `userdebug` release. SELinux Enforcing does not imply complete policy or security hardening: existing domain exceptions, `SELINUX_IGNORE_NEVERALLOWS`, unauthenticated bring-up ADB policy and the legacy kernel/vendor stack remain. KernelSU full UID checks are disabled; root security hardening remains outstanding.

## WebView compatibility

The shared Exynos4 tree supplies `exynos4_webview_gles2`. Chromium's GPU-info collector requests ES3 and disables version fallback; Mali-400 only provides ES2. Enabling fallback prevents WebView from incorrectly forcing bitmap rendering and hiding normal video layers.

The build carries the equivalent source patch plus a strict transformation for the verified ARM WebView **156.0.8078.47**. It checks the complete library hashes and instruction context, preserves APK layout, and signs the result normally. An unknown bundled WebView revision intentionally fails the build until compatibility is reviewed.

Later app-store/manual provider updates are not patched by this build step. An unpatched replacement provider can bring the video issue back. Carrying the source fix in a maintained WebView source build is the preferred long-term path.

## Sources and building

This repository contains the device configuration, not the complete Android source tree. Matching proprietary blobs and the Exynos4 platform patches are required. The release's `source-revisions.json`, manifest and source supplement record build revisions, local patches and required configuration files. The snapshot includes local changes; it is provenance, not yet a complete public sync manifest. The planned LineageOS-UL publication is separate work.

Key shared trees:

- [n80xx-common](https://github.com/html6405/android_device_samsung_n80xx-common/tree/lineage-23.2)
- [smdk4412-common](https://github.com/html6405/android_device_samsung_smdk4412-common/tree/lineage-23.2)
- [Exynos4 kernel](https://github.com/html6405/android_kernel_samsung_smdk4412/tree/lineage-23.2)

With the matching LOS 23.2 tree, platform forks and vendor files in place:

```sh
source build/envsetup.sh
lunch lineage_n8000-bp4a-userdebug
make -j8 bacon
```

Please report the exact model/build, reproduction steps and sanitized logs. EFS backups, IMEI/IMSI and personal location data must stay private.

## Original device-tree notice

Copyright (C) 2012 The CyanogenMod Project

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

## Optional KernelSU Manager

The manager is optional and linked separately; it is not bundled or uploaded with this ROM. Use [KernelSU v3.3.1 Manager 32680 (original upstream APK)](https://github.com/backslashxx/KernelSU/releases/download/v3.3.1/KernelSU_v3.3.1_32680-release.apk), ARM32/armeabi-v7a, UAPI 5,12138194bytes. SHA-256: `d3a813af25045ad745bec812bf76005b301e26b85f6b627c9e05c61482c3bc39`. Do not substitute “latest”32736 / UAPI 6. Existing KernelSU security configuration remains unchanged.

Earlier N8000 runtime evidence includes legitimate Ex4 app root access and grant retention across reboots. On the new complete 2026-11-22 ZIP, the manager reported **Functioning**, kernel 32676-5, manager 32680-5, 32-bit and SELinux Enforcing. No new root grant was made in that bounded check. Full UID checks remain disabled; revocation/profile coverage and security hardening are not established.
