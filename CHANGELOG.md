# GT-N8000 LineageOS 23.2 preview changelog

## 2026-11-24 — VFP kernel preview

- Add the upstream ARM VFP hotplug fix to clear the cached hardware owner when a CPU goes offline.
- The matching Boot-only test completed one long suspend/wake cycle with 13 h 42 min of cumulative suspend, unchanged core process IDs, SELinux Enforcing and an empty crash buffer on the tested hardware.
- The signed full-ROM preview keeps the November 22 Android build identity. The complete ZIP passed host checks. Device validation used the same kernel installed through Boot only.
- ROM SHA-256 `f8a6c543d7f1b0d2d9988fcc4846c03dd6d5df27aed4ec23962f8a1def6af9b5`. [Manual preview for GT-N8000](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-23.2-20261124-vfp-preview)

## 2026-11-22 — experimental Enforcing

ROM package: `lineage-23.2-20261122-HTML6405-n8000.zip` (690,402,336 bytes)
Incremental: `1795369378`
SHA-256: `1f083b4ff63eaa0606b7243947323f1579a7cc5a0be0604edcc326fe29c28bd4`

- Full ROM now boots SELinux Enforcing. Give the legacy ION heap-mask property an Exynos4 label, permit initialization by `vendor_init`, and allow the four observed Codec2 client domains to read it, addressing blocked audio-decoder buffer allocation. Existing domain exceptions/Neverallow violations and security hardening remain outstanding.
- Retain the original dock keyboard driver without the failed experimental hotplug/wake changes. Automatic software-keyboard handling and reliable long dock sleep/wake remain unresolved.
- Include the reviewed Vanilla OTA backuptool prerequisite correction for Lineage 23 and embedded SAR product discovery. Existing app-only microG survived one actual Vanilla update; this does not validate privileged addon.d preservation or two OTA cycles.
- The exact full ZIP was installed without wiping/repartitioning; packaged kernel/readback matched. Post-update Enforcing boot, WLAN connectivity, YouTube Samsung hardware AVC playback, Maps rendering, retained microG self-check/options/grants and optional KernelSU manager recognition passed. SIM was absent before/after: calls/SMS/mobile data were not retested on this exact ZIP.
- Earlier Enforcing runtime checks covered audio/Prime, Dots bidirectional calls followed by media, GPS, S Pen/sensors and suspend/wake. These remain earlier-runtime evidence, not a comprehensive new-ZIP regression pass.
- HEVC remains disabled. Prime preview green bars, the earlier transient seek artifact, historical Mali crashes and the recurring `ueventd` crash class remain unresolved. No general stability-fix claim.
- Vanilla ROM: no Google services, microG, Minimal-GApps or KernelSU Manager APK bundled. Optional manager 32680/UAPI 5 is linked in README; kernel 32676 retains full UID checks disabled and is not claimed hardened.

## 2026-11-21 — experimental

ROM package: `lineage-23.2-20261121-HTML6405-n8000.zip` (690,405,912 bytes)
SHA-256: `443563c69c8e1ff5ead5d74f8af8210e3a8418833335f779e0b57074d44a13c5`

- Temporarily stop advertising HEVC decoders on Exynos4 while HEVC playback remains unresolved. AVC and audio codec configuration is retained. The final packaged ROM passed a Prime playback and forward/backward seek smoke test using Samsung AVC, with a smooth start and good audio reported by the user. HEVC-only platform playback remains unsupported.

- Correct cropped AVC output buffer reallocation and reset stale MFC frames after seeking. The tested 960×540 Prime stream now uses 960×544 buffers instead of 1920×1080. The final packaged-ROM smoke test confirmed playback and forward/backward seeking; broader app coverage remains pending.

- Known issue: one transient partial-image freeze was observed after seeking in Prime and recovered spontaneously. Subsequent forward/backward seek tests with diagnostic logging were clean; the issue is not established as fixed. The diagnostic library is not intended for release.

- Bluetooth call audio through the tested Dots passed in both directions on the 2026-11-21 GT-N8000 ROM. Media audio through the Dots also worked after hanging up. This was a Permissive test with the HEVC-only runtime configuration; final Enforcing and broader headset coverage remain pending.

- A short camera recording and local Gallery playback passed with picture, sound and forward seeking on the tested GT-N8000. Logs confirm Samsung AVC encoding and Gallery AVC hardware decoding. These checks used the 2026-11-21 Permissive ROM with the HEVC-only runtime configuration.

- An unplugged sleep test on the earlier candidate with the same source configuration captured about two hours of cumulative suspend over a 2 h 20 min window. Battery fell from 100% to 97%; the user reported about three seconds to display wake and then responsive UI. Overnight battery life and long-term stability remain unverified.

- This remains a Vanilla, experimental userdebug release with SELinux Permissive, existing domain exceptions and kernel debug history enabled. GApps/microG variants and their persistence across Vanilla OTA updates are not validated. The final package was flashed without wiping data; recovery and storage metadata were preserved.

## 2026-11-13 — experimental

ROM package: `lineage-23.2-20261113-HTML6405-n8000.zip` (689,968,737 bytes)
SHA-256: `5548c3ae73d84b85c6d0fedf1b3ffc5eddf825f0cfe07778d0b642d4068beaab`

- Updated Exynos4 graphics compatibility for the legacy Mali-400 GLES renderer and EGL behavior.
- Kept SurfaceFlinger screenshot layers alive while deferred rendering completes, with regression coverage for layer removal and cross-thread cleanup. A GT-N8000 run also completed 20 captures across ten Settings/Home cycles without restarting SurfaceFlinger; this does not establish a fix for the separate Mali `eglQuerySurface` crashes.
- Improved legacy Samsung device support across USB FunctionFS, GPS, RIL/mobile-data compatibility, software audio decoding, and WebView GLES2 video rendering.
- Added or refined shared dock handling, including legacy 30-pin dock detection, configurable analog-dock audio behavior, and USB function selection. IR remote and `sec_keyboard` docking station operation were confirmed on the test device.
- Added an optional P4NOTE repeated-router-advertisement offload experiment in the kernel. It is disabled by default and is not a general Wi-Fi performance fix.
- Restored the recovery-patch opt-out and omitted the unused standalone recovery image from the ROM package, reducing package size.
- Updated kernel-side Exynos4 device support. Further behavior and stability testing is still in progress.

### Tested on the GT-N8000

- Android boots with GLES rendering; Wi-Fi internet access and video playback work on the tested unit.
- Voice calls and 2G/EDGE mobile data work. 3G was unavailable for testing on the local network.
- GPS acquired outdoor 3D fixes; a fresh-state test took about 217 seconds to acquire a fix and then retained valid fixes for about 21 minutes.
- IR remote and `sec_keyboard` docking station work.
- Recovery remained installed after updating an existing compatible LOS 23 installation.

### Known gaps

- The release is an experimental userdebug build. System freezes have occurred, and long-term stability is not established.
- Historical SystemUI Mali `eglQuerySurface` crashes are not confirmed fixed.
- Audio routing remains incomplete: system/ringtone and camera-recording paths need further testing.
- Bluetooth, hotspot, USB OTG, external SD, camera, S Pen, all sensors, long suspend, battery life, IPv6 mobile data, DRM, and 3G remain unverified or only partially tested.
- The included WebView compatibility change targets the bundled WebView version documented in the release README; app-store updates are not patched automatically.
- N8010, N8013 and N8020 require separate device-specific ROM builds. This N8000 ZIP must not be flashed on those models.

See the [release page](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-23.2-20261113) for the package, checksum, installation notes, and full test report.
