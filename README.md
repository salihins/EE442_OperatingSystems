# EE442_OperatingSystems
Projects to understand operating systems, written in C, in a Linux environment

--- Project 1 ---

In the first part of this assignment, you are asked to simulate chemical reactions to form water,
carbondioxyde, nitrogen dioxide and ammoniac. Due to synchronization problems it is hard to obtain
these molecules properly. In order to obtain water molecule, two H atoms and one O atom must get
all together at the same time. Similarly, one C atom-two O atoms, one N atom-two O atoms, and one
N atom-three H atoms must get all together simultaneously to form carbondioxyde, nitrogen dioxide
and ammoniac molecules, respectively.
We need to think atoms as threads. When we get all the required atoms at the same time, the related
chemical reaction happens. You are expected to use only mutexes, locks and condition variables for
synchronization, but you should avoid starvation, busy waiting etc.
In order to change the parameters of the program, command-line options should be used in this
assignment.

--- Project 1 ---

--- Project 2 ---

Threads can be separated into two kinds: kernel-level threads and user-level threads. Kernel-level threads are
managed and scheduled by the kernel. User-level threads are needed to be managed by the programmer and
they are seen as a single-threaded process from the kernel’s point of view. The user-level threads have some
advantages and disadvantages over kernel-level threads.
Advantages:
 User-level threads can be implemented on operating systems which do not support threads.
 Because there is no trapping in kernel, context switching is faster.
 Programmer has direct control over the scheduling policy.
Disadvantages:
 Blocking system calls block the entire process.
 In the case of a page fault, the entire process is blocked, even if some threads might be runnable.
 Because kernel sees user-level threads as a single process, they cannot take advantage of multiple CPUs.
 Programmer has to make sure that threads give up the CPU voluntarily or has to implement a periodic
interrupt which schedules the threads.
For this homework, you will write a program which manages user-level threads and schedules them using a
preemptive scheduler of your own. You will use <ucontext.h> to implement user-level threads


In your main() function, create user-level threads. A newly created thread should be assigned to an empty spot
in ThreadInfo array. If there is no empty spot, thread creation should wait for a thread to finish and an array
spot to be emptied.
Implement the following functions:
initializeThread (), which initializes all global data structures for the thread. You need to define actual data
structures but one constraint you have is to accommodate ucontext_t inside your structures.
createThread (), which creates a new thread. If the system is unable to create a new thread, it will return -1 and
print out an error message. This function will be used for setting up a user context and associated stack. A newly
created thread will be marked as READY when it is inserted into the system.
runThread (), which switches control from the main thread to one of the threads in the thread array, which also
activates the timer that triggers context switches.
exitThread (), which removes the thread from the thread array (i.e. the thread does not need to run anymore).
Scheduler:
In this assignment you are going to schedule 5 threads (processors). Each process has 3 processor burst times and
3 I/O burst times.
[1st CPU][1st I/O][2nd CPU][2nd I/O][3rd CPU][3rd I/O]
We are going to assume that we have five I/O servers (i.e. all processors can make I/O job concurrently) and a single
device queue. When the 1st CPU execution is completed, the processor’s state is changed from running to I/O and
does 1st I/O job. After I/O is completed, the processor returns to the device queue and waits for grabing CPU for
second CPU execution.
Scheduler 1: P&WF_scheduler (), which makes context switching (using swapcontext()) using a preemptive and
weighted fair scheduling structure of your choice (for example you can use lottery scheduling) with a switching
interval of two seconds. A context switch should take place when the associated interrupt comes every two
seconds. If a thread finishes, its place in the thread array will be marked as empty and the scheduler will free the
stack it has used.
Scheduler 2: SRTF_scheduler (), which implements “Shortest Reamining Time First Scheduling” algorithm. This
algorithm makes context switching with a switching interval of two seconds. A context switch should take place
when the associated interrupt comes every two seconds. If a thread finishes, its place in the thread array will be
marked as empty and the scheduler will free the stack it has used.

--- Project 2 ---

--- Project 3 ---

In this homework, you are expected to implement a simple user mode file system (FS) that employs a
file allocation table (FAT). The FS obtained could be used on a physical drive or on a disk image to be
named as “disk”
FAT consists of 4096 entries. Each entry is 4 bytes long and bytes are laid out in little-endian order
(meaning the most significant byte is at the highest address).
 The first entry must always be 0xFFFFFFFF.
 If an entry is 0x0, then the block is assumed to be empty.
 If an entry is 0xFFFFFFFF, then the block is assumed to be the last block of a file.
 If an entry has a value between 0x0 and 0x1000, the value points to the entry in the table where
the next block of the file is located.
 The file list contains 128 items, each of which is 256 bytes long.
 A file item in the list has the following layout:
 File name: Maximum of 247 characters + the ‘/0’ string delimiter (248 bytes)
 First block: Location of the first block of the file in the FAT (4 bytes)
 File size: Size of the file in bytes (4 bytes) 

File contents are written in 512-byte chunks.
Core Features:
FS is expected to support the following features:
1. Formatting: Overwrites the disk header with an empty FAT and an empty file list. The user should
be able to format a disk from the terminal by typing ./myfs disk –format
2. Writing: Copies a file to the disk. The command ./myfs disk -write source_file destination_file
should obtain the source_file and write it to the disk with name destination_file.
3. Reading: Copies a file from the disk. The command ./myfs disk -read source_file destination_file
should get the source_file in the disk and copy it to the computer as destination_file.
4. Deleting: Deletes a file in the disk. i.e. Entries occupied by the file should be emptied (0x00000000).
Command: ./myfs disk -delete file
5. Listing: Prints all visible files (i.e. files whose names do not start with a ‘.’) and their respective sizes
in the disk. Alphabetical order is not required. Command: ./myfs disk –list
6. Printing File List: Prints File List to the “filelist.txt” file. The layout of the output will be same as File
List in Table 2. All the entries of the file (including empty ones) should be printed. Command: ./myfs
disk –printfilelist
7. Printing FAT: Prints FAT to the “fat.txt” file. The layout of the output will be same as FAT in Table 1
(Use tab character to separate columns.). All the entries of the FAT (including empty ones) should be
printed. Command: ./myfs disk –printfat
8. Disk Defragmentation: Merges fragmented files into one contiguous space or block. This will allow
your system to access files save new ones more efficiently. Command: ./myfs disk –defragment.
*In order to test the correctness of the “Disk Defragmentation” you need to implement “Printing FAT”
feature. Without implementing it, you will not get a grade from “Disk Defragmentation”. 

--- Project 3 ---
