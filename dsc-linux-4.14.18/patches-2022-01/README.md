This directory is a continuation of patches from patches-2021-08; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**00_edac.patch**<br>
The edac driver reports correctable and uncorrectable errors from the
DDR controller.  The driver relies on device-tree nodes installed
by u-boot to convey:
- The active DDR channels.
- The NOC DDR hashes and bypass regions.

The active DDR channels tell the driver which channels to monitor.
The hashes allow the driver to reverse the memory address hashing so
that the reports can provide a real system address for the fault.

**01_ltc3888.patch**<br>
This patch adds support for the LTC3888 VRM on Ortano-ADI cards.
- Adding support for ltc3888 and ltc3888-1 driver
- Adding IOUT read for core and arm for ADI cards

**02_trap_serror.patch**<br>
Handle SError interrupt of type Decode Error.
On Elba, an access to an invalid bus address results in a Decode SError
that will panic the kernel.  Bugs in the HAL that cause these are very
hard to debug, so trap these and if coming from user space, turn the
Decode Error into SIGBUS.  Otherwise, if coming from the kernel, ignore.
