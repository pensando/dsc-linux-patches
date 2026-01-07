This directory is a continuation of patches from patches-2025-11; applying
to a v5.15.129 kernel tree, to support the Pensando Elba ASIC based card.

## Commits
**00_sbus_secure_mode_support.patch**<br>
```
drivers/soc/pensando/sbus: Secure mode support

Add generic pen_secure APIs for sbus driver to use to access secure
sbus registers.

In future other drivers can also use these APIs for other supported
secure registers.
```
**01_penfw_secure_mode_new_smcs.patch**<br>
```
drivers/soc/pensando/penfw: New SMC support for secure-mode.

Added support for the following new SMCs to penfw driver.

 - PENFW_OP_BSM_SET_RUNNING
 - PENFW_OP_SET_PCIE_PLL_CLK
 - PENFW_OP_ENABLE_SERDES_IROM
 - PENFW_OP_GET_BSM_STATE
 - PENFW_OP_GET_RESET_CAUSE
 - PENFW_OP_SET_RESET_CAUSE
```
**02_rstcause_secure_mode.patch**<br>
```
drivers/soc/pensando/rstcause: Add secure mode support.

RSTCAUSE register is one of the secure register on elba.
When secure mode is turned on, RSTCAUSE register needs to be
accessed using penfw SMC.
```
**03_bsm_secure_mode.patch**<br>
```
drivers/soc/pensando/bsm: Add secure-mode support.

BSM register is a secure-register on Elba.
Use SMC call interface to access BSM register when secure-mode
is turned on.
```
**04_elba_edac_secure_mode.patch**<br>
```
drivers/edac/elba_edac: Secure mode support

Elba edac registers are secure register and are not accessible
directly when secure-mode is turned on. Use SMC call to read/write
edac registers.
```
