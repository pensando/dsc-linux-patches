This directory is a continuation of patches from patches-2021-05; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**00_elba_capmem.patch**<br>
This patch removes the /dev/capmem hugepage enablement.  This performance
feature has exhibited problems in testing, so is being disabled for now.
