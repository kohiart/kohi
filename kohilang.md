---
description: Design, discussion, and benchmarks for KohiLang.
---

# KohiLang

KohiLang is an intermediate format for graphics rendering. It is agnostic to the underlying rasterizer. It has the following design goals:

* Reduce the storage cost of long-form generative artworks, especially on Ethereum.&#x20;
* Enable multiple graphics libraries and tools to switch to an EVM native backend, by choosing to compile to KohiLang.
* Enable graphics-based [hyperstructures ](https://jacob.energy/hyperstructures.html)built around an immutable core of graphics rendering capabilities on Ethereum.

Instead of storing complete Solidity programs for EVM native artworks, or storing complete third party libraries and their implementations, such as the p5js library and accompanying source code, those programs are compiled to KohiLang, and the resulting operation instruction set is stored instead, read by a composer application, and fed to a rasterizer, such as [KohiComposer](kohicomposer.md).

### Design Challenges

There are multiple technical hurdles to overcome to achieve the result of a render-agnostic instruction set for drawing.

* EVM performance in terms of runtime execution is poor for all tested EVM implementations (geth, nethermind, and evmone), making current long-form native artworks exhibit rendering times between 4 hours and 9 days.
* Due to the runtime cost, general purpose EVM clients and third-party node providers (Etherscan, Alchemy, Infura, et. al) disallow rendering. This limitation currently enforces IPFS-based caching for displaying works at scale, limits artworks to static presentation, and forces recovery (re-creating the cached artwork directly from on-chain EVM bytecode) to occur through local nodes with custom configuration and limitations removed (see: [mochi](https://github.com/kohiart/mochi))

KohiLang is one piece of the solution to the problem. By providing a compact intermediate format, storage for future works is minimized, and a standard instruction set is useful for benchmarking other approaches, namely, GPU-acceleration of EVM native artworks, which is a core focus for Kohi.

### nano

Nano is an extremely simple graphics stack machine to establish benchmarks for further work.&#x20;

| OPCODE | Stack Output                                                   | Description                                                           |
| ------ | -------------------------------------------------------------- | --------------------------------------------------------------------- |
| SIZE   | `width:uint32, height:uint32`                                  | Defines the canvas size. Must be the first opcode.                    |
| CLEAR  | `color:uint32`                                                 | Clears the canvas with a given ARGB color value.                      |
| SOLID  | `blend:bool, color:uint32, [command:byte, x:uint64, y:uint64]` | Draws solid (closed and filled) geometry based on provided path data. |

### nano-benchmarks

{% hint style="info" %}
For generative works, all benchmarks use the seed `13371337`
{% endhint %}

| Artwork              | Instruction Count | Instruction Size (in bytes) |
| -------------------- | ----------------- | --------------------------- |
| City Lights          | 448,887           | 98,735,191                  |
| the Universe Machine | 224,416           | 52,427,340                  |
