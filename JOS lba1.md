MIT 6.828 JOS


Lab1 Bootstrapping

#The PC's Physical Address Space

##Intel 8088 16-bit(1MB)

- 0x00000000-0x0009FFFF(640KB) Low Memory

- 0x000A0000-0x000BFFFF(128KB) VGA Display

- 0x000C0000-0x000EFFFF(192KB) 16-bit devices, expansion ROMS

- 0x000F0000-0x000FFFFF(64KB)  BIOS

###Modern PCs have a hole in physical memory from 0x000A0000 to 0x00100000, dividing RAM into conventional memory

#The ROM BIOS

### f000:fff0---0xffff0  ljmp  $0xf000,$0xe05b (real mode)

- The IBM PC starts executing at physical address 0x000ffff0, which is at the very top of the 64KB area reserved for the ROM BIOS.
- The PC starts executing with CS = 0xf000 and IP = 0xfff0.
- The first instruction to be executed is a jmp instruction, which jumps to the segmented address CS = 0xf000 and IP = 0xe05b.

###physical address = 16*segment+offset

- Set up and interrupt descriptor table
- Initialize various devices such as VGA display
- After initializing the PCI bus, searches a bootable device
- Transfer control to boot loader

#The Boot Loader  0x7c00

- Sector - 512 byte
- The first sector is boot sector
- Load the boot sector into 0x7c00 - 0x7dff
- jmp to 0000:7c00 

###boot/boot.S

- Switched the processor from real mode(access the first 1MB memory) to 32-bit protected mode(Virtual memory, above the 1MB)

- reads the kernel from the hard disk by directly accessing the IDE disk devices registers via x86's special I/O instructions

#Kernel 0x10000c(0xf010000c)

##entry.s

	# Load the physical address of entry_pgdir into cr3.  entry_pgdir
	# is defined in entrypgdir.c.
	movl	$(RELOC(entry_pgdir)), %eax
	movl	%eax, %cr3
	# Turn on paging.
	movl	%cr0, %eax
	orl	$(CR0_PE|CR0_PG|CR0_WP), %eax
	movl	%eax, %cr0

before the piece of code,


