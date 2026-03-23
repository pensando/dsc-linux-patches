This directory is a continuation of patches from patches-2026-01; applying
to a v5.15.129 kernel tree, to support the Pensando Elba ASIC based card.

## Commits
**00-i2c-designware-timeout-fix.patch**<br>
```
i2c: designware: fix __i2c_dw_disable() in case master is holding SCL low

The DesignWare IP can be synthesized with the IC_EMPTYFIFO_HOLD_MASTER_EN
parameter.  In this case, when the TX FIFO gets empty and the last command
didn't have the STOP bit (IC_DATA_CMD[9]), the controller will hold SCL low
until a new command is pushed into the TX FIFO or the transfer is aborted.

When the controller is holding SCL low, it cannot be disabled.
The transfer must first be aborted.
Also, the bus recovery won't work because SCL is held low by the master.

Check if the master is holding SCL low in __i2c_dw_disable() before trying
to disable the controller. If SCL is held low, an abort is initiated.
When the abort is done, then proceed with disabling the controller.

This whole situation can happen for instance during SMBus read data block
if the slave just responds with "byte count == 0".
This puts the driver in an unrecoverable state, because the controller is
holding SCL low and the current __i2c_dw_disable() procedure is not
working. In this situation only a SoC reset can fix the i2c bus.
```
**01-mmc-hpi-to-interrupt-lengthy-cache-flush.patch**<br>
```
mmc: core: Use HPI to interrupt lengthy cache flush (#494)

There is no cache flush duration time limit per JEDEC
and the default timeout is 30 seconds with no retry.
When this timeout occurs a filesystem partition may
go read only.

JEDEC compliant devices support HPI to interrupt the
cache flush.  Support HPI interrupt of lengthy cache
flush to allow normal IO and then when the next periodic
cache flush is issued the cache flush continues.
```
