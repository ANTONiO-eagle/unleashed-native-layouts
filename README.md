# Unleashed native keyboard-layout candidate and firmware-size evidence

This repository separates two related but **not interchangeable** items:

1. A GPL-3.0 source kit for an experimental Bad USB per-script native `.kl` layout feature, based on Unleashed `unlshd-095` (`9c6327764c18fc8376b5941aa8bde79fe681fe8f`). It is a candidate, not an upstream pull request or official Unleashed release.
2. A measured size comparison of two F7 firmware payloads. The comparison is descriptive only; it does **not** isolate the savings caused by LTO or any individual code change.

## Source kit

Download [`hybridus-module2-native-layouts-source.zip`](hybridus-module2-native-layouts-source.zip). It contains the patch, the ten full modified/added source files, GPL-3.0 license and notices, tests, test logs, stock layout fixtures, format evidence, and limitations.

The kit's recorded validation is for Unleashed 095E/API 88.26. Its verification record reports a 65,664-byte external FAP with `APPCHK` passing and host tests passing; no physical-device, real USB/BLE-host or real SD/FlipperFormat test was performed. The source documents that `.kl` is a 256-byte ASCII-indexed HID table and does **not** implement UTF-8, Unicode, or Cyrillic.

On 2026-11-19, the patch passed `git apply --check` against the public upstream `dev` commit `6c4574bcb2674c4b6e5630f80ee1202eb4101241`. A clean current-`dev` build and rerun of the tests were not completed in this session (the dependency/submodule setup timed out); this is not evidence of a current-`dev` build.

## Firmware-size comparison

See [`firmware-payload-comparison.md`](firmware-payload-comparison.md) for the method, exact byte counts, SHA-256 digests, API versions, and limits. The observed Hybridus payload is 2,464 bytes smaller than the official `unlshd-095` payload, but API versions differ (88.27 vs 88.26) and the builds are not a controlled LTO-only comparison. **No LTO saving or per-feature firmware-flash delta is claimed.**

## Upstream discussion

The feature was discussed in [DarkFlippers/unleashed-firmware issue #1218](https://github.com/DarkFlippers/unleashed-firmware/issues/1218). No pull request is included or implied by this repository.

## License and status

The source kit contains its GPL-3.0 license and notices. It is an unaffiliated, experimental candidate. No firmware binary, VGM files, or installation instructions are distributed here.

## Upstream outcome

A separately prepared implementation of `DUCKY_LANG` was merged into Unleashed `dev` in [PR #1224](https://github.com/DarkFlippers/unleashed-firmware/pull/1224) ([merge commit `690af3d`](https://github.com/DarkFlippers/unleashed-firmware/commit/690af3d02bd5bc2e285ee509b9cdd78483aa9a92)). This repository remains a historical API 88.26 source kit; it is not the merged diff or a shipping release. The merge covers per-script selection of native `.kl` layouts; it does not add a new saved default or Unicode support. Check the upstream releases separately for stable availability.
