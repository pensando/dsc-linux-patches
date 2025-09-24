This directory is a continuation of patches from the previous patch set; applying
to a v5.15.129 kernel tree, to support the Pensando Elba ASIC based card.

## Commits
**00-penfw-sbus.patch**<br>
```
drivers/soc/pensando : penfw driver: Add support for secure services using 
new ioctl argument format. Also Add support for new SMCs.
drivers/soc/pensando: sbus driver: Add support for secure sbus register
read/write via SMC.

```
