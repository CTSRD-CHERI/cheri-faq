## General

### What is CHERI?

CHERI (Capability Hardware Enhanced RISC Instructions) is an international research project to revisit the fundamental security assumptions in both hardware and software. [An Introduction to CHERI](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-941.pdf) provides a high-level overview of the project. It builds upon an earlier fruit-themed project known as [BERI](https://www.cl.cam.ac.uk/research/security/ctsrd/beri/).


### What types of threat has CHERI been designed to prevent?

CHERI has been developed to have two applications: memory-safe C/C++ and software compartmentalization.

Memory-safe C/C++ includes spatial, referential, and temporal protection mechanisms, defending against both vulnerabilities and exploitation techniques. Out-of-bound writes and reads, i.e. [buffer overflows](https://en.wikipedia.org/wiki/Buffer_overflow) and over-reads, are but two examples of spatial access vulnerability that can be avoided. Exploit chains and techniques, such as the [confused deputy problem](https://en.wikipedia.org/wiki/Confused_deputy_problem), can be prevented directly.

[Compartmentalizations](https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/cheri-compartmentalization.html) are fine-grained models that sandbox a given application, limiting the likelihood of it crashing and resulting in a denial of service if attacked. Such a compartment therefore provides resilience to a variety of applications or services that need to demonstrate high-availability or -reliability, e.g. image processors. Where malicious payloads would ordinarily crash or compromise the image processor, the compartmentalized application would simply render the standard icon for missing images in the browser instead.


### What CHERI-extended hardware is available to use?

[Arm Morello](https://www.arm.com/architecture/cpu/morello)
is currently the only System-on-Chip with a CHERI-extended CPU.

There are several
[CHERI-RISC-V prototypes on FPGA](https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/cheri-risc-v.html),
including a superscalar, out-of-order CHERI-Toooba core extending
[Toooba](https://github.com/bluespec/Toooba)
that
is based on
[RiscyOO](https://github.com/csail-csg/riscy-OOO).

Additionally, CHERI-RISC-V and Morello can be emulated using QEMU-CHERI or
Morello FVP.


### How can I get an Arm Morello board?

Go to the
[Digital Security by Design](https://www.dsbd.tech/get-involved/morello-board-request/)
website and follow the instructions there.


### How can I emulate CHERI?

1. Clone the
   [cheribuild](https://github.com/CTSRD-CHERI/cheribuild)
   repository.

1. [Install dependencies](https://github.com/CTSRD-CHERI/cheribuild#pre-build-setup)
   for your operating system.

1. Build a software stack and run CheriBSD in a QEMU-based VM.

   * For Arm Morello: `cheribuild.py run-morello-purecap -d`

   * For CHERI-RISC-V: `cheribuild.py run-riscv64-purecap -d`


### Where are the cheribuild.py images and builds stored?

By default, any images will reside in `$HOME/cheri/output` and builds in `$HOME/cheri/build`.


### Can I build targets from specific branches using cheribuild.py?

Yes. Once executed, the cheribuild.py script will create a `$HOME/cheri` directory to contain all of the repositories, builds, and configuration needed. If you change to the specific directory for your target, you will then be able to check out the correct branch and run `./cheribuild.py` against it.
```bash
cd $HOME/cheri/cheribsd
git checkout releng/22.12

cd $HOME/cheribuild
./cheribuild.py cheribsd-morello-purecap
```


### Can I develop bare-metal applications for Morello?

Yes. Arm have created an example of a [bare-metal application](https://git.morello-project.org/morello/docs/-/blob/morello/mainline/common/standalone-baremetal-readme.rst) for the board.


## Operating systems


### What operating systems are being developed for CHERI?

* [CheriBSD](https://cheribsd.org/) for
  [Arm Morello](https://www.arm.com/architecture/cpu/morello) and
  [CHERI-RISC-V](https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/cheri-risc-v.html)

* [Linux](https://linux.morello-project.org/docs/) for
  [Arm Morello](https://www.arm.com/architecture/cpu/morello)

* [Android](https://git.morello-project.org/morello/docs/-/blob/morello/mainline/android/readme.rst)
  for
  [Arm Morello](https://www.arm.com/architecture/cpu/morello)

* [CHERIoT RTOS](https://github.com/microsoft/cheriot-rtos) for the
  [CHERIoT platform](https://aka.ms/cheriot-tech-report)

* seL4

  There are several institutions interested in developing seL4 for CHERI.
  You can talk to them in the `#sel4` channel at the
  [CHERI-CPU](https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/cheri-slack.html)
  Slack.


### Which operating system should I use to experiment with CHERI?

Check the
[CHERI OS-feature matrix](https://www.morello-project.org/cheri-feature-matrix/)
to compare CheriBSD and Morello Linux.

At the moment,
[CheriBSD](https://cheribsd.org/)
is the most fully featured operating system running on CPUs with CHERI
extensions.
It also provides a large number of pre-compiled third-party software
dependencies that you can simply install with package managers for your project.
If you intend to work with code that runs on UNIX-like operating systems, we
recommend you to start with CheriBSD.


## CheriBSD


### How do I get started with CheriBSD on Morello?

Follow the
[Getting Started with CheriBSD guide](https://ctsrd-cheri.github.io/cheribsd-getting-started/)
and do not skip any section.

The guide has been structured to make you aware of actions you should take
before trying to use CheriBSD.
Of critical importance, you should upgrade your Morello board firmware before
installing CheriBSD.


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
[these instructions](/#how-can-i-switch-to-another-cheribsd-kernel-eg-a-pure-capability-kernel)
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


## Software porting


### What programming languages can I use on CHERI?

* CHERI C/C++

  Currently, the most mature programming languages for CHERI are CHERI C and
  CHERI C++.

  See the
  [CHERI C/C++ Programming guide](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-947.pdf)
  for more information.

* Rust

  There is an
  [ongoing project](https://soft-dev.org/events/cheritech22/slides/Cooksey.pdf)
  at the University of Kent to add support for CHERI in Rust.

* Python/Ruby/Lua/JavaScript/...

  There are no interpreters that support CHERI for these programming languages.


### Can I use Python with CHERI?

There is no Python interpreter that would run your Python code using
capability-aware instructions.

You can use a Python interpreter compiled for a baseline architecture but it
does not give you any of the security benefits of a CHERI-enabled CPU.


### Is my software safer just by running it on a CHERI-extended CPU?

No. While an operating system (i.e., a kernel, basic programs and libraries
shipped with the OS) might be compiled for the pure-capability ABI and hence
prevent memory-safety bugs in its components, code compiled for a baseline
architecture (e.g., AArch64 for Morello, RISC-V for CHERI-RISC-V) does not
benefit from security features of CHERI.

You must compile your code to explicitly use CHERI capabilities to access memory
either by compiling it for the pure-capability ABI or the hybrid ABI with
appropriate data type qualifiers that indicate the intention of using CHERI
capabilities in place of legacy C pointers (in case of C/C++).


### What are the pure-capability and hybrid ABIs?

The pure-capability ABI (also known as CheriABI in CheriBSD and PCuABI in Linux
and Android) is a new process ABI that uses only CHERI capabilities to access
memory and interact with a kernel.

The hybrid ABI allows a process to access memory using legacy pointers and CHERI
capabilities.
When compiling such C/C++ code, a pointer must be annotated with the
`__capability` type qualifier to use a CHERI capability instead of a legacy
pointer.

Both pure-capability and hybrid CheriBSD kernels support the pure-capability and
hybrid ABIs at the same time.
You can also switch between pure-capability and hybrid CheriBSD kernels without
replacing user-space programs and libraries.

See the
[CheriABI](https://www.cl.cam.ac.uk/research/security/ctsrd/pdfs/201904-asplos-cheriabi.pdf)
paper and the
[PCuABI](https://git.morello-project.org/morello/kernel/linux/-/wikis/Morello-pure-capability-kernel-user-Linux-ABI-specification)
specification for more information on the ABIs.


### Should I compile my code for the pure-capability or hybrid ABI?

In most cases, porting software to the pure-capability ABI is straightforward
and does not heavily disrupt code.
For example, the Capabilities Limited company shows in their report
[Assessing the Viability of an Open-Source CHERI Desktop Software Ecosystem](https://www.capabilitieslimited.co.uk/_files/ugd/f4d681_e0f23245dace466297f20a0dbd22d371.pdf)
that they needed to modify 0.026% lines of code in around 6 million lines
of code from KDE, Qt, X11 for CheriABI to run KDE Plasma on top of Xvnc on
CheriBSD.

It might seem that compiling code for the hybrid ABI is a better approach to
port software to CHERI than compiling it for the pure-capability ABI because you
can incrementally introduce CHERI capabilities in your code, and not worry about
all incompatibilities with CHERI at once.

However, when using the hybrid ABI, you have to annotate all pointers as CHERI
capabilities, and still you will not have any guarantees that there are no
legacy pointers left.
Additionally, all third-party libraries that your code depends on will not use
CHERI capabilities, unless you change them as well.

There are still some situations when choosing the hybrid ABI over the
pure-capability ABI has benefits.
In case of extremely large code bases (e.g., the Chromium browser and its
dependencies), you might want to compartmentalize parts of your code base now
(e.g., the V8 JavaScript engine in Chromium), parts that are known for security
vulnerabilities with significant impacts, rather than spend a lot of time
trying to port the whole project for the pure-capability ABI and then
compartmentalize it.


### How easy it is to compile C/C++ code for the pure-capability ABI?

The LLVM-CHERI assists a developer in C/C++ code porting to CHERI C/C++ by
printing appropriate warnings whenever it encounters incompatibilities between
C/C++ and CHERI C/C++ semantics.
In many cases, it is sufficient to only fix compile-time issues to successfully
run a program compiled for the pure-capability ABI.

However, programming language interpreters, Just-in-Time compilers and other
software that embeds metadata in pointers, implements custom memory allocators
or performs other non-trivial pointer manipulations can cause run-time issues
that must be investigated using a debugger.
A user can examine these issues with the GDB-CHERI debugger that can disassemble
capability-specific CPU instructions and display properties of CHERI
capabilities, including their validity, stored in registers and in memory.

See the
[CHERI C/C++ Programming guide](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-947.pdf)
for example compiler warnings and solutions to them,
and the
[Getting Started with CheriBSD guide](https://ctsrd-cheri.github.io/cheribsd-getting-started/helloworld/index.html)
for an example CheriABI "Hello World" – how to compile and debug it.


### Can I link CHERI C/C++ code compiled for the pure-capability ABI and the hybrid ABI?

No. LLVM-CHERI does not allow to link a program for one ABI with libraries
compiled for another ABI.


### What software has been ported to CHERI?

For CheriBSD, there are ~9,000 pre-compiled CheriABI packages, including
KDE Plasma 5, Git, Rsync, sudo, Bash, tmux.
Besides that, there are ~24,000 hybrid ABI packages, including LLVM-CHERI,
GDB-CHERI, Chromium, Firefox, Vim, Python, Meson, Ninja.

You can list all available software on CheriBSD with `pkg64c rquery %n` and
`pkg64 rquery %n`.
Alternatively, you can search for a specific package using
`pkg64c search <pattern>` and `pkg64 search <pattern>`.

If you simply require a list of packages independently of CheriBSD, there are tarballs on [pkg.cheribsd.org](https://pkg.cheribsd.org) that contain JSON-formatted data. To generate a list of CHERI packages with specific fields, you can use `curl`, `tar`, and `jq`:
```bash
curl -O https://pkg.cheribsd.org/CheriBSD:20220828:aarch64c/packagesite.txz
tar xf *.txz
jq -j '.name," (",.version,"; ",.abi,"): ",.www,"\n"' *.yaml | sort -uf > aarch64c.txt
```
To create a similar list of hybrid packages, use the following instead:
```bash
curl -O https://pkg.cheribsd.org/CheriBSD:20220828:aarch64/packagesite.txz
```

The packages are built using
[CheriBSD ports](https://github.com/CTSRD-CHERI/cheribsd-ports),
a fork of
[FreeBSD ports](https://github.com/freebsd/freebsd-ports)
that includes a collection of ~32,000 third-party software ports for FreeBSD.

See the
[Getting Started with CheriBSD](https://ctsrd-cheri.github.io/cheribsd-getting-started/packages/)
guide for more information on third-party software packages,
and the
[CheriBSD Ports and Packages](https://freebsdfoundation.org/wp-content/uploads/2023/05/CheriBSD_ports.pdf)
article to read more on the package building process.


### How can I print a CHERI capability in CHERI C/C++?

See the
[Displaying Capabilities](https://github.com/CTSRD-CHERI/cheri-c-programming/wiki/Displaying-Capabilities)
wiki page for
[printf(3)](https://man.cheribsd.org/cgi-bin/man.cgi/dev/printf.3)
string formats.

You can also store a formatted string with properties of a CHERI capability
using
[strfcap(3)](https://man.cheribsd.org/cgi-bin/man.cgi/dev/strfcap.3).


### How can I display a CHERI capability in GDB-CHERI?

See the
[Displaying Capabilities](https://github.com/CTSRD-CHERI/cheri-c-programming/wiki/Displaying-Capabilities)
wiki page for `print` command output formats.


## Compartmentalization


### What compartmentalization models are available for CHERI?

* [Library-based compartmentalization](https://man.cheribsd.org/cgi-bin/man.cgi/c18n.7)
  in CheriBSD

  Available for Morello in the 22.12 release.

* [Colocation](https://github.com/CTSRD-CHERI/cheripedia/wiki/Colocation-Tutorial)
  in CheriBSD

  Available for CHERI-RISC-V in the
  [cocalls branch](https://github.com/CTSRD-CHERI/cheribsd/tree/cocalls).

  There are plans to make this model available for Morello as well.


## Community


### Where should I ask questions related to CHERI?

Join the
[CHERI-CPU](https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/cheri-slack.html)
Slack workspace or the
[cl-cheri-discuss](https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/cheri-lists.html)
mailing list and ask questions there.

Alternatively, if your question is related to Linux or Android for Arm Morello,
you can join the
[Morello Forum](https://community.arm.com/support-forums/f/morello-forum).


### Where can I report issues related to CHERI?

If you are not sure what is a cause of your issue, you can contact us first,
as described above.

* [Getting Started with CheriBSD on Arm Morello](https://github.com/CTSRD-CHERI/cheribsd-getting-started/issues)

* [CheriBSD](https://github.com/CTSRD-CHERI/cheribsd/issues)

* [CheriBSD ports and packages](https://github.com/CTSRD-CHERI/cheribsd-ports/issues)

* [LLVM-CHERI for Morello](https://git.morello-project.org/morello/llvm-project/-/issues)

* [LLVM-CHERI](https://github.com/CTSRD-CHERI/llvm-project/issues)

  Issues related to LLVM-CHERI but not specific to Morello should be reported
  here.

* [GDB-CHERI](https://github.com/CTSRD-CHERI/gdb/issues)

* [QEMU-CHERI](https://github.com/CTSRD-CHERI/qemu/issues)


### How can I contribute to this document?

We encourage anyone interested in improving this document to make contributions
to it. In order to add a new question:

1. Add your question and answer to `src/questions.md`.

1. Create a pull request in the
   [CTSRD-CHERI/cheri-faq](https://github.com/CTSRD-CHERI/cheri-faq/pulls)
   repository with your change.
