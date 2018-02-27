+++
title = "load iNES"
description = "Load iNES file to NES"
date = 2018-02-27
template = "page.html"
tags = ["NES"]
+++

## First of all
NES consists of some parts. For example CPU, PPU, APU and so on. I start from loading iNES file,  which is a format of NES binary programs. You can check iNES format [here](http://wiki.nesdev.com/w/index.php/INES).

## Two parts of iNES
iNES have two parts. PRG_ROM and CHR_ROM. PRG_ROM is for game logic and CHR_ROM is for graphic. When iNES is loaded, PRG_RAM load PRG_ROM data and V_RAM load CHR_ROM.

## PRG_RAM and V_RAM
PRG_RAM and V_RAM has 2KB memory.

## init project
~~~
% cargo init --bin nes
% cd nes
~~~

## nes module
This is the today's directory structure.
~~~
% tree
.
├── lib.rs
├── main.rs
└── nes
    ├── cpu
    │   └── mod.rs
    ├── mod.rs
    └── ppu
        └── mod.rs
~~~

### CPU
`nes/cpu/mod.rs`
```rust
pub struct Cpu {
    prg_ram: PrgRam,
}

impl Cpu {
    pub fn new(prg_ram: PrgRam) -> Cpu {
        Cpu { prg_ram }
    }
}

pub struct PrgRam {
    memory: Box<[u8; 0xFFFF]>,
    v_ram: Arc<Mutex<VRam>>,
}

impl PrgRam {
    pub fn new(memory: Box<[u8; 0xFFFF]>, v_ram: Arc<Mutex<VRam>>) -> PrgRam {
        PrgRam { memory, v_ram }
    }
}
```

### PPU
`nes/ppu/mod.rs`
```rust
pub struct Ppu {
    v_ram: Arc<Mutex<VRam>>,
}

impl Ppu {
    pub fn new(v_ram: Arc<Mutex<VRam>>) -> Ppu {
        Ppu { v_ram }
    }
}

pub struct VRam(Box<[u8; 0xFFFF]>);
impl VRam {
    pub fn new(memory: Box<[u8; 0xFFFF]>) -> VRam {
        VRam(memory)
    }
}
```

## debug
