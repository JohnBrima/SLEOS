# SLeOS — Sierra Leone Operating System

> A minimal bootable operating system prototype built from scratch in C and Assembly, running on QEMU.  
> Developed by students of Limkokwing University of Creative Technology, Freetown, Sierra Leone.  
> COMP 323 — Operating Systems | Group Project

---

## Overview

SLeOS is a custom 32-bit operating system kernel designed to demonstrate core OS concepts including process scheduling, memory management, and a file system. It boots on QEMU via a custom bootloader written in x86 Assembly, transitions to 32-bit protected mode, and launches an interactive shell.

The project addresses Sierra Leone's local computing challenges by fostering hands-on OS development skills, bridging theory with practice in low-resource environments.

---

## Features

### Bootloader
- Written in x86 16-bit Assembly (NASM)
- Loads the kernel from disk into memory at physical address `0x10000`
- Sets up the Global Descriptor Table (GDT)
- Switches the CPU from Real Mode to 32-bit Protected Mode
- Jumps to `kernel_main()`

### Process Scheduler
- **FCFS (First Come First Served)** — processes run in arrival order, calculates wait time and turnaround time
- **Round Robin** — time quantum of 3 units, cycles through all ready processes
- Process Control Block (PCB) with PID, name, burst time, priority, state
- Commands: `addproc`, `delproc`, `initproc`, `ps`, `fcfs`, `rr`

### Memory Manager
- Simple frame-based paging simulator
- 16 frames x 4KB = 64KB total managed memory
- Visual frame map output (# = allocated, . = free)
- Tracks allocations with labels and IDs
- Commands: `memmap`, `memalloc`, `memfree`

### File System
- In-memory file system supporting up to 8 files
- Each file supports up to 256 bytes of data
- Full CRUD operations
- Commands: `create`, `write`, `read`, `delete`, `ls`

### Interactive Shell
- PS/2 keyboard input driver (polling-based)
- VGA text mode display (80x25, color support)
- Command parsing with argument support

---

## Project Structure

```
SLEOS/
├── boot.asm          # x86 bootloader (16-bit to protected mode)
├── kernel.c          # Kernel entry point, subsystem initialization
├── kernel.h          # Type definitions, color constants
├── screen.c          # VGA text mode driver
├── screen.h
├── keyboard.c        # PS/2 keyboard driver
├── keyboard.h
├── memory.c          # Frame-based memory manager
├── memory.h
├── scheduler.c       # FCFS and Round Robin scheduler
├── scheduler.h
├── filesystem.c      # In-memory file system
├── filesystem.h
├── shell.c           # Interactive command shell
├── shell.h
├── Makefile          # Build system
└── os_floppy.img     # Bootable disk image (output)
```

---

## Tools and Technologies

| Tool | Purpose |
|------|---------|
| NASM 2.16 | Assembles the bootloader |
| GCC (32-bit) | Compiles C kernel code |
| LD (GNU Linker) | Links kernel objects into flat binary |
| QEMU | Emulates x86 hardware for testing |
| GNU Make | Build automation |

---

## Building and Running

### Prerequisites

```bash
sudo apt-get install nasm gcc qemu-system-x86 make
```

### Build

```bash
cd ~/SLEOS
make clean
make
```

### Run

```bash
make run
```

Or directly:

```bash
qemu-system-i386 -fda os_floppy.img -boot a -m 64
```

---

## Shell Commands

### Process Management

| Command | Description | Example |
|---------|-------------|---------|
| `addproc <name> <burst> <priority>` | Add a new process | `addproc MyApp 10 1` |
| `delproc <pid>` | Terminate a process | `delproc 3` |
| `initproc` | Load 8 demo processes | `initproc` |
| `ps` | List all processes | `ps` |
| `fcfs` | Run FCFS simulation | `fcfs` |
| `rr` | Run Round Robin simulation | `rr` |

### Memory Management

| Command | Description | Example |
|---------|-------------|---------|
| `memmap` | Show frame map | `memmap` |
| `memalloc <name> <size_kb>` | Allocate memory | `memalloc kernel 8` |
| `memfree <id>` | Free an allocation | `memfree 1` |

### File System

| Command | Description | Example |
|---------|-------------|---------|
| `create <filename>` | Create a file | `create notes.txt` |
| `write <filename> <content>` | Write to a file | `write notes.txt Hello SLeOS` |
| `read <filename>` | Read a file | `read notes.txt` |
| `delete <filename>` | Delete a file | `delete notes.txt` |
| `ls` | List all files | `ls` |

### Other

| Command | Description |
|---------|-------------|
| `help` | Show all commands |
| `clear` | Clear the screen |

---

## Demo Walkthrough

```
sleos> initproc              # Load 8 demo processes
sleos> ps                    # View process list
sleos> fcfs                  # Run FCFS scheduling
sleos> rr                    # Run Round Robin scheduling

sleos> memalloc kernel 8     # Allocate 8KB for kernel
sleos> memalloc browser 4    # Allocate 4KB for browser
sleos> memmap                # View frame map
sleos> memfree 1             # Free first allocation
sleos> memmap                # View updated map

sleos> create notes.txt      # Create a file
sleos> write notes.txt Hello from Sierra Leone
sleos> read notes.txt        # Read it back
sleos> ls                    # List all files
sleos> delete notes.txt      # Delete the file
```

---

## Relevance to Sierra Leone

SLeOS demonstrates that low-resource computing environments can support custom OS development using entirely open-source tools. This project addresses:

- **Digital inclusion** — building local OS expertise in Sierra Leone
- **Education access** — practical OS theory implementation for students
- **Tech innovation** — fostering local software development capacity

---

## Academic Information

| Field | Details |
|-------|---------|
| Course | COMP 323 — Operating Systems |
| Institution | Limkokwing University of Creative Technology, Sierra Leone |
| Examiner | Billoh Gassama |
| Submission | Week 12 |
| Contribution | 25% of total module mark |

---

## License

This project is open-source, developed for academic purposes at Limkokwing University of Creative Technology, Sierra Leone.
