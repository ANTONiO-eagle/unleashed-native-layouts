# F7 firmware payload size comparison (not an LTO attribution)

**Measured on 2026-11-19 from the two `.tgz` files named below.** The quantity compared is the raw firmware payload inside the DfuSe `firmware.dfu`, not the archive size, DFU wrapper size, updater, radio firmware, resources, or apps.

| Package | Source / identity | API | Payload bytes | Payload SHA-256 |
|---|---|---:|---:|---|
| Hybridus LTO preserve, F7 | Owner-provided `flipper-z-f7-update-hybridus-lto-safe-20261118.tgz`; archive SHA-256 `c8798c2bf3478a31da18405f159598f8b708a553b5dadbbddbf318c7954baea7` | 88.27 | 872,916 | `6b195b417321e49dd23a4ae1b2db02384743c26e3c5998898954c728cf5d5d44` |
| Unleashed `unlshd-095`, F7 | Official release page: <https://github.com/DarkFlippers/unleashed-firmware/releases/tag/unlshd-095>; firmware package linked there: <https://unleashedflip.com/fw/unlshd-095/flipper-z-f7-update-unlshd-095.tgz>; downloaded archive SHA-256 `be8639a4b4659a4e3790e5d18fecc755e4eb6c90b7c68810639fa5bb61f48608` | 88.26 | 875,380 | `b46373377ad40074b260f258ae39a9a8925aa1c703732c6abce14cab76861829` |

Both DfuSe elements start at `0x08000000`; each `.dfu` wrapper is 309 bytes larger than its payload. Subtracting the payload counts gives **872,916 − 875,380 = −2,464 bytes**: the Hybridus payload is 2,464 bytes smaller in this particular pair.

## How to reproduce

1. Obtain the official `unlshd-095` package from the release link above and verify its archive SHA-256.
2. Open each `.tgz`, read its `firmware.dfu`, parse the DfuSe target and element, and extract the element at `0x08000000` (not the wrapper). The element lengths and hashes above are the measured values.
3. For the Hybridus package, the checked archive was the owner's Drive file with the name and SHA-256 shown above. The report does not redistribute that firmware package.

## What this does and does not establish

- It establishes only the byte lengths and hashes of these two extracted payloads.
- The official release identifies API 88.26; the Hybridus build is API 88.27. The API mismatch alone means this is not a strictly matched build comparison.
- The packages are not controlled clean rebuilds under identical source, compiler, linker, feature, and build settings. Thus the 2,464-byte difference **cannot be attributed to LTO**, nor to any single feature. It is not evidence that LTO saved 2,464 bytes.
- This does not measure the firmware flash delta of the native-layout Bad USB patch. That patch is an external FAP candidate; an FAP's file size is not MCU firmware flash.
- The Hybridus audit's conditional 7,724-byte remainder assumes `SFSA=215`; the device's SFSA was not read. Actual app-accessible flash remainder is therefore unknown.
- No device was flashed as part of this comparison. Package size does not prove installed state or runtime behavior.
