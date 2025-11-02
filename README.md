# Demystifying the Embedded C Build Process

## 1. Introduction

This repository documents the step-by-step process of converting C source code into an executable binary for an embedded target, **entirely from the command line**. The primary objective is to build and analyze each stage of the compilation toolchain manually, without the abstraction of an Integrated Development Environment (IDE).

By executing each tool (preprocessor, compiler, assembler, linker) individually, I demonstrate the precise flow of transformations a program undergoes—from human-readable `.c` files to machine-specific `.hex` files. This project serves as a practical exploration of compiler mechanics, file formats (ELF, HEX), and build automation using Makefiles.

The analysis is performed using the **GNU ARM Embedded Toolchain**, a standard in professional embedded systems development.

---

## 2. The Build Process: From `.c` to `.hex`

The journey from a source file to a format a microcontroller can execute involves several distinct stages. The entire set of tools that performs these conversions is known as the **Toolchain**. The following diagram illustrates this standard flow.


# Demystifying the Embedded C Build Process

## 1. Introduction

This repository documents the step-by-step _ process of converting C source code into an executable binary for an embedded target_ , **entirely from the command line**. The primary objective is to build and analyze each stage of the compilation toolchain manually, without the abstraction of an Integrated Development Environment (IDE).

By executing each tool (preprocessor, compiler, assembler, linker) individually, I demonstrate the precise flow of transformations a program undergoes—from human-readable `.c` files to machine-specific `.hex` files. This project serves as a practical exploration of compiler mechanics, file formats (ELF, HEX), and build automation using Makefiles.

The analysis is performed using the **GNU ARM Embedded Toolchain**, a standard in professional embedded systems development.

---

## 2. The Build Process: From `.c` to `.hex`

The journey from a source file to a format a microcontroller can execute involves several distinct stages. The entire set of tools that performs these conversions is known as the **Toolchain**. The following diagram illustrates this standard flow.

```mermaid
graph TD
    %% === INPUT FILES ===
    subgraph "📁 Input Source Files"
        A[main.c]:::source
        B[led.c]:::source
        C[led.h]:::header
    end

    %% === STAGE 1: COMPILATION ===
    subgraph "🧩 Stage 1: Compilation (Each .c File)"
        A --> D(Preprocessor):::process
        C --> D
        D --> E["main.i<br/>(Expanded C Code)"]:::file
        E --> F(Compiler):::process
        F --> G["main.s<br/>(Assembly Code)"]:::file
        G --> H(Assembler):::process
        H --> I["main.o<br/>(Relocatable Object File)"]:::object

        B --> J(Preprocessor):::process
        C --> J
        J --> K["led.i<br/>(Expanded C Code)"]:::file
        K --> L(Compiler):::process
        L --> M["led.s<br/>(Assembly Code)"]:::file
        M --> N(Assembler):::process
        N --> O["led.o<br/>(Relocatable Object File)"]:::object
    end

    %% === STAGE 2: LINKING ===
    subgraph "⚙️ Stage 2: Linking"
        I --> P(Linker):::link
        O --> P
        Q[linker_script.ld]:::config --> P
        P --> R["program.elf<br/>(Executable & Linkable Format)"]:::elf
    end

    %% === STAGE 3: CONVERSION ===
    subgraph "🔄 Stage 3: Conversion"
        R --> S(Object Copy):::convert
        S --> T["program.hex<br/>(Flashable Hex File)"]:::hex
    end

    %% === FINAL TARGET ===
    subgraph "🎯 Final Target"
        T --> U((Microcontroller Flash)):::target
    end

    %% === STYLES ===
    classDef source fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#000,font-weight:bold;
    classDef header fill:#fff3e0,stroke:#fb8c00,stroke-width:2px,color:#000,font-weight:bold;
    classDef process fill:#ede7f6,stroke:#5e35b1,stroke-width:1.5px,color:#000;
    classDef file fill:#f3e5f5,stroke:#8e24aa,stroke-width:1.5px,color:#000;
    classDef object fill:#c8e6c9,stroke:#2e7d32,stroke-width:2px,color:#000,font-weight:bold;
    classDef link fill:#fff9c4,stroke:#f9a825,stroke-width:2px,color:#000,font-weight:bold;
    classDef config fill:#fbe9e7,stroke:#d84315,stroke-width:1.5px,color:#000;
    classDef elf fill:#c5cae9,stroke:#3949ab,stroke-width:2px,color:#000,font-weight:bold;
    classDef convert fill:#e0f2f1,stroke:#00695c,stroke-width:2px,color:#000,font-weight:bold;
    classDef hex fill:#ffe0b2,stroke:#ef6c00,stroke-width:2px,color:#000,font-weight:bold;
    classDef target fill:#c8e6c9,stroke:#1b5e20,stroke-width:3px,color:#000,font-weight:bold;

```
