# Repository policy

## Upstream boundary

- Canonical upstream is Google's AOSP Git service, not an archived GitHub mirror.
- Track `android-latest-release`; record the resolved platform branch and manifest head when refreshing upstream.
- AOSP is a manifest-selected collection of repositories. Never describe the manifest repository alone as a complete Android source mirror.
- Preserve upstream repository names, history, licenses, and provenance when importing or reconciling source.

## Cheap-phone work

- Keep cheap-phone-specific changes distinguishable from upstream source.
- Prefer new work on branches and conservative merges.
- Low-memory behavior is an explicit target; do not silently generalize device-specific observations into platform-wide claims.
- Evidence from emulators, generic AOSP targets, and physical phones must be labeled separately.

## Layout

- Build, fetch, and generated work belongs under `_/`.
- `_/aosp/` is a local complete AOSP checkout and is intentionally ignored by the controlling Git repository.
