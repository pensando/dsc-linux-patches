This directory holds a patch list that can be applied, in numeric order, to
a v4.14.18 kernel tree, bringing it up to date with the Pensando
master tree as of 4/7/2020.

## Commits imported directly from kernel.org
These are pulled from the kernel.org master branch, and their commit sha is
present in the filename.

The files were generated using 'git show SHA > filename'

**00_kernel_org_fa150019_gicv3_its_1_3.patch**<br>
**01_kernel_org_9d111d49_gicv3_its_2_3.patch**<br>
**02_kernel_org_558b0165_gicv3_its_3_3.patch**<br>
The ARM GIC ITS is the Interrupt Translation Service that is used to
steer MSI interrupt writes to the correct IRQ number.  In the GICv3
architecture one of the required parameters into the translation is a
DeviceID, which represents the originating devices so as to provide
a distinct interrupt space.  In a system with PCIE peripherals the
DeviceID would come from the root complex.  In an SoC like Capri
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

This is in preparation for _18_capri_cadence_quadspi_apb_ahb_hazard_.

**08_kernel_org_2cc78838_cadence_quadspi_octal_support.patch**<br>
**09_kernel_org_d678d222_cadence_quadspi_typo.patch**<br>
The useful part of these commits is that they evolve the quirk
mechanism.  Pulled in to keep _18_capri_cadence_quadspi_apb_ahb_hazard_
compatible with newer code.

## Commits Based partly on upstream kernel.org
These commits use source from upstream where the original commit could
not be directly applied.

**15_kernel_org_rtc_pcf85363.patch**<br>
This commit imports 8f7b1d7:drivers/rtc/rtc-pcf85363.c from kernel.org,
but removes the nvram functions that aren't compatible with the
v4.14.18 kernel.

## New code for basic Capri SoC support
The following commits comprise additions for basic Capri SoC support.

**19_capri_initial_support.patch**<br>
This adds the platform Kconfig entries for the Pensando Capri SoC
and an initial device-tree description of the ASIC.

**20_capri_emmc_support.patch**<br>
This adds the Capri EMMC phy support and adds the Arasan EMMC
controller to the ASIC device tree.

**21_capi_spi_driver.patch**<br>
This pulls in the driver for Capri's SPI controller, which is based
on the Designware SPI controller, but due to ASIC errata must be
used in a non-standard way.

**22_capri_irq_chip.patch**<br>
This diff implements Capri's internal interrupt hierarchy.

**23_capri_uio.patch**<br>
This code adds UIO interrupt support for interrupts related
to the host-facing PCIE (slave) controller Ethernet uplinks.

## Changes to existing code
The following diffs modify existing drivers.

**10_micron_macronix_qspi_devices.patch**<br>
mtd: spi-nor: Declare support for SPI_NOR_DUAL_READ on the Micron
MT25QU02G device, and add the declaration for the Macronix MX66U2G45G

**11_cadence_quadspi_spi_rx_bus_width_property.patch**<br>
mtd/spi-nor/cadence-quadspi.c: support spi-rx-bus-width property on
subnodes.

This commit allows the width of the bus between the controller and the
device to be specified in the device tree.

**12_hwmon_tps53659_driver.patch**<br>
hwmon/pmbus: Add a driver for the TI TPS53659, based on
Vadim Pasternak's TPS53679.c driver.

**13_i2c_designware_sda_recovery.patch**<br>
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

**14_wdt_rpl.patch**<br>
watchdog/dw_wdt.c: Support the reset pulse width from the
device-tree.

The Designware WDT supports a programmable reset pulse width.
The new devicetree property "snps,reset-pulse-len" allows this to
be specified in units of APB clock cycles.

Note: The upstream driver ensures that writes to the control register
preserves the reset-pulse setting, and leaves it up to pre-OS firmware
to pre-set the value.  We'll look at using that scheme in the future
in favor of this patch.

**16_softdog_panic.patch**<br>
This is a trivial diff that makes the default soft watchdog panic
behavior definable via a KConfig.

**17_pensando_cpld_spidev.patch**<br>
This diff adds "pensando,cpld" as a compatible spidev.

