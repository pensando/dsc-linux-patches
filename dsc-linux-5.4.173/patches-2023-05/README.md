This directory is a continuation of patches from patches-2023-03; applying
to a v5.4.173 kernel tree, to support the Pensando Elba ASIC.

**00_elba_emmc_dtsi.patch**<br>
```
elba.dtsi: Improved sdclk and sdclk-hsmmc timing.

This patch sets the Elba EMMC standard and high-speed delays to zero,
to improve setup timing.
```
