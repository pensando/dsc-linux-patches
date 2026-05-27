This directory holds a patch set that can be applied to
a v6.8.12 kernel tree, so as to support the Pensando Salina
ASIC based card; as of 4/2/2025.

## Commits
**01_mmc-sdhci-cadence-Support-ADMA-with-bounce-buffers.patch**<br>
```
Add bounce buffer support for AMD Pensando Elba, Giglio,
and Salina eMMC performance improvement.

When the property pensando,bounce-buffer is defined the
pre-allocated memory region is used for all DMA by the
host controller with descriptors/buffers bounced from/to
the original DDR address.
```
**02_hwmon-pmbus-ltc2978-Add-support-for-ltc3888.patch**<br>
```
```
**03_hwmon-pmbus-tps53679-Add-support-for-tps53659.patch**<br>
```
```
**04_i2c-designware-Support-stuck-SDA-line-recovery.patch**<br>
```
This patch supports the Designware I2C stuck bus recovery feature.
The procedure for stuck SDA recovery involves a polling loop in interrupt
mode.  This should last just long enough for transmission of 9 bits,
after which the hardware should indicate that the recovery attempt is
complete.  There have been examples where this fails, so there is also
a hard maximum on time in the recovery loop.
```
**05_i2c-rd1173-Add-Lattice-SPI-to-I2C-bus-driver.patch**<br>
```
The Lattice dual i2c master IP is in a system CPLD
attached to the SoC over a SPI interface
```
**06_arm64-dts-Add-AMD-Pensando-SoC-support.patch**<br>
```
Includes Elba, Giglio and Salina SoCs
```
**07_soc-pensando-Add-AMD-Pensando-SoC-drivers.patch**<br>
```
Support for Elba, Giglio and Salina SoC based boards
```
**08_arm64-defconfig-Add-AMD-Pensando-defconfig.patch**<br>
```
Includes Elba, Giglio and Salina SoC board defconfig
```
**09_arm64-Add-AMD-Pensando-UIO-support.patch**<br>
```
Supports Elba, Giglio and Salina SoC boards
```
**10_irqchip-pensando-Add-AMD-Pensando-IRQ-driver.patch**<br>
```
Supports Elba, Giglio and Salina SoC boards
```
**11_mtd-spi-nor-winbond-Add-support-for-W25Q02NW.patch**<br>
```
```
**12_EDAC-elba-Add-AMD-Pensando-Elba-Giglio-SoC-EDAC-driv.patch**<br>
```
```
**13_mfd-Add-AMD-Pensando-system-controller.patch**<br>
```
Support Pensando CPLD system controller with supporting
eMMC reset controller for Elba and Giglio boards

Support Salina SoC N1 eMMC reset controller with
userspace IPC to the Salina SoC on-chip A35 processor
which controls the SPI interface to the CPLD
```
**14_rtc-elbacpld-Support-RTC-in-AMD-Pensando-system-cont.patch**<br>
```
The RTC is in the board system controller CPLD connected over
SPI to AMD Pensando Elba, Giglio or Salina SoCs
```
**15_spi-dw-mmio-Add-AMD-Pensando-Salina-SoC-support.patch**<br>
```
The Salina SoC is identical to Elba except for a different
spics control register offset
```
**16_arm64-Enable-platform-SError-handler.patch**<br>
```
```
**17_perf-arm-cmn-Enable-AMD-Pensando-Salina-SoC-CMN-PMU-.patch**<br>
```
Salina SoC does not have a direct interrupt wired to GIC.  This
requires use of the PRP interrupt chain to make the interrupt work.

The Giglio SoC similarly requires use of PRP interrupt chain
for eMMC interrupt handling.
```
