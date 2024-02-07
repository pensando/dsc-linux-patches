This directory is a continuation of patches from patches-2023-09; applying
to a v5.15.129 kernel tree, to support the Pensando Elba ASIC based card.

## Commits
**00-mmc-bounce-buffer.patch**<br>
```
mmc: sdhci-cadence: Enable host driver defined bounce buffer

Elba SoC dt property pensando,bounce-buffer enables bounce
buffers in ADMA mode.  In this mode the host controller
memory access is exclusively in a bypass memory region.
Userspace IO data from coherent memory is copied to/from
the bypass region.
```
**01-edac-bypass.patch**<br>
```
drivers/edac: elba: Support multiple DDR bypass ranges.

This commit supports picking up multiple bypass DDR ranges from
the device-tree.
```
**02-capmem-dm.patch**<br>
```
drivers/soc/pensando/cap_mem.c: Support DM region mapping.

Migrate the standard device memory ranges out of cap_mem.c and
into the ASIC-specific dts file.  This gives us ASIC-specific
exports, without ugly #ifdefs in the driver.

Support a CAPMEM_GET_RANGES2 ioctl that provides a disctinct type
enumeration for bypass regions.  This gives userspace the capability
to find the bypass region programatically.
```
