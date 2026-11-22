# Samsung Galaxy Note 10.1 (3G) — GT-N8000

Device configuration for **unofficial LineageOS 21 / Android 14**, maintained by [html6405](https://github.com/html6405). Device codenames include `p4noterf` / `p4noterfxx`.

## Downloads

- [2026-11-22 experimental LineageOS 21 release](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-21.0-20261122) · [ROM ZIP](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-21.0-20261122/lineage-21.0-20261122-HTML6405-n8000.zip) · [SHA-256](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-21.0-20261122/lineage-21.0-20261122-HTML6405-n8000.zip.sha256).
- [Release archive](https://github.com/html6405/android_device_samsung_n8000/releases) · [Changelog](CHANGELOG.md) · [Shared model overview](https://github.com/html6405/android_device_samsung_n80xx-common/tree/lineage-21.0-2026).

For **GT-N8000 only**, unofficial LineageOS 21 / Android 14, experimental userdebug/Enforcing. N8010/N8013/N8020 require distinct builds and hardware acceptance; N8010 work proceeds separately. This is not a stable or security-current release.

ROM: `lineage-21.0-20261122-HTML6405-n8000.zip`, **618126733 bytes**. SHA-256:

```text
95402542b87045b5be9d3027d1180406e3d25e5745f02191a4f72816ecdb1df7
```

## Required partition layout and installation

The inspected 16 GB test unit has **2 GiB SYSTEM**, **500 MiB HIDDEN/VENDOR** and **152 MiB CACHE**; BOOT and its installed recovery partition are **8 MiB each**. This is the actual tested layout, not a claim that 2 GiB is the minimum LOS21 SYSTEM size or that every other layout is supported. The source SYSTEM image budget is 1400 MiB, smaller than the physical test partition. The ROM ZIP writes SYSTEM/HIDDEN/BOOT; it does not repartition, write CACHE or install a recovery image. The recovery partition's SHA-256 remained identical before and after the final update.

An update of the existing compatible LOS21 test installation through TWRP 3.7.0_9-0 without a data wipe was tested. Migration from stock, other Android versions, different layouts and a factory-reset setup are not covered. Keep independent personal-data/EFS backups and the original PARAM backup before any planned data wipe or partition work. Repartitioning is a separate operation, not a routine ROM update. Check the final release's model, layout and supplied SHA-256 before installing it.

## Changes and tested status

- Restore working Bluetooth cellular call audio with the legacy-controller CVSD policy and matching RIL audio-path ABI/enum corrections; preserve the prior AIF2/SCO mixer fixes. Two-way headset calls, Bluetooth media and speaker playback passed the observed tests. Call volume routing and cached-volume reapplication are corrected.
- Complete skipped single-SIM data commands and publish a subscription that becomes ready late; the user confirmed the mobile-data indication. This is not exhaustive carrier/APN/SMS validation.
- Route the Exynos4 audio service's framework AIDL clients through system Binder and permit typed SystemSuspend access. A 26-acquisition wake-lock probe passed (about 68 ms total); prior missing-SystemSuspend errors did not recur in the checked interval.
- Label the legacy Mali/UMP/secion/gralloc/mapper/allocator libraries for in-process use and permit the observed radio-domain ION write. The previous Phone UI EGL/allocator crash was resolved in the tested path.
- Report the standard Wacom pen-inserted switch; The input device now exposes SW_PEN_INSERTED instead of SW_MUTE_DEVICE; the false hardware microphone block cleared after reboot. The user completed pen and microphone/privacy checks. DT2W and the refined sec_keyboard_kill workaround remain enabled/preserved.
- Permit the observed Wi-Fi helper file-attribute lookup. Attempts by RIL to inspect private application database paths remain intentionally blocked; the fallback is retained.

## Remaining limits

SurfaceFlinger long-standby/resume SIGSEGV remains open: the latest short regression test does not establish a long-term fix. Intermittent green Prime preview artifacts remain unresolved. HEVC stays deliberately unadvertised; no HEVC/DRM compatibility claim. Camera remains HAL1/OpenCamera; Camera3/Aperture migration and GPS changes are deferred. No required keyboard/Bluetooth workaround was removed.

The latest no-USB test measured **26m59s cumulative real system suspend** across nine sleep/resume cycles, with the same boot and no new crash, ANR, tombstone or SELinux denial after its baseline. This was not one continuous long sleep. SIM had been deliberately removed during that interval, so the earlier cellular tests are separate evidence.

SELinux is **Enforcing**, but `SELINUX_IGNORE_NEVERALLOWS := true` and existing legacy exceptions remain. An independent assertion-enabled CIL check fails with **344 existing violation blocks** both before and after subtracting the ten new audio-Binder/radio-ION grants; no additional normalized violation was reported for those grants. This is not full neverallow or security compliance. Security patch level remains **2024-08-05**.

## Sources and building

Maintained source branches use **`lineage-21.0-2026`**. The [pinned source manifest](release-source-manifest.xml) selects the matching device trees, kernel, hardware and platform revisions. Proprietary vendor files are required separately.

Use the release-specific source manifest and revision records for exact provenance. A device-tree tag archive alone is not the complete Android build tree; proprietary vendor files are separate.

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
