This directory holds a patch set that can be applied to
a v5.15.129 kernel tree, so as to support the Pensando Elba ASIC
based card; as of 9/7/2023.

## Commits
**00_hwmon_tps53659_driver.patch**<br>
```
hwmon/pmbus: Add support for the TI TPS53659
This patch adds support for the Texas Instruments TPS53659.
Datasheet: https://www.ti.com/lit/gpn/tps53659
```
**01_hwmon_ltc3888.patch**<br>
```
drivers/hwmon: Adding support LTC3888
This patch adds support for the LTC3888 VRM.
- Adding support for ltc3888 and ltc3888-1 driver
- Adding LTC3888 IOUT support.
Datasheet: https://www.analog.com/media/en/technical-documentation/data-sheets/ltc3888-3888-1-3888-2.pdf
```
**02_i2c_designware_stuck_bus_recovery.patch**<br>
```
i2c-designware: Support stuck SDA line recovery.
This patch supports the Designware I2C stuck bus recovery feature.
The procedure for stuck SDA recovery involves a polling loop in interrupt
mode. This should last just long enough for transmission of 9 bits,
after which the hardware should indicate that the recovery attempt is
complete. There have been examples where this fails, so there is also
a hard maximum on time in the recovery loop.
```
**03_i2c_lattice_rd1173.patch**<br>
```
i2c: Add Lattice RD1173 I2C controller driver.
The Lattice RD1173 is a pair of I2C controllers accesed via a SPI
interface.
```
**04_arm64_traps_bad_mode_platform.patch**<br>
```
arm64/traps: Call platform handler for do_serror
When taking a bad_mode exception, give platform code an opportunity
to decide whether the error the event can be ignored or not.
```
**05_mtd_cadence_quadspi_elba_quirks.patch**<br>
```
drivers/spi/spi-cadence-quadspi.c: add quirks for the
 Pensando controller
The Pensando SoC QSPI controller has a hazard between writes to the APB
interface and writes to AHB that affects indirect writes.  A dummy
APB register read is required between the APB write that starts an
indirect write operation, and the first AHB write of data.
We also want to disable DAC mode, for robustness.
```
**06_mmc_cadence_elba_support.patch**<br>
```
drivers/mmc/host: Pensando Elba support in the Cadence EMMC
 host controller
This patch adds Elba SoC support to the Cadence EMMC controller.
The Elba SoC has particular accessor requirements for non-word-size
registers.
```
**07_spi_dw_elba_chipselect.patch**<br>
```
spi-dw: Support Pensando Elba custom chip-select
Use a custom chip-select handler for Elba to ensure that a valid
Designware intrinsic chip-select is still activated even when using GPIOs.
Specify "pensando,elba-spi" in the device-tree to pick up this change.
```
**08_winbond_w25q02nw.patch**<br>
```
drivers/mtd/spi-nor: Winbond w25q02nw flash support.
Added Winbond w25q02nw flash support.
Product page: https://www.winbond.com/hq/product/code-storage-flash-memory/serial-nor-flash/?__locale=en&partNo=W25Q02NW_DTR
```
**09_elba_initial.patch**<br>
```
arch/arm64: Initial support for the Pensando Elba SoC
This patch adds initial support for Elba that allows a kernel to boot,
along with an initial device-tree and elba_defconfig that is compatible
with common Pensando PCI cards.
```
**10_emmc_hardware_reset.patch**<br>
```
drivers/reset: Add emmc hardware reset
Add multi-function driver with sub-device reset driver to
execute emmc hardware reset.
```
**11_elba_flash_parts.patch**<br>
```
dts/pensnado: Elba flash partitions
Pensando Elba-based PCIE cards have a common flash layout.
This patch adds the partitions to the dts files.
```
**12_elba_edac.patch**<br>
```
drivers/edac: Add Elba EDAC support
The edac driver reports correctable and uncorrectable errors from the
DDR controller.  The driver relies on device-tree nodes installed
by u-boot to convey:
- The active DDR channels.
- The NOC DDR hashes and bypass regions.
The active DDR channels tell the driver which channels to monitor.
The hashes allow the driver to reverse the memory address hashing so
that the reports can provide a real system address for the fault.
```
**13_elba_capmem.patch**<br>
```
drivers/soc/pensando: /dev/capmem driver.
The capmem driver provides an controlled interface for applications
to map memory (in preference to /dev/mem).
```
**14_elba_irqchip.patch**<br>
```
Interrupt domain controllers for Elba ASIC.
This supports using ASIC registers as IRQ chips, one per domain. The
result is a many level interrupt controller. There are actually three
different register blocks, each of which has its own configuration of
enable and disable mechanisms.
```
**15_elba_uio.patch**<br>
```
drivers/uio: UIO drivers for Elba
Support userspace interrupt drivers for Elba. PCIE events.
```
**16_elba_bsm.patch**<br>
```
drivers/pensando/soc: Boot State Machine (BSM) integration.
The Pensando BSM interacts with the boot firmware to provide a level
of recovery should a Linux image fail to start.  If the BSM is
armed and a kernel panic + reset occur, then the boot firmware
will consider the loaded image as bad and will select the alternate
image to boot next.
```
**17_elba_crash.patch**<br>
```
drivers/soc/pensando: crash dump driver.
The crash dump driver writes the kernel message log to
memory-mapped qspi flash.
```
**18_elba_rstcause.patch**<br>
```
drivers/soc/pensando: Add the Reset Cause driver
The reset cause driver exposes sysfs nodes under
/sys/pensando/firmware/rstcause that report the reason for the last
reboot and, for software affecting a new reboot, a file in which to
write why the next reboot is occurring.  This is used by the Pensando
platform userspace software.
```
**19_elba_penpcie.patch**<br>
```
soc/pensando: pcie driver
pcie driver provides these services on elba
1) panic blink handler runs after panic, restart when host restarts
2) sysfs panic_reboot node to restart immediately on panic
3) /dev/penpcie controlled access to pcie clock domain registers
```
**20_elba_kpcimgr.patch**<br>
```
drivers/soc/pensando: kpcimgr driver.
kpcimgr driver enables the relocation of the module code to handle
Elba indirect PCIe transactions.
```
**21_elba_mnicdts.patch**<br>
```
dts/pensando: add mnet and mcrypt devices, with reserved dma
 memory
This patch adds the mnet and mcrypt devices to the device-tree, along
with a reserved memory region for use by the mnet instances.  This avoids
memory allocation failure due to wider system memory fragmentation.
```
**22_elba_sbus.patch**<br>
```
drivers/soc/pensando sbus driver
sbus driver provides read/write capability to the devices
on the sbus rings using ioctl interface to /dev/sbus<n> device.
```
**23_kdump_bootcount.patch**<br>
```
drivers/soc/pensando: boot_count to sysfs for kdump.log
Add boot_count to sysfs. Augment the kdump timestamp with
the boot_count number. Also, adjust the kdump timestamp.
```
**24_elba_psci.patch**<br>
```
arch/arm64/boot/dts: psci support
Change CPU enable-method from 'spin-table' to 'psci'.
```
**25_penfw.patch**<br>
```
drivers/soc/pensando: penfw driver
penfw driver provides sysfs and ioctl interface for the userspace
to communicate to the bl31 secure monitor
This driver is only used with Secure Boot.
```
**26_defconfig_ipv6_netfilter.patch**<br>
```
arm64/configs: Add CONFIG_IP6_NF_IPTABLES for elba
This commit adds IPV6 netfilter support to the elba_defconfig file.
```
