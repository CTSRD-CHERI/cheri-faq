## Operating systems

<!-- toc -->

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


### How do I get started with CheriBSD on Morello?

Follow the
[Getting Started with CheriBSD guide](https://ctsrd-cheri.github.io/cheribsd-getting-started/)
and do not skip any section.

The guide has been structured to make you aware of actions you should take
before trying to use CheriBSD.
Of critical importance, you should upgrade your Morello board firmware before
installing CheriBSD.
