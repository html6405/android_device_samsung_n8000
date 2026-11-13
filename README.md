# Samsung Galaxy Note 10.1 (3G) — GT-N8000

Device configuration for **unofficial LineageOS 23.2 / Android 16**, maintained by [html6405](https://github.com/html6405). Device codenames include `p4noterf` / `p4noterfxx`.

## Downloads

- [2026-11-13 experimental release and changelog](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-23.2-20261113)
- [ROM ZIP](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-23.2-20261113/lineage-23.2-20261113-HTML6405-n8000.zip)
- [SHA-256 checksum](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-23.2-20261113/lineage-23.2-20261113-HTML6405-n8000.zip.sha256sum)
- [Previous 2026-11-08 release](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-23.2-20261108)

This is a development preview. Current testing is on a **32 GB GT-N8000**; 16 GB units and other variants are not verified. **N8010, N8013 and N8020 require their own builds.**

## Required partition layout and installation

This build requires **2 GiB SYSTEM and 152 MiB CACHE**, with a compatible custom recovery. The ROM ZIP assumes this layout already exists; it does not change the partition table. Do not flash it onto an unprepared stock layout. For devices that need the layout changed, use the separate [N80xx System 2 GiB repartitioner](https://github.com/html6405/n80xx-repartition/releases) and read its device-specific support notes first. The repartitioner is a separate, potentially destructive operation and is not part of routine ROM updates. Its N8000 profile has been tested on 16 GB and 32 GB units; this ROM has only been tested on one 32 GB N8000.

Keep independent backups of personal data and EFS. Verify the ZIP with the supplied checksum before installing it in recovery. Updating a compatible existing LOS 23 test installation without wiping data was tested; stock-ROM migration and clean-install combinations have not been validated. The recovery partition was checked byte-for-byte before installation and after Android boot and remained unchanged.

## Bring-up status

| Feature | Current evidence |
| --- | --- |
| Boot / graphics | Boots on the test N8000; legacy GLES RenderEngine and Mali EGL working. |
| Wi-Fi | Internet and HTTPS video playback tested. |
| Audio | Media/ringtone playback and call audio tested; basic voice recording reported working. Full routing and camera-recording audio remain unverified. |
| Remote and docks | IR remote and Samsung `sec_keyboard` docking station tested. Analog desk-dock audio routing has a compatibility change but still needs broader testing. |
| RIL | SIM/baseband, voice calls and 2G/EDGE mobile data tested. 3G untested on the available network. |
| GPS | Outdoor 3D fixes verified, including a fresh-state acquisition and about 21 minutes of subsequent valid fixes. First acquisition may take several minutes; an initial daemon restart was observed. |
| WebView video | Local H.264 and HTTPS 960x540 video tested in Jelly with real Mali-400 / GLES2 capability detection. Other browsers/codecs/DRM are not covered by this result. |
| Recovery | Automatic recovery restoration disabled through `BOARD_NO_RECOVERY_PATCH`; installed recovery retained after flash and boot. |

System freezes/crashes have been reported during bring-up; long-term stability is not established. Historical SystemUI crashes in the Mali driver around EGL surface queries have not been proven fixed. USB/host ADB transport can be intermittent. Bluetooth, hotspot, USB OTG, external SD, camera/video-recording paths, S Pen features, all sensors, suspend/battery life, IPv6 mobile data and DRM are not comprehensively tested. Tablet UI/density and automatic OTA-feed delivery need further validation.

Google apps are not bundled; GApps and microG combinations are untested. This is a `userdebug` build with an unauthenticated ADB bring-up policy and a legacy kernel/vendor stack; it is not a modern production security baseline.

## WebView compatibility

The shared Exynos4 tree supplies `exynos4_webview_gles2`. Chromium's GPU-info collector requests ES3 and disables version fallback; Mali-400 only provides ES2. Enabling fallback prevents WebView from incorrectly forcing bitmap rendering and hiding normal video layers.

The build carries the equivalent source patch plus a strict transformation for the verified ARM WebView **156.0.8078.47**. It checks the complete library hashes and instruction context, preserves APK layout, and signs the result normally. An unknown bundled WebView revision intentionally fails the build until compatibility is reviewed.

Later app-store/manual provider updates are not patched by this build step. An unpatched replacement provider can bring the video issue back. Carrying the source fix in a maintained WebView source build is the preferred long-term path.

## Sources and building

This repository contains the device configuration, not the complete Android source tree. Matching proprietary blobs and the Exynos4 platform patches are required. The release's `source-revisions.json` records the published bring-up repositories.

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
