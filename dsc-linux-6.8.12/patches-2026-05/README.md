This directory contains patches applying to a v6.8 kernel tree (dsc-linux-6.8),
to support the AMD Pensando Salina ASIC based card.

## Commits
**0001-dts-Add-Deschutes-board-support-based-on-Giglio-496-.patch**<br>
```
dts: Add Deschutes board support based on Giglio (#496) (#510)

- arch/arm64/boot/dts/amd: add giglio-deschutes-flash128.dts and
  giglio-deschutes.dtsi for the Deschutes board variant based on Giglio
- drivers/hwmon/pmbus/tps53679: add tps53689 support
```
**0002-mmc-sdhci-cleanup-and-HS400-Enhanced-Strobe-for-Sali.patch**<br>
```
mmc: sdhci cleanup and HS400 Enhanced Strobe for Salina

- delete workaround sdhci-giglio.c (interrupt fixed via DTS)
- mmc: sdhci-cadence: Salina enable HS400 Enhanced Strobe
```
**0003-spi-cadence-quadspi-fix-cpld-spimux-driver-and-cpld-.patch**<br>
```
spi: cadence-quadspi fix, cpld-spimux driver, and cpld-spimux fix

- spi: cadence-quadspi: fix indirect write timeout on Pensando Salina N1
  (APB-AHB hazard)
- spi: add AMD Pensando Salina CPLD SPI multiplexer driver
- spi: cpld-spimux: Fix CPLD CSMAP write
```
**0004-i2c-designware-slave-FIFO-stub-and-dynamic-MCTP-mast.patch**<br>
```
i2c: designware: slave FIFO stub and dynamic MCTP master/slave switching

- i2c: designware: Stub slave FIFO config when slave mode disabled
- i2c: designware: add dynamic master/slave switching for MCTP
```
**0005-i2c-rd1173-Salina-support-reset-on-error-restore-int.patch**<br>
```
i2c: rd1173: Salina support, reset on error, restore interrupt clear

- i2c: rd1173: add Salina support
- i2c: rd1173: reset port on transfer error
- i2c: rd1173: Restore interrupt clear
```
**0006-arm64-dts-salina-PSCI-sensors-watchdog-QSPI-freq-I2C.patch**<br>
```
arm64: dts: salina: PSCI, sensors, watchdog, QSPI freq, I2C aliases, secure image

- arm64: dts: salina: enable sensor and MCTP I2C busses
- Added psci support for salina
- Change the QSPI freq to 40Mhz
- arm64: dts: salina: add INA3221 power monitor support
- arm64: dts: salina: add watchdog nodes
- arm64: dts: salina: disable unused i2c1 controller
- I2C aliases to preserve existing I2C bus numbering, for mainfw
- Create secure dts to create secure fit image
```
**0007-arm64-dts-amd-salina-add-eMMC-reset-support-578.patch**<br>
```
arm64: dts: amd: salina: add eMMC reset support (#578)

- arm64: dts: salina-asic-common: add eMMC reset bindings
- drivers/reset/reset-elbasr: add eMMC reset support
- drivers/mmc/host/sdhci-cadence: wire up eMMC reset
```
**0008-arm64-configs-salina-goldfw-defconfig-updates.patch**<br>
```
arm64: configs: salina/goldfw defconfig updates

- Porting Elba dm_crypt and goldfw size optimization to Leni
- Add missing configs to salina gold defconfig
- arm64: configs: salina: enable INA3221 power monitor
- Add mctp and designware slave to goldfw defconfig
- arm64: configs: salina: enable RESET_ELBASR for eMMC reset support
```
