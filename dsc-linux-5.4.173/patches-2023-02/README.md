This directory holds a patch set that can be applied to
a v5.4.173 kernel tree, so as to support the Pensando Elba ASIC
based card; as of 02/16/2023.

**00_penfw.patch**<br>
```
drivers/soc/pensando: penfw driver
penfw driver provides sysfs and ioctl interface for the userspace
to communicate to the bl31 secure monitor
```
**01_elba_defconfig_cleanup.patch**<br>
```
defconfig: cleanup elba_defconfig
Remove CONFIG_MFD_SYSCON as that is automatically enabled when
MFD_PENSANDO_ELBASR is enabled.
```
