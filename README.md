<!-- ANCHOR: cover -->

# CHERI Frequently Asked Questions

University of Cambridge, SRI International

This is a living document with frequently asked questions regarding Capability
Hardware Enhanced RISC Instructions (CHERI).
The document aims to help people get started with experimenting with CHERI
hardware-software stacks, including Arm Morello and CHERI-RISC-V.

CHERI-related competition participants (e.g., Digital Security by Design
Technology Access Programme (DSbD TAP), Defence and Security Accelerator (DASA))
might find this document useful when exploring their project prototype ideas.

## Contributions

We encourage anyone interested in improving this document to make contributions
to it, e.g. by adding a new question or extending an answer.
See
[How can I contribute to this document?](../questions/community.md#how-can-i-contribute-to-this-document)
for more information.


<!-- ANCHOR_END: cover -->

## Building

Building the book from the Markdown sources requires
[mdBook](https://github.com/rust-lang/mdBook)
and
[mdbook-toc](https://crates.io/crates/mdbook-toc).
Once installed, `mdbook build`
will build the static HTML files in the `book/` directory, whilst `mdbook
serve` will build and serve them at `http://localhost:3000`. Please refer to
the mdBook documentation for futher options.
