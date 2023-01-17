This directory holds a patch set that can be applied to
a v5.4.173 kernel tree, so as to support the Pensando Elba ASIC
based card; as of 01/16/2023.

## New code for Elba SoC support

**00_pciep_regrd32.patch**<br>
```
drivers/soc/pensando: refactor pciep_regrd32 for kpciemgr
Renamed pciep_regrd to pciep_regrd32 and modified to
take physical address and size as arguments.
```
**01_elba_kpcimgr.patch**<br>
```
drivers/soc/pensando: kpciemgr driver
kpciemgr driver enables the relocation of the module code to handle
Elba indirect PCIe transactions.
```
**02_emmc_hardware_reset.patch**<br>
```
drivers/reset: Add emmc hardware reset
Add multi-function driver with sub-device reset driver to
execute emmc hardware reset.
```
**03_elba_sbus.patch**<br>
```
drivers/soc/pensando sbus driver
sbus driver provides read/write capability to the devices
on the sbus rings using ioctl interface to /dev/sbus<n> device.
```
**04_kdump_bootcount.patch**<br>
```
drivers/soc/pensando: boot_count to sysfs for kdump.log
Add boot_count to sysfs. Augment the kdump timestamp with
the boot_count number. Also, adjust the kdump timestamp.
```
**05_i2c_lattice_rd1173_reset.patch**<br>
```
drivers/i2c: Reset Lattice RD1173 master for i2c_busy set.
Reset RD1173 when busy condition is detected even after transfer
completion timeout. Also, enable sysfs access to RD1173 master
registers and fix statistics as multiple cmdstat bits can be set.
```
