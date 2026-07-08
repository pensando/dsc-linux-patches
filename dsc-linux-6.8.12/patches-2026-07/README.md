This directory is a continuation of patches from patches-2026-05;
applying to a v6.8.12 kernel tree, to support the Pensando Salina,
Elba and Giglio ASIC based cards.

## Commits
**0001-arm64-dts-Add-missing-elba-giglio-DTS-files.patch**<br>
```
arm64/dts/amd: Add missing elba/giglio DTS files

Add PSCI, flash128, and secure DTS variants for Elba and Giglio SoCs.
Also refactors elba-16core.dtsi into separate PSCI and spin-table
includes, and adds flash128 partition layout support.
```
**0002-i2c-rd1173-formatting-fix-and-mctp-i2c-dynamic-debug.patch**<br>
```
drivers/i2c/rd1173: formatting and mctp-i2c dynamic debug logging fix

Fix code formatting in the rd1173 Lattice SPI-to-I2C bus driver and
fix dynamic debug logging in the mctp-i2c driver to use proper format
specifiers.
```
**0003-soc-pensando-bsm-add-A35-BSM-with-read-only-access.patch**<br>
```
soc/pensando/bsm: add A35 BSM with read-only access from N1

Add a second BSM (bsm_a35) for the A35 subsystem alongside the
existing N1 BSM. Refactor cap_bsm.c to support multiple instances,
iterating all pensando,bsm DT nodes. Expose both as separate sysfs
entries under /sys/firmware/pensando/.

The A35 BSM is read-only from N1 Linux -- N1 can read A35 BSM fields
but cannot modify it. A35 retains sole ownership of its BSM state.
The N1 BSM remains read-writable.
```
**0004-arm64-dts-amd-salina-fix-and-rename-pentrustfw-partition.patch**<br>
```
arm64/dts/amd/salina: fix and rename pentrustfw partition labels

Fix flash partition label for a35 pentrustfw1 and rename pentrustfw
partition labels for consistency.
```
**0005-drivers-edac-Add-Salina-EDAC-support.patch**<br>
```
drivers/edac: Add Salina EDAC support

Port Salina EDAC support from A35 to N1. The Salina EDAC driver is
similar to Elba and relies on device-tree nodes installed by u-boot.
```
