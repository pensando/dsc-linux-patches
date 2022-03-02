This directory holds a patch set that can be applied to
a v4.14.18 kernel tree, so as to support the Pensando Elba ASIC
based card; as of 10/20/2020.

## Commits imported directly from kernel.org
These are pulled from the kernel.org master branch, and their commit sha is
present in the filename.

**00_kernel_org_fa150019_gicv3_its_1_3.patch**<br>
**01_kernel_org_9d111d49_gicv3_its_2_3.patch**<br>
**02_kernel_org_558b0165_gicv3_its_3_3.patch**<br>
The ARM GIC ITS is the Interrupt Translation Service that is used to
steer MSI interrupt writes to the correct IRQ number.  In the GICv3
architecture one of the required parameters into the translation is a
DeviceID, which represents the originating devices so as to provide
a distinct interrupt space.  In a system with PCIE peripherals the
DeviceID would come from the root complex.  In an SoC like Elba
where all the devices are "platform" devices there is no source of
DeviceID.

Pensando ASICs implement a "pre-ITS" scheme compatible with Socionext's
Synquacer SoC, and these commits pull in their upstream changes.

From the _02_kernel_org_558b0165_gicv3_its_3_3_ commit:
> The Socionext Synquacer SoC's implementation of GICv3 has a so-called
> 'pre-ITS', which maps 32-bit writes targeted at a separate window of
> size '4 << device_id_bits' onto writes to GITS_TRANSLATER with device
> ID taken from bits [device_id_bits + 1:2] of the window offset.

**03_kernel_org_70597eec_cadenec_qspi_arm64.patch**<br>
This commit makes the Cadence QuadSPI driver available on ARM64
(Kconfig change - previously ARM (32-bit) only).

**04_kernel_org_eb6a4dcc_isb_barrier_1_2.patch**<br>
**05_kernel_org_51696d34_isb_barrier_2_2.patch**<br>
These commits pull in some required instruction barriers (ISB) related
to PTE and TLB operations,

**06_kernel_org_56c6855c_micron_mt25qu02g.patch**<br>
Adds the SPI NOR definition of the Micron MT25QU02G QSPI, used on Pensando
cards.

**07_kernel_org_61dc8493_cadence_quadspi_wr_delay.patch**<br>
The Cadence QuadSPI controller uses two memory interfaces:
- An APB interface for register access
- An AHB interface for indirect FIFO and direct access

When performing an indirect write (i.e. flash program operation), a
GO bit is written to a controller register via the APB bus, followed
by data being written via the AHB bus.  A SoC-specific hazard may
be present if the APB and AHB transactions aren't serialized or take
different paths through the chip.

This commit implements quirk support in the driver, with the quirk
for the TI SoC requiring a time-based delay between the APB write
and first AHB write.

This is in preparation for _26_cadence_quadspi_apb_ahb_hazard_.

**08_kernel_org_2cc78838_cadence_quadspi_octal_support.patch**<br>
**09_kernel_org_d678d222_cadence_quadspi_typo.patch**<br>
The useful part of these commits is that they evolve the quirk
mechanism.  Pulled in to keep _26_cadence_quadspi_apb_ahb_hazard_
compatible with newer code.

**10_kernel_org_1bfe8889_dw_wdt_add_stop.patch**<br>
**11_kernel_org_a81abbb4_dw_wdt_rpl.patch**<br>
These patches support the firmware configuring the watchdog's
Reset Pulse Length by having the driver properly read/modify/write
the watchdog's configuration register.

**12_kernel_org_f02cebd_cadence_sdhci.patch**<br>
**13_kernel_org_ef6b756_cadence_sdhci.patch**<br>
**14_kernel_org_3f1109d_cadence_sdhci.patch**<br>
**15_kernel_org_159a8b4_cadence_sdhci.patch**<br>
**16_kernel_org_1d45a3f_cadence_sdhci.patch**<br>
**17_kernel_org_18b587b_cadence_sdhci.patch**<br>
**18_kernel_org_f6bc818_cadence_sdhci.patch**<br>
**19_cadence_sdhci_revert_159a8b4.patch**<br>
These patches advance the Cadence SDHCI driver, which is used by Elba.
The _159a8b4_ patch is a prerequisite for the later ones, but is
incompatible with 4.14.18, so patch 19 reverts it.

