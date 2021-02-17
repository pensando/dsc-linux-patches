This directory is a continuation of patches from patches-2021-01b; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**00_elba_mmc_sdhci_remove_quirk.patch**<br>
This patch removes the SDHCI_QUIRK_BROKEN_TIMEOUT_VAL quirk from the
EMMC driver.  This quirk is no longer needed with the latest u-boot.

**01_elba_capmem_hugepage.patch**<br>
This patch adds hugepage support to /dev/capmem mappings.  This can
greatly reduce the number of TLB misses when running P4 dataplane
maintenance code on the ARM.
