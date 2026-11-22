# GT-N8000 experimental test report — 2026-11-22

This report covers the frozen LineageOS21 Android14 GT-N8000 package proposed for publication as **lineage-21.0-20261122-HTML6405-n8000.zip**. It is not an N8010/N8013/N8020 test report or a general stability certification.

## Exact artifact

- Size:618126733 bytes.
- SHA256:`95402542b87045b5be9d3027d1180406e3d25e5745f02191a4f72816ecdb1df7`.
- Build incremental:`eng.html64.20261122.085904`.
- Android SDK34; declared security patch **2024-08-05**. This is not a current-security-patch claim.
- Packaged stock boot image SHA256:`6592550f6771c1541f733fc607748005d03a129f9114de9febc9599a6fa0be3d`.

The frozen ZIP passed an independent full ZIP CRC test and SHA256 check. It contains system/vendor block-update payloads and stock boot.img, with GT-N8000 assertions. The package does not target N8010 and contains no Magisk-named payload.

## Installation and runtime provenance

The existing compatible N8000 installation was updated without wiping data. Recovery completed successfully and the transferred ROM matched the host hash. The stock boot image was read back and matched before the owner-requested Magisk-patched boot was installed separately. **The release ZIP remains unmodified and does not include that patched boot.** Runtime acceptance below was performed on the owner's Magisk-rooted test device; a separate full unrooted-runtime acceptance run is not claimed.

Boot completed with kernel#15. The frozen build's tracked kernel source was verified by central integration as equivalent to Wacom-fix commit`829724c592e9752d3e7b5bdf8ae4450ce4f1446e`; later N8010 configuration/output changes are not the source or artifact of this release. SELinux remained Enforcing.

## Observed results

| Area | Result and practical limit |
| --- | --- |
| Boot / update | Successful recovery update and complete boot. Initial post-update ART cache rebuilding explained the longer first logo phase in the preceding integration test. No clean install, factory reset, stock migration or alternate partition-layout acceptance is claimed. |
| Pen / microphone privacy | Standard SW_PEN_INSERTED replaces the legacy switch code that Android interpreted as a hardware microphone-mute switch. The user completed the requested pen insertion/removal and microphone tests. Post-test capture showed no hardware microphone block and a stable Phone process. Privacy settings were not bypassed. |
| Phone graphics | The targeted Phone settings screen opened with the permanent graphics-library labels and ION access correction; repeated launches in the preceding integration and latest acceptance did not reproduce the EGL-loader abort. The old crash is not counted as a new failure. |
| Bluetooth calls / audio | The user confirmed headset-call audio and media playback, including return to the tablet speaker after disconnect. A subsequent integrated audio-service test confirmed a successful call and acquisition/release of the client-interface wake lock through the correct Binder path. These are bounded headset/call tests, not exhaustive coverage of all headsets, codecs, routes or long calls. |
| Telephony state | An active data subscription was observed after complete initialization with SIM present. SIM removal during the later sleep test was intentional and is not classified as a modem failure. The sleep test itself is not another cellular-call/data test. |
| Latest-ROM sleep / USB reconnect | About27 minutes of real cumulative system suspend, nine complete resume cycles, successful USB reconnect, unchanged boot identity and stable Phone/SurfaceFlinger/SystemServer processes. No new crash, ANR, kernel panic or AVC after the sleep baseline. |
| Earlier overnight control | A preceding integration survived approximately7h48 of real suspend over an eight-hour battery interval without another SurfaceFlinger crash. This supports the direction of the fixes but is not an overnight test of the final Wacom package. |
| DT2W | Default initialization/toggling had been tested in the preceding integration; the final package's post-sleep PowerManager state still reports DT2W enabled. Required keyboard workaround is retained. Physical dock/Space-key acceptance is not newly claimed. |

For the latest sleep test, battery realtime37m44.827s minus battery CPU-uptime10m45.570s gives **26m59.257s** of suspend. The matching total-realtime/uptime difference agrees. Existing tombstone listings were identical before and after; the crash buffer remained empty. Historical RIL-startup AVCs at09:13 preceded the sleep baseline and are not new resume failures.

## Remaining limits

- A bounded successful resume test does not establish that every previously sporadic standby/USB failure is permanently eliminated. More long-duration use and repeated reconnections remain useful.
- Legacy RIL still probes an old private Telephony MTU-database path and receives limited `/data/data` label/category denials before using its built-in fallback. No broad app-data access or MCS bypass was added for it.
- SELinux Enforcing does not mean globally neverallow-clean. The device retains its existing legacy neverallow-ignore configuration. Targeted policy/delta checks are not full security certification.
- Prime preview artifacts and broad DRM/player/codec compatibility are not fully cleared by these audio, graphics and pen tests. No new HEVC quality/performance certification is claimed here; describe actual packaged codec exposure from the verified registry rather than assuming a new capability.
- GPS outdoor fix, hotspot, all sensors, physical keyboard/dock combinations and an exhaustive clean-setup/full-function matrix are not newly certified.
- N8010 is a separate build and device-acceptance stage. Its configuration changes must not be substituted for this frozen N8000 kernel/artifact.

This remains an **experimental userdebug prerelease**. Public source revisions/supplements and release assets must be verified separately by the release coordinator before publication. No publication is implied by this report.
