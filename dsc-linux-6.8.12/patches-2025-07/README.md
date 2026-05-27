This directory is a continuation of patches from patches-2025-04;
applying to a v6.8.12 kernel tree, to support the Pensando Salina,
Elba and Giglio ASIC based cards.

## Commits
**01_mmc-core-Use-HPI-to-interrupt-lengthy-cache-flush.patch**<br>
```
There is no cache flush duration time limit per JEDEC
and the default timeout is 30 seconds with no retry.
When this timeout occurs a filesystem partition may
go read only.

JEDEC compliant devices support HPI to interrupt the
cache flush.  Support HPI interrupt of lengthy cache
flush to allow normal IO and then when the next periodic
cache flush is issued the cache flush continues.
```
**02_irqchip-pensando-Fix-partial-of_iomap-leak-on-error.patch**<br>
```
If of_iomap() fails partway through mapping multiple regions,
the previously mapped regions were not being unmapped, leading
to a memory/resource leak.
```
