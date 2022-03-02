This directory holds a patch set that can be applied to
a v5.4.173 kernel tree, so as to support the Pensando Elba ASIC
based card; as of 2/25/2022.

## Commits imported directly from kernel.org
**00_kernel_org_1d45a3f_cadence_sdhci.patch**<br>
```
mmc: sdhci-cadence: remove unneeded 'inline' marker
'static inline' in .c files does not make much sense because
functions may or may not be inlined irrespective of the 'inline'
marker. It is just a hint.
This function is quite small, so very likely to be inlined by the
compiler's optimization (-O2 or -Os), but it is up to the compiler
after all.
```
**01_kernel_org_f6bc818_cadence_sdhci.patch**<br>
```
mmc: sdhci-cadence: fix PHY write
Accordingly to Cadence documentation, PHY write procedure is:
1. Software sets the PHY Register Address (HRS04[5:0]) and the
   PHY Write Data (HRS04[15:8]) fields.
2. Software sets the PHY Write Transaction Request (HRS04[24]) field to 1.
3. Software waits as the PHY Write Transaction Acknowledge (HRS04[26])
   field is equal to 0.
4. Hardware performs the write transaction to PHY register where
   HRS04[15:8] is a data written to register under HRS04[5:0] address.
5. Hardware sets the PHY Transaction Acknowledge (HRS04[26]) to 1 when
   transaction is completed.
6. Software clears the PHY Write Transaction Request (HRS04[24]) to 1
   after noticing that the PHY Write Transaction Acknowledge (HRS04[26])
   field is equal to 1.
7. Software waits for the PHY Acknowledge Register (HRS04[26]) field is
   equal to 0.
Add missing steps 3 and 7. Lack of these steps causes
integrity errors detested by hardware.
```
**02_kernel_org_a9970507_cadence_quadspi_dac_disable.patch**<br>
```
mtd: spi-nor: cadence-quadspi: Provide a way to disable DAC
 mode
Currently direct access mode is used on platforms that have AHB window
(memory mapped window) larger than flash size. This feature is limited
to TI platforms as non TI platforms have < 1MB of AHB window.
Therefore introduce a driver quirk to disable DAC mode and set it for
non TI compatibles. This is in preparation to move to spi-mem framework
where flash geometry cannot be known.
```
**03_kernel_org_766c6b63_spi_gpio_cs.patch**<br>
```
spi: fix client driver breakages when using GPIO descriptors
[Based on 766c6b63aa044e84b045803b40b14754d69a2a1d]
Commit f3186dd87669 ("spi: Optionally use GPIO descriptors for CS GPIOs")
introduced the optional use of GPIO descriptors for chip selects.
A side-effect of this change: when a SPI bus uses GPIO descriptors,
all its client devices have SPI_CS_HIGH set in spi->mode. This flag is
required for the SPI bus to operate correctly.
This unfortunately breaks many client drivers, which use the following
pattern to configure their underlying SPI bus:
static int client_device_probe(struct spi_device *spi)
{
	...
	spi->mode = SPI_MODE_0;
	spi->bits_per_word = 8;
	err = spi_setup(spi);
	..
}
In short, many client drivers overwrite the SPI_CS_HIGH bit in
spi->mode, and break the underlying SPI bus driver.
This is especially true for Freescale/NXP imx ecspi, where large
numbers of spi client drivers now no longer work.
Proposed fix:
```
## Changes to existing code
**04_spi_nor_micron_macronix_qspi_devices.patch**<br>
```
mtd: spi-nor: add mx66u51235f and mx66u2g45g devices.
Declare support for SPI_NOR_DUAL_READ on the Micron
MT25QU02G device, and add the declaration for the Macronix MX66U2G45G
```
**05_mtd_cadence_quadspi_spi_rx_bus_width_property.patch**<br>
```
mtd/spi-nor/cadence-quadspi.c: support spi-rx-bus-width
 property on subnodes.
This commit allows the width of the bus between the controller and the
device to be specified in the device tree.
```
**06_hwmon_tps53659_driver.patch**<br>
```
hwmon/pmbus: Add a driver for the TI TPS53659, based on Vadim
 Pasternak's TPS53679.c driver.
```
**07_hwmon_ltc3888.patch**<br>
```
drivers/hwmon: Adding support LTC3888
This patch adds support for the LTC3888 VRM.
- Adding support for ltc3888 and ltc3888-1 driver
- Adding LTC3888 IOUT support.
```
**08_i2c_designware_stuck_bus_recovery.patch**<br>
```
i2c-designware: Add I2C code that attempts to recover from a
 stuck SDA line.
This patch supports the Designware I2C stuck bus recovery feature.
The procedure for stuck SDA recovery involves a polling loop in interrupt
mode. This should last just long enough for transmission of 9 bits,
after which the hardware should indicate that the recovery attempt is
complete. There have been examples where this fails, so there is also
a hard maximum on time in the recovery loop.
```
**09_arm64_traps_bad_mode_platform.patch**<br>
```
arm64/traps: Call platform handler for bad_mode
When taking a bad_mode exception, give platform code an opportunity
to decide whether the error the event can be ignored or not.
```
## New code for Elba SoC support
**10_mtd_cadence_quadspi_elba_quirks.patch**<br>
```
mtd/spi-nor/cadence-quadspi.c: add quirks for the Pensando
 controller
The Pensando SoC QSPI controller has a hazard between writes to the APB
interface and writes to AHB that affects indirect writes.  A dummy
APB register read is required between the APB write that starts an
indirect write operation, and the first AHB write of data.
We also want to disable DAC mode, for robustness.
```
**11_mmc_cadence_elba_support.patch**<br>
```
drivers/mmc/host: Pensando Elba support in the Cadence EMMC
 host controller
This patch adds Elba SoC support to the Cadence EMMC controller.
The Elba SoC has particular accessor requirements for non-word-size
registers.
```
**12_spi_dw_elba_chipselect.patch**<br>
```
spi-dw: Support Pensando Elba custom chip-select
Use a custom chip-select handler for Elba to ensure that a valid
Designware intrinsic chip-select is still activated even when using GPIOs.
Specify "pensando,elba-spi" in the device-tree to pick up this change.
```
**13_spi_spidev_elba_cpld.patch**<br>
```
drivers/spi/spidev.c: Add pensando,cpld device tree compat
 entry
The Pensando ASICs uses a spidev interface to access the board
CPLD.  This commit adds support for the entry in spidev.c
```
**14_elba_initial.patch**<br>
```
arch/arm64: Initial support for the Pensando Elba SoC
This patch adds initial support for Elba that allows a kernel to boot,
along with an initial device-tree and elba_defconfig that is compatible
with common Pensando PCI cards.
```
**15_elba_flash_parts.patch**<br>
```
dts/pensnado: Elba flash partitions
Pensando Elba-based PCIE cards have a common flash layout.
This patch adds the partitions to the dts files.
```
**16_elba_edac.patch**<br>
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
**17_elba_capmem.patch**<br>
```
drivers/soc/pensando: /dev/capmem driver.
The capmem driver provides an controlled interface for applications
to map memory (in preference to /dev/mem).
```
**18_elba_irqchip.patch**<br>
```
Interrupt domain controllers for Elba ASIC.
This supports using ASIC registers as IRQ chips, one per domain. The
result is a many level interrupt controller. There are actually three
different register blocks, each of which has its own configuration of
enable and disable mechanisms.
```
**19_i2c_lattice_rd1173.patch**<br>
```
i2c: Add Lattice RD1173 I2C controller driver.
The Lattice RD1173 is a pair of I2C controllers accesed via a SPI interface.
```
**20_elba_uio.patch**<br>
```
drivers/uio: UIO drivers for Elba
Support userspace interrupt drivers for Elba. PCIE events.
```
**21_elba_bsm.patch**<br>
```
drivers/pensando/soc: Boot State Machine (BSM) integration.
The Pensando BSM interacts with the boot firmware to provide a level
of recovery should a Linux image fail to start.  If the BSM is
armed and a kernel panic + reset occur, then the boot firmware
will consider the loaded image as bad and will select the alternate
image to boot next.
```
**22_elba_crash.patch**<br>
```
drivers/soc/pensando: crash dump driver.
The crash dump driver writes the kernel message log to
memory-mapped qspi flash.
```
**23_elba_rstcause.patch**<br>
```
drivers/soc/pensando: Add the Reset Cause driver
The reset cause driver exposes sysfs nodes under
/sys/pensando/firmware/rstcause that report the reason for the last
reboot and, for software affecting a new reboot, a file in which to
write why the next reboot is occurring.  This is used by the Pensando
platform userspace software.
```
**24_elba_penpcie.patch**<br>
```
soc/pensando: pcie driver
pcie driver provides these services on elba
1) panic blink handler runs after panic, restart when host restarts
2) sysfs panic_reboot node to restart immediately on panic
3) /dev/penpcie controlled access to pcie clock domain registers
```
**25_elba_mnicdts.patch**<br>
```
dts/pensando: add mnet and mcrypt devices, with reserved dma
 memory
This patch adds the mnet and mcrypt devices to the device-tree, along
with a reserved memory region for use by the mnet instances.  This avoids
memory allocation failure due to wider system memory fragmentation.
```
