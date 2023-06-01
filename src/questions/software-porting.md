## Software porting

<!-- toc -->

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

You can browse CheriABI and hybrid ABI package repositories at [pkg.cheribsd.org](https://pkg.cheribsd.org/).

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
