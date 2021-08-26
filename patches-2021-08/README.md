This directory is a continuation of patches from patches-2021-07; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**00_pcie_msg.patch**<br>
This patch removes the kernel logging of (expected) PCIE access errors,
which can occur during link initialization.  The errors are still
counted, but the kernel logs are not cluttered.

**01_mnic_memreserved.patch**
This patch creates a reserved memory region for use by the mnet
instances.  This avoids memory allocation failure due to wider system
memory fragmentation.

