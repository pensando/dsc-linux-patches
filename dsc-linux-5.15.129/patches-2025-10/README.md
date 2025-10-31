This directory is a continuation of patches from patches-2025-09; applying
to a v5.15.129 kernel tree, to support the Pensando Elba and Giglio ASIC
based card.
## Commits
**0001-perf-arm-cmn-Giglio-SoC-has-direct-interrupt-wired-t.patch**<br>
```
perf/arm-cmn: Giglio SoC has direct interrupt wired to GIC

The workaround using the PRP interrupt chain only applies
to the Salina SoC.
```
**0002-EDAC-elba-revert-some-changes-in-patches-2025-04.patch**<br>
```
EDAC/elba: revert some changes in patches-2025-04

Revert MODULE_LICENSE and pr_crit() changes in patches-2025-04
0005-EDAC-elba-Support-AMD-Pensando-Giglio-SoC.patch
```
**0003-Fix-gcc-version-11.3.1-compile-warnings.patch**<br>
```
Fix gcc version 11.3.1 compile warnings
```
**0004-arm64-dts-Add-AMD-Pensando-Giglio-SoC-support.patch**<br>
```
arm64: dts: Add AMD Pensando Giglio SoC support
```
**0005-mmc-sdhci-cadence-Fix-byte-enable-GENMASK-usage.patch**<br>
```
mmc: sdhci-cadence: Fix byte enable GENMASK usage

Elba byte enables are bits 6:3 of priv->ctrl_addr
```
**0006-soc-pensando-Giglio-SoC-eMMC-interrupt-driver.patch**<br>
```
soc/pensando: Giglio SoC eMMC interrupt driver

The Giglio SoC requires use of the GIG_EM_CSR_AXI_INTREG
to handle the eMMC interrupt.
```
**0007-kpcimgr-support-port-bifurcation-XEN-PSCI-feature-ne.patch**<br>
```
kpcimgr: support port bifurcation, XEN, PSCI, feature
 negotiation, pciesvc load via sysfs

* kpcimgr: XEN, PSCI and feature negotiation support
Introduces new feature negotiation support
Introduces support of virtual UART if booted through XEN
Introduces support for PSCI relase of CPU

* kpcimgr: support loading of pciesvc as firmware
currently pciesvc is being loaded as kernel module,
that creates dependency on kernel version and
compilation.

this patch creates support of loading pciesvc as a
firmware via sysfs entry to eliminate above dependency.

it also expands validity checks to ensure that pciesvc
is not referencing any external functions.

* kpcimgr: support pcie port bifurcation (#497)
Support MSI interrupts configuration for multiple active ports

* kpcimgr: optimize msi intr allocation and make old fields compat fields
user macros to iterate over msi descriptors and derive port number from index
active_port and msi are now compat fields, for compatibility with old pciesvc

* kpcimgr: use kernel version specific msi desciterator macros
msi iterator macros changes from kernel version 5.15 created
macros to choose the right macro based on kernel version

* kpcimgr: add feature flag for port bifurcation
reject new pciesvc module if it does not support port bifurcation
and kpcimgr is currently running in port bifurcation
```
