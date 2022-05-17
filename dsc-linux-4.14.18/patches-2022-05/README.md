This directory is a continuation of patches from patches-2022-01; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**0000_emmc_hwreset.patch**<br>
Add support for Pensando Elba System Resource Chip for userspace access
to the device and hardware reset of the eMMC.  The System Resource Chip
is accessed over a SPI bus using regmap framework.
