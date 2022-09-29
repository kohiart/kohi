---
description: Design, discussion, and benchmarks for KohiLang.
---

# KohiLang

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
