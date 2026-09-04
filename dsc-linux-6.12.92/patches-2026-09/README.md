This directory is a continuation of patches from patches-2026-07; applying
to a v6.12.92 kernel tree, to support the AMD Pensando Elba, Giglio and
Salina ASIC based cards.

## Commits
**0001-modpost-restore-R_ARM-reloc-fallback-defines-for-old-host.patch**<br>
```
modpost: restore R_ARM_* reloc fallback defines for old host toolchain

The 6.9 modpost refactor dropped the #ifndef R_ARM_* fallbacks. Our
Buildroot host toolchain builds HOSTCC with -isystem host/include, whose
elf.h predates these ARM relocation constants, breaking the modpost host
build. Re-add the guarded defines (no-op where elf.h already defines them).
```
**0002-mtd-spi-nor-gigadevice-add-support-for-GD55LF02GF.patch**<br>
```
mtd: spi-nor: gigadevice: add support for GD55LF02GF

Add flash entry for GigaDevice GD55LF02GF (JEDEC ID 0xC8631C), a 2Gbit
(256MB) uniform-sector dual/quad SPI NOR flash.

The device has:
- 4096 x 64KB blocks with 4KB subsector erase on the top and bottom blocks
- Native 4-byte opcode support required for full address space access
- BP[4:0] + CMP block protection bits
- QE at SR2 bit 1 (S9), hardwired to 1 by default but managed via the
  standard sr2_bit1 quad_enable method
```
**0003-spi-cadence-quadspi-salina-disable-STIG-mode.patch**<br>
```
spi: cadence-quadspi: salina: disable STIG mode

Add CQSPI_DISABLE_STIG_MODE to the Salina QSPI platform data quirks.
```
**0004-arm64-configs-salina-gold-enable-MCTP-I2C-transport-and-I2C-designware-slave.patch**<br>
```
arm64: configs: salina_gold: enable MCTP I2C transport and I2C designware slave

- CONFIG_MCTP_TRANSPORT_I2C=y
- CONFIG_I2C_DESIGNWARE_CORE=y
- CONFIG_I2C_DESIGNWARE_SLAVE=y
```
**0005-arm64-dts-amd-ubootenv-and-a35-gold-uboot-flash-partition.patch**<br>
```
arm64: dts: amd: ubootenv and a35 gold uboot flash partition

Remove a35 prefix from a35 gold and main ubootenv partitions as they will
be shared by both A35 and N1. Also, fix gold uboot partition size.
```
