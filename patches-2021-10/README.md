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
