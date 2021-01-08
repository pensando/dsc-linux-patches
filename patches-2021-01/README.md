This directory is a continuation of patches from patches-2020-11; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**00_elba_defconfig_loglevel.patch**<br>
This change sets the CONFIG_CONSOLE_LOGLEVEL_DEFAULT to 4, to stop the
kernel messages coming out of the console at boot time.

**01_elba_emmc_ver_300_cleanup.patch**<br>
Report a version 3.00 SDHCI controller (the limit of what
the 4.14.18 kernel supports) to avoid the warning at start-up:
    mmc0: Unknown controller version (3). You may experience problems.

Remove some unreachable code that was relevant for an earlier
version of the IP.  Write the APB byte-enables before phy_config()
to ensure the writes go through correctly if the function is called
without a chip reset (e.g. through kexec()).

**02_elba_defconfig_ipv6.patch**<br>
Add IPv6 support to Elba via the elba_defconfig file.

**03_elba_dts_mnet.patch**<br>
Add one more mnet device to the Elba device-tree.

**04_elba_lattice_rd1173_i2c.patch**<br>
Elba Ortano cards include a Lattice RD1173 I2C controller pair
in the SPI-conneccted CPLD.  This commit adds a driver for this
controller, and adds the Kconfig, defconfig, and device-tree
changes for Elba.

**05_sched_cve2018_20784.patch**<br>
This commit applies upstream c40f7d74, modified for 4.14.18 in order to address
CVE-2018-20784.

