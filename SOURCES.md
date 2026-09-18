# Android source map

Observed 2026-09-17; rechecked 2026-09-18.

This file identifies the parts of Android that matter most to cheap-phone work. It is a working map, not a replacement for the complete manifest-selected tree.

## Source families

- **AOSP** is the canonical upstream. `android-latest-release` currently selects `android17-release`.
- **LineageOS** is the most useful broad device-support downstream to keep alongside AOSP. Its controlling manifest is `LineageOS/android`; the current default branch is `lineage-23.2`.
- **GrapheneOS** is a useful hardened downstream and source of reference implementations. Its controlling manifest is `GrapheneOS/platform_manifest`; the current default branch is `17`.
- A downstream manifest may use its own fork for one component and the AOSP repository directly for another. Always follow the manifest rather than assuming every path has a downstream fork.

## First-wave components

| AOSP checkout path / repository | Why it matters here | LineageOS | GrapheneOS |
| --- | --- | --- | --- |
| `platform/manifest` | Manifest repository: complete source inventory and revisions; `repo` stores it under `.repo/manifests` rather than as a normal source checkout path | `LineageOS/android` | `GrapheneOS/platform_manifest` |
| `build/make` | Product definitions, Android Go defaults, image composition | `LineageOS/android_build` | `GrapheneOS/platform_build` |
| `build/soong` | Build graph and Android.bp machinery | `LineageOS/android_build_soong` | `GrapheneOS/platform_build_soong` |
| `system/core` | init, properties, process groups, fastboot plumbing, early boot | `LineageOS/android_system_core` | `GrapheneOS/platform_system_core` |
| `packages/modules/adb` | Android Debug Bridge client/daemon implementation | `LineageOS/android_packages_modules_adb` | `GrapheneOS/platform_packages_modules_adb` |
| `system/fs/fs_mgr` | Android 17 mount/fstab/verity/overlayfs code; directly relevant to new filesystems | In `android_system_core/fs_mgr` on `lineage-23.2`; follow the Lineage manifest as this split changes | `GrapheneOS/platform_system_fs_fs_mgr` |
| `system/vold` | Removable/adoptable storage and volume lifecycle | `LineageOS/android_system_vold` | `GrapheneOS/platform_system_vold` |
| `system/memory/lmkd` | Low-memory pressure policy; directly relevant to Android Go behavior | Current `lineage-23.2` manifest uses AOSP directly | Current GrapheneOS 17 manifest uses AOSP directly |
| `frameworks/native` | Binder-native plumbing, SurfaceFlinger, native graphics/services | `LineageOS/android_frameworks_native` | `GrapheneOS/platform_frameworks_native` |
| `frameworks/base` | system_server, ActivityManager, package/window management, low-RAM framework behavior | `LineageOS/android_frameworks_base` | `GrapheneOS/platform_frameworks_base` |
| `art` | DEX/ART execution, verification, compilation, runtime | `LineageOS/android_art` | `GrapheneOS/platform_art` |
| `bionic` | libc, dynamic linker, native process boundary | `LineageOS/android_bionic` | `GrapheneOS/platform_bionic` |
| `libcore` | Java core libraries used by ART/framework code | Current `lineage-23.2` manifest uses AOSP directly | `GrapheneOS/platform_libcore` |
| `libnativehelper` | JNI/native helper boundary | Current `lineage-23.2` manifest uses AOSP directly | Current GrapheneOS 17 manifest uses AOSP directly |
| `hardware/interfaces` | HAL contracts for sensors, graphics, audio, camera, power and other hardware | `LineageOS/android_hardware_interfaces` | `GrapheneOS/platform_hardware_interfaces` |
| `system/sepolicy` | SELinux policy for services, filesystems, device nodes and permissions | `LineageOS/android_system_sepolicy` | `GrapheneOS/platform_system_sepolicy` |

## Kernel and device layer

The table above is not enough to boot a physical phone.

For every target device also preserve and map:

- the exact kernel tree and kernel configuration;
- `device/<vendor>/<device>`;
- `vendor/<vendor>/...` material that can legally be mirrored;
- hardware-specific HALs and sepolicy;
- bootloader and partition-layout assumptions;
- firmware and proprietary blobs as external inputs when they cannot be redistributed.

For the cheap Android Go phone, record the actual ARMv7/Thumb-2 CPU, kernel branch/config, eMMC/UFS/NAND/storage controller details, RAM/zram configuration, device tree, boot image layout and partition table separately from generic AOSP facts.

## Work priorities

### Storage path

For append-only FAT and SD-card work, trace:

`kernel filesystem/block layer -> fs_mgr -> vold -> framework storage APIs -> app/adapter behavior`

Do not claim that a userspace mount or emulator test proves kernel integration or physical removable-media behavior.

### Low-memory path

For Android Go and cheap-device memory work, trace:

`kernel VM + PSI + zram -> lmkd -> libprocessgroup/cgroups -> ActivityManager -> process/application behavior`

Keep kernel pressure, compressed swap, userspace kill policy and framework process policy as separate layers.

### Direct DEX/native path

For Idriç and direct DEX/JNI work, trace:

`DEX -> ART -> libcore -> JNI/libnativehelper -> bionic -> Binder/native services -> HAL/kernel`

Physical ARMv7 evidence remains distinct from emulator or generic AOSP evidence.

### Graphics path

For direct rendering work, trace:

`app/native producer -> Binder/BufferQueue -> SurfaceFlinger -> composer HAL -> kernel/display driver -> panel`

## Mirror policy

The eventual GitHub mirror should include every repository selected by the controlling manifest, preserving repository boundaries and history.

The components above are the first repositories worth making convenient to inspect, diff and modify. They are not the boundary of the OS.

Keep three relationships explicit:

1. canonical AOSP provenance;
2. LineageOS changes and device-support work;
3. GrapheneOS changes worth studying or selectively reproducing.

Cheap-phone changes should remain a fourth, separately attributable layer.
