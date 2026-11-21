# Samsung Galaxy Note 10.1 (3G) — GT-N8000

Device configuration for **unofficial LineageOS 21 / Android 14**, maintained by [html6405](https://github.com/html6405). Device codenames include `p4noterf` / `p4noterfxx`.

## Downloads

- [2026-11-21 experimental LineageOS 21 release](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-21.0-20261121) · [ROM ZIP](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-21.0-20261121/lineage-21.0-20261121-HTML6405-n8000.zip) · [SHA-256](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-21.0-20261121/lineage-21.0-20261121-HTML6405-n8000.zip.sha256).
- [GT-N8000 release archive](https://github.com/html6405/android_device_samsung_n8000/releases) — contains multiple Android versions; select a release explicitly marked LineageOS 21 for this model.
- [LOS21 changelog and pending changes](CHANGELOG.md).
- [Shared N80xx model overview](https://github.com/html6405/android_device_samsung_n80xx-common/tree/lineage-21.0-2026).

The **2026-11-21 experimental LOS21 release** retains the known SurfaceFlinger standby/resume crash and intermittent green Prime preview artifacts. It is not a stable release. Current testing uses an existing 16 GB GT-N8000 installation; it does not validate every storage variant, clean installation or Google-apps combination. **N8010, N8013 and N8020 require their own builds.** The LOS23.2 release in this repository is a different product/version.

Published ROM: `lineage-21.0-20261121-HTML6405-n8000.zip` — **618141142 bytes**. SHA-256:

```text
c1803dcfc5433d4c778882b640016dbacf00a3c791296d14f295f9e802d7eea3
```

## Required partition layout and installation

The inspected 16 GB test unit has **2 GiB SYSTEM**, **500 MiB HIDDEN/VENDOR** and **152 MiB CACHE**; BOOT and its installed recovery partition are **8 MiB each**. This is the actual tested layout, not a claim that 2 GiB is the minimum LOS21 SYSTEM size or that every other layout is supported. The source SYSTEM image budget is 1400 MiB, smaller than the physical test partition. The ROM ZIP writes SYSTEM/HIDDEN/BOOT; it does not repartition, write CACHE or install a recovery image. The recovery partition's SHA-256 remained identical before and after the final update.

An update of the existing compatible LOS21 test installation through TWRP 3.7.0_9-0 without a data wipe was tested. Migration from stock, other Android versions, different layouts and a factory-reset setup are not covered. Keep independent personal-data/EFS backups and the original PARAM backup before any planned data wipe or partition work. Repartitioning is a separate operation, not a routine ROM update. Check the final release's model, layout and supplied SHA-256 before installing it.

## Bring-up status

Evidence below distinguishes the earlier 2026-11-21 integration tests from the published LOS21 build with fresh metadata and HEVC advertisement disabled. It does not describe the LOS23.2 release or other models.

| Feature | Current LOS21 evidence |
| --- | --- |
| Boot / graphics | Final candidate boots with kernel build #11 and reaches Android. The SurfaceFlinger `app`-thread SIGSEGV reproduced after longer standby/resume on the earlier candidate remains unresolved; no repair is claimed. |
| DT2W | A missing secure setting defaults to enabled after reboot. Setting changes reach the kernel, and a double-tap event was observed. |
| AVC / Prime previews | Matching cropped AVC probe improves from 94/96 to 96/96 frames; release-build 96-frame and five seek/flush checks pass. Prime was exercised, but intermittent green preview lines reappeared even with HEVC hidden. That issue is not fixed, and HEVC causality is not established. General DRM/player compatibility remains unvalidated. |
| HEVC | **Intentionally not advertised in the released build** through OMX/C2 declarations; runtime registry reports zero HEVC decoders. HEVC-only platform playback is unsupported in this package. Earlier software-OMX tests showed functional 480p/720p and only about 18–22 fps at 1080p; those historical results do not mean HEVC is enabled in the final ROM. Hiding capabilities is a workaround, not a decoder repair or a proven cure for the green previews. |
| Bluetooth / call audio | AIF2-Slave restoration and post-SCO speaker-level fixes are built and installed. Repeated real two-way headset calls and post-call audio still require LOS21 testing. WBS/CVSD stack adaptation is not in this build. |
| RIL / mobile data | RIL starts in its intended SELinux domain. Voice, SMS and data operation are not functionally validated by this test cycle. |
| GPS / Wi-Fi | Services and scoped labels/rules were checked; the old reported denials did not recur in the short captures. Outdoor GPS acquisition, broader Wi-Fi behavior and hotspot need separate tests. |
| S Pen / keyboard / docks | S-Pen gesture node has the intended label. The PID-based `sec_keyboard_kill` workaround is retained and the daemon restarts after wake. Full pen behavior, physical dock/Space-key wake and analog-dock routes remain unverified. |
| Camera | Existing HAL1/OpenCamera path retained. Camera3 port is deferred; no complete camera-recording/playback validation in this cycle. |
| Recovery / updates | Final no-wipe recovery installation and all eight installed component checksums passed; recovery bytes remained unchanged. Fresh OTA timestamp/incremental were verified. Automatic feed delivery remains unvalidated. |

A diagnostic-only temporary re-exposure of the software HEVC decoder on the final flashed build completed eleven bounded clear-content runs. Its 1080p GLES throughput remained about 19.6–21.4 fps against 25–30 fps sources; fresh 720p GPU starts varied from about 27–43 fps, so consistently smooth 720p is not established either. The original HEVC-hiding XML was restored afterward and the registry again offered zero HEVC decoders. These tests do not establish pixel correctness or DRM compatibility, explain the green previews, or mean that the packaged ROM advertises HEVC.

The later sleep test demonstrated real system `mem` suspend and resume, with repeated intervening wakeups; it was not one continuous multihour sleep. A libc-free NEON-register probe reported no mismatch during the observed interval, then was stopped for the planned flash before completing its 12-hour active-time run. It is no longer a running test or a completed 12-hour pass. These observations do not establish a fix for the earlier SurfaceFlinger crash. A separate first-boot kernel allocator panic around ADB reconnection, Bluetooth background-cgroup `EINVAL`, and the reapplication of `/data/data` label categories remain open findings.

SELinux remained **Enforcing** during these LOS21 tests. Existing legacy exceptions and `SELINUX_IGNORE_NEVERALLOWS` remain, so this is not a claim of complete policy/security validation. A durable ARM GETREGS fallback now permits useful native crash traces on the old kernel. Full functional, long-term and clean-data acceptance is still pending. Google-apps/microG combinations are not covered by the recorded tests.

## Release workflow

Prepare this explicitly experimental LOS21 package with the unresolved SurfaceFlinger and green-preview issues visible, using a distinct `lineage-21.0-YYYYMMDD` tag. Each published release should include its exact model-specific ZIP, SHA-256 sidecar, `source-revisions.json`, manifest and source supplement with matching checksums. Record the actual artifact name, size, source state and test limits in the release body, this README and `CHANGELOG.md`; the shared N80xx README then links to that release. Publication is not a declaration that the remaining functional/stability tests passed.

The released build was built with regenerated configuration and fresh OTA metadata: `post-timestamp=1795288401`, incremental `eng.html64.20261121.201321`. This replaces the stale 2025-04-05 fields of the earlier integration archive. The existing security-patch level remains **2024-08-05**; a fresh build date is not a new security patch. Updater is configured for the LOS21 feed and model-specific GitHub release archive; actual OTA delivery is a separate check.

## Sources and building

Maintained source branches use **`lineage-21.0-2026`**. The [pinned source manifest](release-source-manifest.xml) selects the matching device trees, kernel, hardware and platform revisions. Proprietary vendor files are required separately.

The release tag records the original public device-tree baseline. The release assets retain the original manifest/revision/source-supplement records; the maintained branches now contain the same tested code with individual fixes and their backport origins. A device-tree tag archive alone is not the complete Android build tree.

- [n80xx-common LOS21](https://github.com/html6405/android_device_samsung_n80xx-common/tree/lineage-21.0-2026)
- [smdk4412-common LOS21](https://github.com/html6405/android_device_samsung_smdk4412-common/tree/lineage-21.0-2026)
- [Exynos4 kernel LOS21](https://github.com/html6405/android_kernel_samsung_smdk4412/tree/lineage-21.0-2026)

With the matching local Android 14 build tree and dependencies in place:

```sh
source build/envsetup.sh
lunch lineage_n8000-ap2a-userdebug
m -j8 bacon
```

Builds sharing one output tree must be serialized; normal configuration regeneration is required for release metadata and new build definitions. Please report the exact model/build, reproduction steps and sanitized logs. Keep EFS backups, IMEI/IMSI and personal location data private.

## Original device-tree notice

The original `README` is retained unchanged. Its text is preserved here:

Device configuration for the Samsung Galaxy Note 10.1 (3G)

p4noterf a.k.a. GT-N8000

Copyright (C) 2012 The CyanogenMod Project

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0
