This directory is a continuation of patches from patches-2021-04; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**00_elba_rstcause.patch**<br>
This patch adds a reset cause driver.  This driver provides an event
bitmap, through sysfs, representing activity that caused the last
reset/reboot (called this_cause) and can be used to record events that
will cause the next reboot (called next_cause).
