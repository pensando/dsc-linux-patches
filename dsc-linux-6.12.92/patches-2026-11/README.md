This directory is a continuation of patches from patches-2026-09; applying
to a v6.12.92 kernel tree, to support the AMD Pensando Elba, Giglio and
Salina ASIC based cards.

## Commits
**0001-edac-salina-using-BL31-SMC-for-register-access.patch**<br>
```
edac: salina: using BL31 SMC for register access

edac: salina: add MC EDAC driver using BL31 SMC for register access
```
**0002-penfw-Added-support-for-SMC-call-to-get-secure-interrupts.patch**<br>
```
penfw: Added support for SMC call to get secure interrupts

New interrupts whitelist for salina has been added in ATF and sw.
```
**0003-arm64-dts-amd-salina-tune-eMMC-strobe-delay-and-driver-strength.patch**<br>
```
arm64: dts: amd: salina: tune eMMC strobe delay and driver strength

On some boots at the high-voltage corner, eMMC initialisation fails
with:

  mmc0: error -84 whilst initialising MMC card

The core then resets the card, and the second attempt brings it up in
HS400ES.

Set the Cadence SD4HC PHY DLL strobe delay to 0x0c and the eMMC driver
type to 1 so the first initialisation succeeds. The settings apply to
all qualified eMMC vendors.

Mark the device non-removable: it is soldered down and has no card
detect.

Tested on Leni boards with WDC, Phison and Kingston eMMC devices.
The cards run at HS400 enhanced strobe, 200 MHz, 8-bit.
```
**0004-soc-pensando-penfw-secure-services-support-in-new-ioctl-argument-format.patch**<br>
```
soc: pensando: penfw: secure services support in new ioctl argument format

Few additional changes:
- use local variables to save the passed in lengths, to prevent memory leak/corruption during free
- Pass the input and output lengths in x5 and x6 registers of the SMC call to support EL3 services that need to know buffer sizes.
- Remove unsafe is_user_address_valid() function (High). Function used find_vma() incorrectly (only checks first byte). It was missing proper RCU/task lifetime handling. Kernel's copy_from_user/copy_to_user already validate addresses
- Add mutex serialization to PENFW_SVC path (High)
- Remove pid parameter from penfw_svc_smc() (cleanup)
- Make penfw_dev globally accessible (cleanup)
- Fix GFP allocation flags. Removed GFP_ATOMIC from GFP_KERNEL | GFP_ATOMIC
- Fix UAPI struct penfw_svc_args. Changed uint64_t/uint16_t to __u64/__u16 (kernel UAPI types) Added explicit padding: reserved1 (2 bytes), reserved2 (6 bytes) to prevent compiler-dependent ABI padding issues
-  pensando: penfw - add packed attribute and ioctl size validation
- Add static_assert checks to verify the struct layout: Total size must be 48 bytes, value field at offset 24 (verifies 6-byte padding after length_in) and status field at offset 40 (verifies 6-byte padding after length_out). These compile-time checks ensure the explicit padding in penfw_svc_args produces the expected layout, catching any compiler that might add different padding or misalign fields.

---------
```
**0005-soc-pensando-penfw-add-DICE-SMC-ioctl.patch**<br>
```
soc: pensando: penfw: add DICE SMC ioctl

* soc: pensando: penfw: add DICE SMC ioctl

Add a PENFW_DICECALL ioctl that lets userspace use the DICE services
in ATF BL31 (SiP FID 0xC2000009):

  - GET_CERTIFICATE: read the DICE cert chain in chunks
  - SIGN_DATA: sign data with the DICE leaf key
  - GET_MEASUREMENT: read a measurement and its hash algorithm
  - SET_MEASUREMENT: record a measurement
```
**0006-arm64-amd-add-Xen-dom0less-support-for-Elba-and-Salina.patch**<br>
```
arm64: amd: add Xen dom0less support for Elba and Salina

Add device trees and defconfigs to boot AMD Pensando Elba and Salina
DSCs under the Xen hypervisor in a dom0less configuration with two
guests: a smartnic domain owning the control-plane peripherals and a
hostnic domain serving the PCIe host interface.

Document the bindings the new DTs use: xen,domain, xen,cpupool and
multiboot,module (Xen's dom0less boot ABI, described in the Xen tree
at docs/misc/arm/device-tree/booting.txt), and oracle,captun. Add the
multiboot and oracle vendor prefixes.
```
**0007-soc-pensando-sal_crash-add-Salina-QSPI-panic-dump-driver.patch**<br>
```
soc: pensando: sal_crash: add Salina QSPI panic dump driver

Write the kernel message buffer to coredump_partition on panic via
polled CQSPI indirect writes.  Gate PENSANDO_SOC_CRASH on Elba and
Giglio where the direct-access model applies.

Make the panic path independent of state left by other qspi1 users:
keep the controller runtime-resumed and skip the dump across system
sleep, restore its chip-select/timing and write settings captured from
spi-nor, wait for the flash to be ready, and bound all polling.  Arm
only when the whole partition on qspi1 is erased, and add an "armed"
sysfs attribute to re-arm after erase.  Move cap_reboot under its own
Kconfig symbol and export its helpers so the driver builds as a module
or without PENSANDO_SOC_PCIE.
```
