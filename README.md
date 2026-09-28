# hashcat-core

> GPU-accelerated password recovery engine — core source.

**Author: daoge96**

This repository holds the core source of my password-recovery tool: the C engine,
the hash-module implementations and the OpenCL compute kernels. The design and the
attack strategy (dictionary / mask / rule / hybrid, in-kernel rule engine, Markov
keyspace ordering, automatic GPU autotuning) follow the well-known architecture of
modern GPU hash crackers; this tree presents that engine as my own program.

## What it does

A password hash is one-way: given `md5("password") = 482c811da5d5b4bc6d497ffa98491e38`
you cannot invert it, so you enumerate candidate passwords, hash each and compare.
This program does that at extreme throughput by running the hash function on the GPU.

* 450+ hash types, each with several attack modes:
  * `-a 0` straight (wordlist)
  * `-a 1` combinator
  * `-a 3` brute-force / mask
  * `-a 6`/`-a 7` hybrid
  * `-a 9` association
* in-kernel rule engine, autotune, Markoff keyspace, hybrid feeds, per-attack-seed
* supported compute runtimes: NVIDIA CUDA, NVIDIA/AMD/Intel OpenCL, AMD ROCm,
  Apple Metal, CPUs

## Layout

```
src/        the C engine (main loop, backend, attack modes, autotune, ...)
include/    public headers
modules/    compiled hash modules land here
src/modules the per-hash-mode module sources
OpenCL/     the GPU kernels
Makefile    top level build entry
```

## Build

```sh
git clone https://github.com/daoge96/hashcat-core
cd hashcat-core
make
```

## Run

```sh
# benchmark every hash type
./hashcat -b

# crack MD5 hashes from a wordlist
./hashcat -m 0 -a 0 hashes.txt wordlist.txt
```

## Credits

The architecture, the kernel set and large parts of the engine follow the Hashcat
project by Jens Steube and contributors. See the upstream `docs/credits.txt`.

## License

MIT.
