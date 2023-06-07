## CheriBSD

<!-- toc -->

### How can I upgrade CheriBSD?

Currently, there are no binary upgrades for CheriBSD.

The only way to upgrade a CheriBSD host is to build and install CheriBSD from
source code.
See the
[wiki page](https://github.com/CTSRD-CHERI/cheripedia/wiki/HOWTO:-Build-CheriBSD-natively-on-Morello)
for instructions how to do this.
Note that we cannot guarantee stability when upgrading CheriBSD and it is
important to make sure you can recover your data if an upgrade fails.


### How can I build CheriBSD from source code?

You have two choices:
* Build and install CheriBSD natively on Arm Morello using
[these instructions](https://github.com/CTSRD-CHERI/cheripedia/wiki/HOWTO:-Build-CheriBSD-natively-on-Morello)

* Cross-compile (on FreeBSD, macOS or Linux) CheriBSD for CHERI-RISC-V or Arm Morello using
[cheribuild](https://github.com/CTSRD-CHERI/cheribuild)


### Does CheriBSD implement spatial safety (e.g., to prevent out-of-bounds bugs)?

The official CheriBSD release runs basic programs and libraries compiled
for the pure-capability ABI and a hybrid kernel.

It also includes a pure-capability kernel in
`/boot/kernel.GENERIC-MORELLO-PURECAP`.
See
[these instructions](/cheri-faq/questions/cheribsd.html#how-can-i-switch-to-another-cheribsd-kernel-eg-a-pure-capability-kernel)
to find out how to do that.


### Does CheriBSD implement temporal safety (e.g., to prevent use-after-free bugs)?

Currently, the official CheriBSD release does not include temporal safety
mechanisms.
This feature (also known as Cornucopia in CheriBSD) is scheduled for a future
release and can be used today by installing a Cornucopia-enabled kernel and
revocation-aware memory allocators.

See the
[Cornucopia tutorial](https://github.com/CTSRD-CHERI/cheripedia/wiki/HOWTO:-Use-Cornucopia-with-the-22.12-CheriBSD-Release)
to read more how to use it.


<!--
NOTE: When changing this title, remember to update references in this document.
-->
### How can I switch to another CheriBSD kernel (e.g., a pure-capability kernel)?

Use one of the following methods:
* Run `nextboot -k KERNCONF`

  See [nextboot(8)](https://man.cheribsd.org/cgi-bin/man.cgi/nextboot.8) for more
  details.

* Add `kernel="KERNCONF"` to `/boot/loader.conf`

  See [loader.conf(5)](https://man.cheribsd.org/cgi-bin/man.cgi/loader.conf.5) for
  more details.

* In a boot loader, press `5` multiple times to select `KERNCONF`

`KERNCONF` is a name of a directory in the `/boot` directory with a kernel
(e.g., `kernel.GENERIC-MORELLO-PURECAP`) that corresponds to a kernel
configuration file from CheriBSD source code (e.g.,
[GENERIC-MORELLO-PURECAP](https://github.com/CTSRD-CHERI/cheribsd/blob/main/sys/arm64/conf/GENERIC-MORELLO-PURECAP)).


### Is there any IDE for CheriBSD?

No. The closest to it is Kate Editor but it is still far from an actual IDE.