This relates to userspace code being able to open /dev/spidev and
perform ioctls to access the on-board CPLD.

**18_capri_cadence_quadspi_apb_ahb_hazard.patch**<br>
Re: _07_kernel_org_61dc8493_cadence_quadspi_wr_delay_

In Pensando ASICs the APB write to the Cadence controller may
be subject to different delays along the (multi-hop) APB chain.

The safest way to avoid the APB->AHB hazard is to bounce an extra
read off the APB port prior to performing the first AHB write.

This diff implements the Capri quirk to do that.

## Changes to common kernel code
These diffs apply to common kernel code that are required
for Capri and how we use it in our products.

**24_precise_pte_control.patch**<br>
In ARM64 Linux all normal memory is mapped as inner shareable.

In Capri we partition the physical memory of the device:
- Some memory we give to Linux for its purposes
- Some memory we give to the hardware P4 dataplane

Some of the P4 memory is I/O coherent with Linux and some is not.
The patch allows us to map cacheable memory as non-shareable.

There may be a way to avoid the need for this patch in the future,
and it's something we're looking at.

**25_arm_no_outer_alloc.patch**<br>
The Level 3 memory cache in the Pensando Capri SoC is used
by the hardware data path cache objects and table data.  Forwarding
performance can be increased by keeping the ARM cores out of L3.
    
This commit adds the _ARM64_MMU_DISABLE_OUTER_CACHE_ALLOCATE_ Kconfig
option which changes how reads and writes are signaled on the
core-external AXI interface.

**34_capri_apb_errata_workaround.patch**<br>
This diff applies to all APB peripheral drivers (qspi, uart, i2c, wdt, spi,
gpio), redirecting their readl() and writel() calls through code to work around
a Capri errata.

## Application-specific drivers
These changes implement DSC product features.

**26_capri_pcie_reset.patch**<br>
If the kernel panics then the firmware may want to delay resetting
the ASIC until the x86 host itself reboots.  This driver waits for
the PCIE event indicating host reboot before resetting the ASIC.

**27_crash_dump.patch**<br>
If the kernel panics, this driver memcpys the kernel message buffer
into some memory-mapped pre-erased flash pages.
Upon a panic, this simple memcpy removes any dependency on working
storage drivers.

In the future when we have kexec support we would launch a crash
kernel and take a more detailed dump, but until then this is a
fairly simple and reliable scheme.  It relies on userspace reading
the flash pages and then erasing them in preparation for a future
panic.

**28_bsm.patch**<br>
The "Boot State Machine" is part of the Pensando boot flow that begins
pre-U-Boot, and allows the system to automatically recover from certain
types of failure such as early boot crashes and kernel panics.  This
driver allows the kernel to participate in the BSM and for its state
to be read out by userspace.

**29_cap_mem.patch**<br>
In Pensando products the P4 dataplane is managed by userspace processes,
which directly mmap ASIC registers and dataplane memory.

The /dev/mem interface does not provide the access control, memory
address range control, and (related to 24_precise_pte_control)
attribute control.

The /dev/capmem driver allows us to eventually remove /dev/mem entirely
and perform access control.

**30_mnic_dtsi.patch**<br>
This is a device-tree change to instantiate multiple mnic devices.

**31_cadence_quadspi_fastread.patch**<br>
This is a read-performance modification that yields ~30% increase in
throughput on the ASIC and even more on the FPGA emulation platform.

It is not essential for the ASIC, were the flash performance is comfortable,
but is worthwhile when running under FPGA emulation.

**32_proc_xmaps.patch**<br>
Related to _29_cap_mem_ and _24_precise_pte_control_, this patch adds
a new /proc/<pid>/xmaps file, similar to /proc/<pid>/maps, which
decodes the PTE attributes so we can verify that memory pages
have been mapped with correct shareability attributes.

This is debug code, somewhat, and Pensando A72-specific.

**33_configs_and_dts.patch**<br>
This change includes the current kernel configs and devicetree for
Pensando cards.  Called "Naples-100", but is common across all current
cards.
