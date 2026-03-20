# Two-Pass Assembler in C (with Macro Preprocessor)

A modular **assembler** written in **C**, developed as a university systems-programming lab project.  
The tool processes assembly source files, expands macros (pre-assembler stage), and then performs a **two-pass assembly** to generate machine-code output files and symbol usage reports.

---

## Pipeline Overview

1. **Pre-Processor (Macro Expansion)**
   - Reads the original `.as` file
   - Expands macros
   - Produces a preprocessed `.am` file

2. **First Pass**
   - Parses each line and validates syntax
   - Builds/updates the **symbol table**
   - Computes instruction/data memory layout and addresses

3. **Second Pass**
   - Resolves symbols (labels, externals, entries)
   - Encodes instructions/data into machine words
   - Writes final output files

---

## Input / Output Files

### Input
- `*.as` - assembly source file (original input)

### Outputs (as applicable)
- `*.am` - preprocessed file after macro expansion
- `*.ob` - object file (encoded machine words; format per course specification)
- `*.ent` - entry labels (generated only if `.entry` labels exist)
- `*.ext` - external label usages (generated only if `.extern` symbols are used)

> If assembly fails due to errors, the program typically avoids generating partial/invalid output files.

---

## Build

### Requirements
- GCC (or compatible compiler)
- GNU Make
- Linux/macOS (recommended)

### Clean
```bash
make clean
```

The project is intended to compile cleanly with strict warning flags (e.g., -Wall -ansi -pedantic) as required by the course workflow

---

### Run
Run the assembler by providing one or more base filenames (without extension).
For each name, the program reads <name>.as and produces outputs for that input.

```bash
./assembler inputMain valid_input empty
```

This expects files like:
- inputMain.as
- valid_input.as
- empty.as

And may generate:
- inputMain.am/.ob/.ent/.ext (depending on directives and symbol usage)
  
---

### Repository Structure
.

├── assembler.c                # main driver (preprocess → first pass → second pass)

├── assembler.h

│

├── pre_proc.c                 # macro expansion → emits .am

├── pre_proc.h

├── pre_proc_errors.c          # preprocessing validation / error scenarios

│

├── first_pass.c               # first pass (symbol table, layout, validation) - my part

├── first_pass_helpers.c

├── first_pass_helpers.h

├── first_pass_error_checks.c

├── first_pass_error_checks.h

│

├── second_pass.c              # second pass (resolution + output) - partner’s part

├── second_pass.h

├── translation_unit.c         # encoding / translation utilities - partner’s part

├── translation_unit.h

│

├── structs.c                  # shared data structures

├── functions.c                # shared utilities

│

├── inputMain.as               # examples / tests

├── inputMain.am

├── inputMain.ob

├── inputMain.ent

├── inputMain.ext

│

├── valid_input.as

├── valid_input.am

├── valid_input.ob

│

├── valid_macro.*              # macro tests

├── invalid_macro.as

│

├── empty.*                    # edge cases

├── error_second_pass.*        # second-pass error cases

└── first_pass_*_errors.*      # first-pass error cases (e.g., .as / .am variants)
