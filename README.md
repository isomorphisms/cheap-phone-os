# cheap-phone-os

A working root for a cheap-phone Android operating system based on the Android Open Source Project.

## Upstream

Canonical source:

- manifest: `https://android.googlesource.com/platform/manifest`
- tracking branch: `android-latest-release`
- current platform branch: `android17-release`

AOSP is a collection of Git repositories rather than one repository. This repository is the controlling root for the tree; the manifest remains the authoritative inventory of upstream component repositories.

## Get the complete upstream tree

```sh
sh _/sync-upstream
```

The script initializes Google's current AOSP manifest and synchronizes every repository selected by that manifest.

## Mirror policy

Do not treat the manifest repository alone as the operating system. Preserve upstream repository identity and provenance. Cheap-phone-specific work belongs on explicit branches or overlays so upstream source can continue to be reconciled cleanly.
