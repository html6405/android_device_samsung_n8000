# GT-N8000 LineageOS 21 changelog

## Source maintenance

The maintained repositories use `lineage-21.0-2026`; see the [pinned source manifest](release-source-manifest.xml). Backports are recorded as individual fixes with their original source or explicit LOS21 adaptation. This source-history/documentation cleanup does not change the published ROM ZIP.

## 2026-11-21 — Experimental

Current evidence: published 2026-11-21 LOS21 experimental build, Android 14, kernel build #11, SELinux Enforcing. [Release and assets](https://github.com/html6405/android_device_samsung_n8000/releases/tag/lineage-21.0-20261121). This experimental publication is not a claim that the remaining known crashes are fixed.

ROM: [`lineage-21.0-20261121-HTML6405-n8000.zip`](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-21.0-20261121/lineage-21.0-20261121-HTML6405-n8000.zip), **618141142 bytes**; [checksum sidecar](https://github.com/html6405/android_device_samsung_n8000/releases/download/lineage-21.0-20261121/lineage-21.0-20261121-HTML6405-n8000.zip.sha256). SHA-256: `c1803dcfc5433d4c778882b640016dbacf00a3c791296d14f295f9e802d7eea3`.

### Integrated changes

- Resolve legacy linker shim paths through O_PATH/procfs without requiring read/getattr access to every configured target.
- Honor CAP_SYS_NICE when attaching another user's tasks to legacy cgroups; the remaining Bluetooth cgroup EINVAL is a separate issue.
- Keep the legacy named-file tombstone fallback write-only when passing its descriptor to crash_dump under Enforcing.
- Restore vendor-executable SELinux domain transitions on the legacy kernel by removing `nosuid` from the vendor mount. Power, Audio, Bluetooth, Trust, GPS, RIL and keyboard services were observed in their intended domains.
- Initialize a missing double-tap-to-wake setting from the device default, preserving an explicit user choice. Fresh missing-setting reboot test and kernel toggling passed.
- Preserve the refined PID-based `sec_keyboard_kill` workaround, private PID state and scoped labels. Sleep/wake service lifecycle passed; physical dock wakeup remains to test.
- Add scoped GPS, S-Pen, WLAN, audio-property and Trust-Hwservice labeling/policy corrections. Preserve the private PARAM backup/helper/marker handling. Existing valid PARAM state was retained, so this is not a fresh partition-write test.
- Correct cropped AVC buffer reallocation and stale input-flush/DPB handling. Add the Stagefright render-timestamp overflow guard. The same synthetic sample improves from 94/96 to 96/96 GPU-output frames; release-build 96-frame and five seek/flush checks pass. Actual Prime testing still reproduces intermittent green preview lines, including with HEVC hidden. The artifact cause and broad DRM/player compatibility remain unresolved.
- Restore AIF2 Slave mode before Bluetooth-call modem PCM setup and restore speaker mixer level after SCO teardown. Focused actual-source/XML checks and component builds passed; real repeated LOS21 headset calls remain pending.
- Restore ARM GETREGS fallback for native crash diagnosis on kernels rejecting GETREGSET. The flashed library produces complete nonfatal ARM backtraces and a usable fatal SurfaceFlinger tombstone. This improves diagnosis, not crash stability.
- Intentionally stop advertising HEVC through OMX/C2 codec XMLs in the released build. Runtime registry confirms zero offered HEVC decoders. This is a capability workaround, not an HEVC decoder repair, and it did not prevent the reported green Prime preview lines. HEVC-only platform playback is unsupported in this package.
- Regenerate the final build configuration and OTA metadata: timestamp `1795288401`, incremental `eng.html64.20261121.201321`. Keep the existing stock compatibility fingerprint and security-patch level `2024-08-05`; do not mistake the new build date for new security fixes.

### Tested scope

- Recovery installation without wiping data, full boot and all eight matching installed component hashes on the existing 16 GB GT-N8000 test installation. Its actual layout is 2 GiB SYSTEM / 500 MiB HIDDEN-VENDOR / 152 MiB CACHE, with 8 MiB BOOT and recovery; recovery SHA-256 stayed unchanged.
- SELinux Enforcing throughout captured tests; no new denials in the short post-integration video captures.
- Before capability hiding, clear Main-profile HEVC clips decoded through software OMX: 480p/720p surface throughput exceeded nominal rate, while tested 1080p output was about18–22fps. These historical results characterize the earlier enabled path; release-build registry confirms HEVC is deliberately unavailable. They do not certify EOS, Main10/HDR, secure content, pixel correctness, A/V synchronization or thermal endurance.
- Diagnostic-only temporary codec re-exposure on the final flashed build completed eleven bounded CPU/GLES runs. Its 1080p GLES output remained about19.6–21.4fps against25–30fps sources; fresh720p GPU starts varied about27–43fps, including one below30fps. This characterizes those clear clips, not consistent720p smoothness, pixel correctness, DRM certification or a HEVC cause of the green Prime previews. The HEVC-hiding configuration was restored after the test.
- Repeated real system suspend/resume transitions were recorded. A later standby/USB reconnect did not crash, but an earlier longer-resume crash was reproduced. No long-term stability conclusion follows from one clean reconnect.
- The independent register probe reported no mismatch during its observed interval but was stopped for the planned flash before completing12hours of active-time checks. It is not a completed12-hour pass or an ongoing final-ROM test.

### Known issues and pending acceptance

- **SurfaceFlinger resume crash remains open.** The `app` thread reads an invalid `preferredFrameTimelineIndex` after standby. Unexpected data in a block initialized from a long-lived NEON zero constant suggests register/context loss or another clobber; the actual cause is not proved. The known CPU_PM VFP notifier fix is already present and must not be double-ported.
- **Green Prime preview artifacts remain open.** They occurred again with HEVC advertisement disabled; that change is not a demonstrated fix or proof that the previews were HEVC. Prior captured preview selection used Samsung AVC. Do not conflate this visual issue with the independent SurfaceFlinger standby crash.
- A separate earlier first-boot kernel allocator panic around ADB reconnection is unresolved.
- Bluetooth background-cgroup `EINVAL` remains. WBS/CVSD configuration requires an Android14-specific stack adaptation and real codec-selection/call testing; adding only the LOS23 property would not implement it here.
- `/data/data` categories can be reapplied after the early platform-init label correction. Full startup and policy acceptance remain pending despite observed Enforcing operation.
- GPS fix, full pen/dock/camera behavior, voice/SMS/data, broad audio routing, clean setup and app combinations are not comprehensively tested.
- Fresh final OTA metadata is verified; the earlier test archive's `1743884445` / `eng.html64.20250405.222046` fields are superseded. The public release assets and checksum are linked above; automatic feed delivery remains unvalidated.

N8010 is the next model for its own build and hardware validation. N8000 evidence does not validate N8010, N8013 or N8020. See [README](README.md) for requirements, test limits and the intended model-specific release workflow.
