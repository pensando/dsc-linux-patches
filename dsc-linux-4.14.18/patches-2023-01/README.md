This directory is a continuation of patches from patches-2022-05; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

-----
**00_elba_sbus_driver.patch**<br>
The Elba SBUS driver provides access to non memory-mapped ASIC registers.

**01_lattice_i2c_update.patch**<br>
This patch rolls up a few advancements to the Lattice rd1173 driver:
- Reset lattice master for i2c_busy set
- Add sysfs access to lattice master registers
- Fix statistics as multiple cmdstat bits can be set
- Fix i2c_busy stuck port 1 affecting port 2

**02_winbond_flash.patch**<br>
This patch adds support for the 2Gbit Winbond w25q02nw flash,
which supports Dual I/O and 4-byte opcodes.

**03_cap_pcie.patch**<br>
Refactor the pciep_regrd32() api to make it easier to use from other
kernel drivers. The function can be used by kpcimgr to make controlled
accesses to pcie registers that might generate SErrors we want to ignore.
