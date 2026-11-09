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

The vendor modules are installed to `/vendor/lib/modules` through
`BOARD_VENDOR_KERNEL_MODULES`; the build generates `modules.dep` and friends.

The stock `vendor_boot.img` also carries 42 modules in `lib/modules/5.4-gki`
(clocks, interconnect, pinctrl, regulators, SMMU, UFS). They are not shipped:
they were built for a GKI kernel (`module_layout` CRC `0x1e5b7ab7`, while this
kernel and its vendor modules use `0x945d02f2`), so none of them loads, and all
of those drivers are built into this kernel. Stock first stage init loads zero
modules from that directory; with the modules in a flat `/lib/modules`, the
first failed module makes first stage init abort and the kernel panic.

## Updating

Replace every file from the same firmware release, never mix releases: the
modules only load into the kernel they were built with. Then update the
security patch level (`BOOT_SECURITY_PATCH`) and the build fingerprint in the
device tree to match that release.
