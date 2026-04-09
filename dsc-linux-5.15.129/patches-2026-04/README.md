This directory is a continuation of patches from patches-2026-03; applying
to a v5.15.129 kernel tree, to support the Pensando Elba ASIC based card.

## Commits
**00_kpcimgr_dynamic_evq_support.patch**<br>
```
drivers/soc/pensando/kpcimgr: Add kpcimgr dyn-evq support

Enhanced pcie event infra using cache coherent memory region.
Support per lif quotas and missed event handling.
Events are directly consumed by nicmgr (pciemgr_if), log messages continue
to be sent to pcimgrd

Summary for changes:
- elba hnic dts contains index, size and memory address of region
  to be used for event ring
- Integration with pcie-evq service module to pass events to userspace
- Additional device file and epoll support for the new ring
```
**01_cap_mem_osync_support.patch**<br>
```
drivers/soc/pensando/cap_mem: O_SYNC supp on coherent region

Honor non-cachable mapping on cohernet region if capmem device was
opened in O_SYNC mode
```
**02_penfw_sysfs_bl31_show_ver.patch**<br>
```
drivers/soc/pensando/penfw_sysfs: fix bl31_show vers buffer size

The SMC call returns at most 4 registers of 8 bytes each (32 bytes).
Reduce the vers buffer from 256 to 33 bytes (32 data + null terminator) to
accurately reflect the actual maximum version string length.
```
**03_penfw_attest_meas_cert_chain_apis.patch**<br>
```
drivers/soc/pensando/penfw: Added attest_meas and get cert_chain to penfw_util

Added the Linux APIs for attest_meas and getting cert_chain.
```
**04_enable_dmcrypt.patch**<br>
```
arch/arm64/configs/: Enable dm_crypt for elba/giglio mainfw
```
**05_increase_uio_penmsi.patch**<br>
```
arch/arm64/boot/dts/pensando/elba: increase uio_penmsi irq to 32 for elba,giglio
```
