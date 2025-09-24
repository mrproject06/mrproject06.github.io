---
title: "Embedded Lecture-3: Linker Script"
date: 2025-09-22 00:00:00  +0500
categories: [Embedded]
tags: [Microcontroller]
---

### Linker Script

-   Linker script is a text file which explains how different sections of the object files should be
    merged to create an output of file.
-   Linker and locator combination assigns unqiue absoulte addresses to different section of the output
    file by referring to address information mentioned in the linker script.
-   Linker scripts are written using the GNU linker command language.
-   Linker script also includes the code and data memory address and size information.
-   GNU linker script has the file extension of .ld.
-   You must supply linker script at the linking phase to the linker using -T option.

### Linker Script Commands
-   Entry
-   Memory
-   Sections
-   KEEP
-   ALIGN
-   AT>

### ENTRY COMMAND
-   This command is used to set the `Entry Point Address` information in the header of final elf file
    generated.
-   In this case `Reset_Handler` is the entry point into the application. The first piece of code that
    executes right after the processor reset.
-   The debugger use this information to locate the first function to execute.
-   Not a mandatory command to use, but required when you debug the elf file using the debugger (GDB).
-   Synatx: Entry(_symbol_name_)
    Entry(Reset_Handler)

### Memory Command
-   This command allows you to descrive the different memories present in the target and their start
    address and size information.
-   The linker uses information mentioned in this command to assign addresses to merged sections.
-   The information is given under this command also helps the linker to calculate total code and data
    memory consumed so far and throw an error message if data, code, heap or stack areas cannot fit into
    available size
-   By using memory command, you can fine tune various memories available in your target and allow different
    sections to occupy different memory areas.
-   Typically one linker script has one memory command.
-   Syntax of memory command.
    MEMORY
    {
            name (attr): ORIGIN=origin, LENGTH=len
    }
    attribute letter: R, W, X, A, I, L ! 

### Section Command
-   SECTIONS command is used to create different output sectionsin the final elf executable generated.
-   Important command by which you can instruct the linker how to merge the input sections to yield an output section.
-   This command also controls the order in which different output sections appear in the elf file generated.
-   By using this command,you also maintain the placement of a section in a memory region. For example, you
    instruct the linker to place the .text section in the FLASH memory region, which is described by the MEMORY
    command.
-   SECTIONS
    {
        .text:
        {
        }>(vma)AT>(lma)
        .data
        {
        }>(vma)AT>(lma)
    }

### Location Counter (.)
-   This is a special linker symbol denoted by a dot `.`
-   This symbol is called `location counter` since linker automatically updates this symbol with location
    (address) information
-   You can use this symbol inside the linker script to track and define boundaries of various sections
-   You can also set location counter to any specific value while writing linker script.
-   Location counter should appear only inside the SECTIONS command
-   The location counter is incremented by the size of the output section
-   It always track VMA.

### Linker Script Symbol
-   A symbol is the name of an address
-   A symbol declaration is not equivalent to a variable declaration what you do in your `C` application
-   Example : __max_heap_size = 0x400; 

