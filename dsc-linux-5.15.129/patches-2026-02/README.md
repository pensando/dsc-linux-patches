This directory is a continuation of patches from patches-2026-01; applying
to a v5.15.129 kernel tree, to support the Pensando Elba ASIC based card.

## Commits
**00_bsm_secure_mode_fixes.patch**<br>
```
drivers/soc/pensando/bsm: Fix various issues with secure-mode.

Don't check for bsm.base in secure-mode.
Fix clearing RUNNING flash in success_store() when in secure-mode.
Update bsm.val appropriately so that getters of device attributes
work correctly.
```
**01_penfw_pcie_serdes_fw_download.patch**<br>
```
drivers/soc/pensando/penfw: Add support for pcie serdes fw download.

PCIe serdes fw download needs secure register access.
In secure-mode, use SMC to download serdes firmware.
```
