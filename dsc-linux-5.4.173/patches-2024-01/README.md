This directory is a continuation of patches from patches-2023-10; applying
to a v5.4.173 kernel tree, to support the Pensando Elba ASIC.

**00-mmc-bounce-buffer.patch**<br>
```
mmc: sdhci-cadence: Enable host driver defined bounce buffer

Elba SoC dt property pensando,bounce-buffer enables bounce
buffers in ADMA mode.  In this mode the host controller
memory access is exclusively in a bypass memory region.
Userspace IO data from coherent memory is copied to/from
the bypass region.
```
