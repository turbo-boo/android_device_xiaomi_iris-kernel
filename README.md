# Prebuilt kernel for Redmi Note 10 JE (iris / XIG02)

Stock kernel, DTB, DTBO and vendor kernel modules of MIUI
**V14.0.26.0.TKRJPKD** (Linux `5.4.259-perf-g98b987981a60`), used by
[`device/xiaomi/iris`](https://github.com/turbo-boo/android_device_xiaomi_iris).
Xiaomi has not published the kernel source for this device.

| Path | Source in the stock firmware |
|------|------------------------------|
| `kernel` | kernel of `boot.img` |
| `dtb/holi.dtb` | DTB of `vendor_boot.img` |
| `dtbo.img` | `dtbo.img`, truncated to the size in its DT table header (1122634 bytes) |
| `vendor/*.ko` | `/vendor/lib/modules/*.ko` |
| `vendor/modules.load` | `/vendor/lib/modules/modules.load`, duplicate lines removed |

The modules are installed to `/vendor/lib/modules` through
`BOARD_VENDOR_KERNEL_MODULES`; the build generates `modules.dep` and friends.

## Updating

Replace every file from the same firmware release, never mix releases: the
modules only load into the kernel they were built with. Then update the
security patch level (`BOOT_SECURITY_PATCH`) and the build fingerprint in the
device tree to match that release.
