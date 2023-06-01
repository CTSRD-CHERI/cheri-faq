## General

<!-- toc -->

### What is CHERI?

[CHERI](https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/) (Capability Hardware Enhanced RISC Instructions) is a joint research project by SRI International and the University of Cambridge to revisit fundamental design choices in hardware and software to dramatically improve system security. [An Introduction to CHERI](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-941.pdf) provides a high-level introduction to CHERI.


### What types of threat has CHERI been designed to prevent?

CHERI enables fine-grained memory protection (e.g. to prevent out-of-bounds and use-after-free bugs) and highly scalable software compartmentalization (e.g. to mitigate future or unknown vulnerabilities in third-party software).


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


### How can I emulate a CHERI-enabled environment?

You can use pre-compiled Docker images from
[Docker Hub](https://hub.docker.com/u/ctsrd)
that include a ready-to-use QEMU-based CheriBSD VM and LLVM to cross-compile
code:

1. Download a Docker image.

   * For Arm Morello:

     ```
     docker pull ctsrd/cheribsd-sdk-qemu-morello-purecap
     ```

   * For CHERI-RISC-V:

     ```
     docker pull ctsrd/cheribsd-sdk-qemu-riscv64-purecap
     ```

1. Run a shell in a Docker container. The container will stop once you exit this
   session.

   * For Arm Morello:

     ```
     docker run -ti --rm --name cheribsd-morello \
         ctsrd/cheribsd-sdk-qemu-morello-purecap:latest
     ```

   * For CHERI-RISC-V:

     ```
     docker run -ti --rm --name cheribsd-riscv \
         ctsrd/cheribsd-sdk-qemu-riscv64-purecap:latest
     ```

1. Run a QEMU-based VM with CheriBSD and use `root` to log in once the `login:`
   prompt appears.

   * For Arm Morello:

     ```
     docker exec -ti cheribsd-morello \
         /opt/cheri/cheribuild/cheribuild.py run-morello-purecap
     ```

   * For CHERI-RISC-V:

     ```
     docker exec -ti cheribsd-riscv \
         /opt/cheri/cheribuild/cheribuild.py run-riscv64-purecap
     ```

1. You can compile code in the QEMU VM, as explained in the
   [Getting Started with CheriBSD guide](https://ctsrd-cheri.github.io/cheribsd-getting-started/helloworld/index.html),
   or cross-compile it in the Docker container shell session using a compiler
   in the directory `/opt/cheri/output/morello-sdk/bin/`.


### How can I build and emulate a CHERI-enabled environment?

1. Clone the
   [cheribuild](https://github.com/CTSRD-CHERI/cheribuild)
   repository.

1. [Install dependencies](https://github.com/CTSRD-CHERI/cheribuild#pre-build-setup)
   for your operating system.

1. Build a software stack and run CheriBSD in a QEMU-based VM.

   * For Arm Morello: `cheribuild.py run-morello-purecap -d`

   * For CHERI-RISC-V: `cheribuild.py run-riscv64-purecap -d`


### Where are cheribuild.py images and build files stored?

By default, images built by `cheribuild.py` are stored in `$HOME/cheri/output` and build files in `$HOME/cheri/build`.


### Can I build a custom CheriBSD branch using cheribuild.py?

Yes. `cheribuild.py` implements several flags to allow you to build multiple CheriBSD branches and store the results separately:
* `--cheribsd-<target>/source-directory /path/to/your/branch` to specify a custom branch's source code directory.
* `--cheribsd-<target>/build-directory /path/to/your/build` to specify where build files should be stored.
* `--cheribsd-<target>/install-directory /path/to/your/rootfs` to specify where root filesystem files should be installed.
* `--disk-image-<target>/path /path/to/your/image.img` to specify where a disk image should be stored.


### Can I develop baremetal applications for Arm Morello?

See this [example](https://git.morello-project.org/morello/docs/-/blob/morello/mainline/common/standalone-baremetal-readme.rst) application.
