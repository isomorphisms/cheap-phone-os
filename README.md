# cheap-phone-os

A working root for a cheap-phone Android operating system based on the Android Open Source Project.

## Upstream

Canonical source:

- manifest: `https://android.googlesource.com/platform/manifest`
- tracking branch: `android-latest-release`
- current platform branch: `android17-release`

GitHub mirror branches in this repository:

- `upstream-manifest` — exact imported history of Google's `android-latest-release` manifest branch
- `cheap-phone-manifest` — the same manifest with the AOSP component remote made absolute so this GitHub repository can be used directly as a Repo manifest source

AOSP is a collection of Git repositories rather than one repository. This repository is the controlling root for the tree; the manifest remains the authoritative inventory of upstream component repositories.

See [SOURCES.md](SOURCES.md) for the high-value AOSP layers and their current LineageOS and GrapheneOS counterparts.

## Get the complete upstream tree

```sh
sh _/sync-upstream
```

The script initializes from this repository's `cheap-phone-manifest` branch and synchronizes every AOSP component selected by that manifest from Google's canonical Git service.

## Mirror policy

Do not treat the manifest repository alone as the operating system. Preserve upstream repository identity and provenance. Cheap-phone-specific work belongs on explicit branches or overlays so upstream source can continue to be reconciled cleanly.
