# coconutOS

A Rust microkernel for GPU-isolated AI inference.

What started as a [tongue-in-cheek Linux distro](https://www.youtube.com/watch?v=lE4UXdJSJM4) inspired by the legendary Richie Guix has evolved into something slightly more ambitious: a capability-based microkernel written from scratch in Rust, designed to run AI inference workloads with GPU isolation that Linux can't offer.

Turns out, when you joke about building your own OS long enough, you eventually just... build one.

## What is coconutOS?

coconutOS runs "shards" — isolated address spaces with their own page tables, capabilities, and GPU partitions. The supervisor (microkernel) is ~5K lines of Rust with zero external runtime dependencies. GPU drivers run in user-mode shards, not ring 0. VRAM is zeroed on free. Every resource is capability-gated. Think OpenBSD's philosophy, applied to GPU compute.

It boots on x86-64 (QEMU/UEFI), isolates GPU partitions via IOMMU, and runs a proof-of-concept transformer inference engine end-to-end.

Read more: [What Comes After the Last Programming Language?](https://raskell.io/articles/what-comes-after-the-last-programming-language/)

## Repositories

| Repo | What it is |
|------|-----------|
| [`coconutOS`](https://github.com/coconut-os/coconutOS) | The microkernel, bootloader, GPU HAL, inference stack — everything |
| [`coconut-mklive`](https://github.com/coconut-os/coconut-mklive) | The original Linux distro that started it all (legacy) |

## Quick Start

```bash
brew install qemu mtools llvm   # or apt equivalent
git clone https://github.com/coconut-os/coconutOS.git
cd coconutOS && ./scripts/qemu-run.sh
```

## Status

| Phase | Status |
|-------|--------|
| CPU microkernel (shards, IPC, capabilities, filesystem) | ✅ Complete |
| GPU isolation (IOMMU, partitioning, DMA, pledge/unveil, ASLR) | ✅ Complete |
| Inference stack (runtime, C FFI, llama2.c port, pipeline parallelism, profiling) | ✅ Complete |
| Benchmarking (Llama 70B latency vs. Linux/ROCm baseline) | 🔄 In progress |
| Hardening & multi-vendor (NVIDIA, Apple, formal verification) | Planned |

## License

[ISC](https://github.com/coconut-os/coconutOS/blob/main/LICENSE)

---

*"Breaks more often, just how I like it..."* — Richie Guix