## Commits Based partly on upstream kernel.org
These commits use source from upstream where the original commit could
not be directly applied.

**20_kernel_org_rtc_pcf85363.patch**<br>
This commit imports 8f7b1d7:drivers/rtc/rtc-pcf85363.c from kernel.org,
but removes the nvram functions that aren't compatible with the
v4.14.18 kernel.

## New code for basic Elba SoC support
The following commits comprise additions for basic Elba SoC support.

**27_elba_initial_support.patch**<br>
This adds the platform Kconfig entries for the Pensando Elba SoC.

**28_elba_spi_gpio_chipselect.patch**<br>
This patch adds support for the Elba's GPIO-like SPI chip-selects.

**30_cap_mem.patch**<br>
In Pensando products the P4 dataplane is managed by userspace processes,
which directly mmap ASIC registers and dataplane memory.

The /dev/mem interface does not provide the access control, memory
address range control, and (related to 24_precise_pte_control)
attribute control.

The /dev/capmem driver allows us to eventually remove /dev/mem entirely
and perform access control.

**33_elba_config_dts.patch**<br>
This patch adds elba_defconfig and DTS files.

## Changes to existing code
The following diffs modify existing drivers.

**21_micron_macronix_qspi_devices.patch**<br>
mtd: spi-nor: Declare support for SPI_NOR_DUAL_READ on the Micron
MT25QU02G device, and add the declaration for the Macronix MX66U2G45G

**22_cadence_quadspi_spi_rx_bus_width_property.patch**<br>
mtd/spi-nor/cadence-quadspi.c: support spi-rx-bus-width property on
subnodes.

This commit allows the width of the bus between the controller and the
device to be specified in the device tree.

**23_hwmon_tps53659_driver.patch**<br>
hwmon/pmbus: Add a driver for the TI TPS53659, based on
Vadim Pasternak's TPS53679.c driver.

**24_i2c_designware_sda_recovery.patch**<br>
Add I2C code that attempts to recover from a stuck SDA line.

Note that it may not be possible to recover from a stuck SDA line and there
is no way to recover from a stuck SCL line. Be aware that the procedure
for SDA recovery involves a polling loop in interrupt mode. This should
last just long enough for transmission of 9 bits, after which the
hardware should indicate that the recovery attempt is complete. There
have been examples where this fails, so there is also a hard maximum on
time in the recovery loop.

Details on the I2C device are in the Synopsys DesignWare DW_apb_i2c
Databook.

**25_pensando_cpld_spidev.patch**<br>
This diff adds "pensando,cpld" as a compatible spidev.

This relates to userspace code being able to open /dev/spidev and
perform ioctls to access the on-board CPLD.

**26_cadence_quadspi_apb_ahb_hazard.patch**<br>
Re: _07_kernel_org_61dc8493_cadence_quadspi_wr_delay_

In Pensando ASICs the APB write to the Cadence controller may
be subject to different delays along the (multi-hop) APB chain.

The safest way to avoid the APB->AHB hazard is to bounce an extra
read off the APB port prior to performing the first AHB write.

This diff implements the Elba quirk to do that.

**29_elba_cadence_mmc.patch**<br>
This patch adds Elba-specific changes for the Cadence SDHCI controller.
The diff breaks out the Elba-specific changes into a new file,
sdhci-cadence-elba.c, requiring common definitions to be moved out
of sdhci-cadence.c into a new sdhci-cadence.h.


## Application-specific modifications
These changes are related to how Pensando uses the Elba SoC: How we
boot, how we crash, and other things that may not be appropriate to
try to upstream.

**31_cap_bsm.patch**<br>
The "Boot State Machine" is part of the Pensando boot flow that begins
pre-U-Boot, and allows the system to automatically recover from certain
types of failure such as early boot crashes and kernel panics.  This
driver allows the kernel to participate in the BSM and for its state
to be read out by userspace.

**32_cap_crash.patch**<br>
If the kernel panics, this driver memcpys the kernel message buffer
into some memory-mapped pre-erased flash pages.
Upon a panic, this simple memcpy removes any dependency on working
storage drivers.

In the future when we have kexec support we would launch a crash
kernel and take a more detailed dump, but until then this is a
fairly simple and reliable scheme.  It relies on userspace reading
the flash pages and then erasing them in preparation for a future
panic.

