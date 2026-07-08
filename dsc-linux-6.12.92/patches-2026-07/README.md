This directory contains patches applying to a v6.12.92 kernel tree
(dsc-linux-6.12), to support the AMD Pensando Elba, Giglio and Salina ASIC
based cards.

## Commits
**0001-dt-bindings-add-AMD-Pensando-Elba-Giglio-and-Salina-.patch**<br>
```
dt-bindings: add AMD Pensando Elba, Giglio and Salina SoCs

Add devicetree binding documentation and shared dt-bindings headers for
the AMD Pensando Elba, Giglio and Salina SoCs and their on-chip service
blocks: capmem, PCIe kernel-assist, boot-state-machine, crash log,
reset-cause, sbus, penfw, kpcimgr, the CSR interrupt controller, EDAC,
ELBASR MFD/reset, the mnet network device, tawk-ipc and the CPLD RD1173
and SPI-multiplexer devices.  Add the "amd" vendor prefix and trivial
device entries used by these boards.

Also extend the existing Cadence QSPI/SDHCI and LTC2978 PMBus bindings
with the AMD Pensando compatibles used by these SoCs.
```
**0002-arm64-traps-report-AMD-Pensando-async-SError-as-SIGB.patch**<br>
```
arm64: traps: report AMD Pensando async SError as SIGBUS

On AMD Pensando SoCs an asynchronous SError triggered by a user-mode
access to a faulting memory region should be delivered to the task as
SIGBUS rather than panicking the kernel.  Add the hook used by the
Pensando platform to convert these aborts.
```
**0003-soc-pensando-add-AMD-Pensando-SoC-service-drivers.patch**<br>
```
soc: pensando: add AMD Pensando SoC service drivers

Add the AMD Pensando SoC service drivers used by the Elba, Giglio and
Salina SoCs: the capmem user-memory driver, PCIe kernel-assist (kpcimgr)
and pciemgr interface, boot-state-machine, crash/panic log, reset-cause,
sbus read/write, the penfw secure-boot interface and the CMN PMU and
Giglio eMMC interrupt helpers, with the drivers/soc Kconfig and Makefile
hooks.
```
**0004-irqchip-add-AMD-Pensando-SoC-interrupt-controller.patch**<br>
```
irqchip: add AMD Pensando SoC interrupt controller

Add the AMD Pensando SoC interrupt controller driver with hierarchical
CSR interrupt support, and take the GIC-v3 ITS MSI encapsulator address
from the devicetree on Pensando platforms.
```
**0005-EDAC-add-AMD-Pensando-Elba-and-Salina-memory-control.patch**<br>
```
EDAC: add AMD Pensando Elba and Salina memory controllers

Add EDAC drivers for the Elba and Salina DDR memory controllers,
reporting correctable and uncorrectable ECC errors.
```
**0006-mfd-add-AMD-Pensando-ELBASR-CPLD.patch**<br>
```
mfd: add AMD Pensando ELBASR CPLD

Add the ELBASR CPLD MFD driver and register definitions used to expose
the reset and RTC sub-functions on AMD Pensando boards.
```
**0007-reset-add-AMD-Pensando-ELBASR-reset-controller.patch**<br>
```
reset: add AMD Pensando ELBASR reset controller

Add the ELBASR reset controller used for eMMC and peripheral reset on
AMD Pensando Salina boards.
```
**0008-rtc-add-AMD-Pensando-ELBA-CPLD-RTC.patch**<br>
```
rtc: add AMD Pensando ELBA CPLD RTC

Add the RTC driver for the AMD Pensando ELBA CPLD real-time clock.
```
**0009-uio-add-AMD-Pensando-UIO-drivers.patch**<br>
```
uio: add AMD Pensando UIO drivers

Add the AMD Pensando UIO drivers: pengic generic IRQ, penmsi MSI,
linkmac uplink MAC status and pciemac, used to handle ASIC interrupts
and status decoding in userspace.
```
**0010-i2c-designware-add-AMD-Pensando-support.patch**<br>
```
i2c: designware: add AMD Pensando support

Add Pensando-specific support to the DesignWare I2C driver: stuck SDA
line recovery, a slave-mode FIFO configuration stub and dynamic
master/slave switching.
```
**0011-i2c-add-AMD-Pensando-CPLD-RD1173-SPI-to-I2C-driver.patch**<br>
```
i2c: add AMD Pensando CPLD RD1173 SPI-to-I2C driver

Add the Lattice RD1173 SPI-to-I2C bus driver used on AMD Pensando Elba
and Salina boards, with the i2c-busses Kconfig and Makefile hooks.
```
**0012-spi-cadence-quadspi-fix-indirect-write-timeout-on-AM.patch**<br>
```
spi: cadence-quadspi: fix indirect write timeout on AMD Pensando

Fix an indirect-write completion timeout observed on the Cadence QSPI
controller on AMD Pensando Salina SoC.  Refactor the driver so the Salina
indirect-write quirk only affects Salina.
```
**0013-spi-add-AMD-Pensando-CPLD-SPI-multiplexer-and-Salina.patch**<br>
```
spi: add AMD Pensando CPLD SPI multiplexer and Salina dw support

Add the AMD Pensando CPLD SPI multiplexer driver and enable the
DesignWare MMIO SPI controller on the Salina SoC, with the spi Kconfig
and Makefile hooks.
```
**0014-mmc-sdhci-cadence-add-AMD-Pensando-bounce-buffer-and.patch**<br>
```
mmc: sdhci-cadence: add AMD Pensando bounce buffer and HS400-ES support

Add ADMA bounce-buffer support, HS400 Enhanced Strobe and the Salina
variant to the Cadence SDHCI driver, and use HPI to interrupt a lengthy
eMMC cache flush in the MMC core.
```
**0015-hwmon-pmbus-add-LTC3888-and-TPS53659-support.patch**<br>
```
hwmon: pmbus: add LTC3888 and TPS53659 support

Add device support for the LTC3888 (ltc2978 driver) and TPS53659
(tps53679 driver) regulators used on AMD Pensando boards.
```
**0016-net-mctp-i2c-fix-dynamic-debug-logging.patch**<br>
```
net: mctp-i2c: fix dynamic debug logging

Add dynamic debug logging in the MCTP I2C transport.
```
**0017-perf-arm-cmn-enable-CMN-PMU-on-AMD-Pensando-Salina.patch**<br>
```
perf: arm-cmn: enable CMN PMU on AMD Pensando Salina

Enable the Arm CMN-700 PMU on the AMD Pensando Salina SoC.
```
**0018-arm64-dts-amd-add-Elba-Giglio-and-Deschutes-boards.patch**<br>
```
arm64: dts: amd: add Elba, Giglio and Deschutes boards

Add the devicetree sources for the AMD Pensando Elba and Giglio SoCs and
the Giglio-based Deschutes board, including PSCI and spin-table boot,
flash partitioning and secure-boot variants.
```
**0019-arm64-dts-amd-add-Salina-SoC-and-boards.patch**<br>
```
arm64: dts: amd: add Salina SoC and boards

Add the devicetree sources for the AMD Pensando Salina SoC and its A35,
N1 and HAPS board variants, including PSCI and spin-table boot, flash
partitioning, sensor and MCTP I2C buses and secure-boot variants.
```
**0020-arm64-amd-add-dtb-Makefile-entries-and-Pensando-defc.patch**<br>
```
arm64: amd: add dtb Makefile entries and Pensando defconfigs

Add the arch/arm64/boot/dts/amd Makefile dtb targets and the Elba,
Giglio and Salina defconfigs, including the gold and HAPS variants.
```
**0021-soc-pensando-bsm-add-read-only-access-to-A35-BSM-from-N1.patch**<br>
```
soc/pensando/bsm: add read-only access to A35 BSM from N1

Mark the A35 BSM (bsm_a35) as read-only in the device tree so that
N1 Linux can read A35 BSM fields but cannot modify them. A35 retains
sole ownership of its BSM state. The N1 BSM remains read-writable.
```
