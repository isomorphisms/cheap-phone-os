# Mirror shape

AOSP cannot be represented faithfully as a native GitHub fork of one repository because upstream is a Repo-managed collection of Git repositories hosted on `android.googlesource.com`.

`cheap-phone-os` is therefore both the controlling repository for cheap-phone work and an archive root for multiple viable Android source answers.

## Breadth-first rule

Do not optimize the mirror around one preferred ROM. Preserve as many technically viable public implementations as practical:

- canonical AOSP;
- broad device-support downstreams;
- hardened and privacy-oriented downstreams;
- x86, generic-system-image, and unusual hardware ports;
- device and kernel trees relevant to cheap or old hardware;
- historically useful projects whose source still contains a distinct answer even if the project is discontinued.

A dead project can still be valuable evidence. Mark it historical; do not silently drop it.

## Ref layout

Each imported Git repository keeps its own history under namespaced refs in this GitHub repository:

- upstream branch `17` from source `grapheneos-manifest` becomes `refs/heads/mirror/grapheneos-manifest/17`;
- an upstream tag `android-17.0.0_r1` becomes `refs/tags/mirror/<source>/android-17.0.0_r1`.

This avoids flattening unrelated working trees into `main`. Git naturally deduplicates identical commit, tree, and blob objects while the namespaced refs preserve provenance.

The seed inventory is [MIRRORS.tsv](MIRRORS.tsv). It is intentionally extensible: add manifests first, then add high-value component repositories, device trees, kernel trees, build systems, and archived implementations as they are verified.

Run:

```sh
sh _/mirror-sources
```

The importer is intentionally conservative. Ordinary fast-forward updates work. A non-fast-forward rewrite fails instead of erasing the previously mirrored history; archive the old ref explicitly before replacing it.

## Complete trees

A complete buildable tree still comes from the controlling manifest and its selected component repositories. `_/sync-upstream` materializes the current AOSP tree locally.

The long-term mirror should preserve component-repository boundaries and history, not flatten hundreds of gigabytes of checkout data into `main`. Additional component repositories can be added to `MIRRORS.tsv` and carried as their own namespaced ref families.

Cheap-phone-specific policy, overlays, receipts, build entrypoints, source maps, and provenance indexes stay on `main`.
