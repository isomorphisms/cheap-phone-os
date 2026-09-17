# Upstream record

Observed 2026-09-17.

Canonical AOSP manifest repository:

`https://android.googlesource.com/platform/manifest`

Tracked manifest branch:

`android-latest-release`

Observed manifest head:

`ad156f32caaa06dae91c02d443f6a8fe210eaa54`

That manifest selects:

`android17-release`

Do not substitute the archived `aosp-mirror/platform_manifest` GitHub repository as the authority. It has fallen behind Google's canonical manifest.

AOSP is intentionally split across many Git repositories. A complete source checkout is the set selected by the canonical manifest, not the contents of the manifest repository alone.
