This directory is a continuation of patches from patches-2020-10; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

## Commits imported directly from linux-stable
These commits come from the linux-stable git tree
git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git

**01_stable_b56f2a4_spi_dw.patch**<br>
**02_stable_2168f2a_spi_dw.patch**<br>
**03_stable_06a0145_spi_dw.patch**<br>
**04_stable_cec8990_spi_dw.patch**<br>
These patches fix various issues found in the Designware SPI driver.

**05_stable_fd8e4af_ipv4_sysctl.patch**<br>
This patch addresses CVE-2019-18805.

## Changes to existing code
The following diffs modify existing drivers.

**06_dw_spi_elba_chipselect.patch**<br>
This patch fixes access SPI chip-select 2 & 3 on Elba-based cards.
