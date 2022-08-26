This directory is a continuation of patches from patches-2022-02; applying
to a v5.4.173 kernel tree, to support the Pensando Elba ASIC.

## New code for Elba SoC support
**00_elba_serror_fix.patch**<br>
```
arm64: fix platform_serror handling.

The previous serror handling patch incorrectly hooked bad_mode()
whereas it should have hooked do_serror().
```
