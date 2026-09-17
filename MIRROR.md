# Mirror shape

AOSP cannot be represented faithfully as a native GitHub fork of one repository because upstream is a Repo-managed collection of Git repositories hosted on `android.googlesource.com`.

`cheap-phone-os` is therefore the controlling repository for the fork. A complete upstream checkout is produced from the canonical manifest by `_/sync-upstream`.

The long-term GitHub mirror should preserve component-repository boundaries rather than flattening roughly 250 GB of checkout data into one Git repository. Component mirrors can then be substituted into a local manifest while this repository remains the root for cheap-phone-specific policy, overlays, receipts, and build entrypoints.
