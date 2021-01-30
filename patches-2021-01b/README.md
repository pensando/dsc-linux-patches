This directory is a continuation of patches from patches-2021-01; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**00_elba_defconfig_rcu_nohz.patch**<br>
This patch enables rcu callback offloading & adaptive tick mode to
reduce jitter.

**01_elba_mmc_sdhci_adma_clenaup.patch**<br>
This patch adds support for HS200 ADMA and auto-tuning.  It also fixes
a race condition in the register accessors.

**02_arm64_platform_serror.patch**<br>
This patch allows a platform driver (pcie in our case) to handle an
SError interrupt in cases where one might be expected.  This is required
in the kernel code at the moment, as we do not have an EL3 monitor to
catch these.

**03_elba_pcie_driver.patch**<br>
This patch adds the Penando PCIE driver, which performs host-facing
link maintenance, SError handling, and reboot hooks.

**04_elba_defconfig_ptp.patch**<br>
This patch enables PTP (IEEE1588) support.

**05_elba_platform_irqchip.patch**<br>
This patch supports ASIC internal interrupts so they can be accessed
by userspsace daemons via uio.
