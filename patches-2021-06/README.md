This directory is a continuation of patches from patches-2021-05; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**00_add_brdcfg_mtd_partitions.patch**<br>
This patch adds mtd partitions for the board_config utility.

**01_fix_uio_pm_enable.patch**<br>
This patch fixes the uio/penmsi driver's use of the pm_runtime_enable()
functions, which were being called in an incorrect sequence.
