This directory is a continuation of patches from patches-2023-01;
applying to a v5.4.173 kernel tree, to support the Pensando Elba ASIC.

**00_penfw.patch**<br>
```
drivers/soc/pensando: penfw driver
penfw driver provides sysfs and ioctl interface for the userspace
to communicate to the bl31 secure monitor
```
**01_elba_defconfig_cleanup.patch**<br>
```
defconfig: cleanup elba_defconfig
Remove CONFIG_MFD_SYSCON as that is automatically enabled when
MFD_PENSANDO_ELBASR is enabled.
```
**02_elba_psci.patch**<br>
```
arch/arm64/boot/dts: psci support
Change CPU enable-method from 'spin-table' to 'psci'.
```
**03_i2c_lattice_rd1173_intr.patch**<br>
```
drivers/i2c: Fix Lattice RD1173 interrupt handling
The rd1173 has two i2c masters whose interrupts are tied together.
Fill out in the master_xfer which i2c adapter the transfer is for
so the irq handler knows which master cmdstat to read.  Fixes a
stuck busy port 1 from affecting port 2.
```
