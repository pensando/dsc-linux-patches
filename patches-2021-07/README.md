This directory is a continuation of patches from patches-2021-06; applying
to a v4.14.18 kernel tree, to support the Pensando Elba ASIC.

**00_capmem_dirty.patch**<br>
This patch marks capmem pages as writeable+dirty, to bypass the clean/dirty
page tracking on newer kernels (newer than 4.14.18).

This avoids the page-fault on first write, which measurably affects system
startup time.
