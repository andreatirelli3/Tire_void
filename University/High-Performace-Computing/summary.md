# Introduction
**High-performance computing** *is the practice of **aggregating computing power** to increase performance to solve complex problems.*

> **Embedded cmputing system (ES)**, not a computer but an electronic device that includes a programmable computer.

- Typically use a battery, **HPC** helps to obtain a balance between **performance** and **energy efficiency**.

> **Bell's law**, summarizes embedded systems evolution over time.
> *"Roughly every decade a new, lower priced computer class forms based on a new programming platform, network, and interface resulting in new usage and the establishment of a new industry."*

Hardware increase in:
- **parallelism**,
- heterogeneous.
The challenge for the developer is to maximize the performance from these systems.
# Multicore revolution
> There is no longer single ISA, but there are different sets of processors, each with their own ISA.

*High performance computers relies on highly **heterogeneous** systems/SoC.*
- SoC, System on Chip.

![[Schermata del 2024-06-14 09-48-19.png]]

> **Moore's law**, *The number of transistors in an integrated circuit doubles every two years.*

## Instruction-level parallelism
## Multicore
## Dark silicon
> *Portion of hardware that cannot be used at full power or cannot be used at all, to avoid increasing the power consumption.*

Dark silicon, consequence of **utilization wall**.
The proposed solution for this problem are the **Four horsemen**. Four different proposed solution to approach the problem of the utilization wall.
### Utilization wall
> *The portion of a chip that can work at full frequency drops exponentially after each processor generation because of power constraints.*

### Four Horsemen
1. **Shrinking**, manufacture should make smaller processors. *Not ideal because dark silicon does not mean **useless silicon**.*
2. **Dim**, can be divide in 2 types:
	- **spacial**, more core, but with lower frequency;
	- temporal, each core has a higher frequency, but not used all together. *widely used in battery-limited systems.*
3. **Specialized**, dark silicon used only to perform **specialized** tasks.
4. **Deus ex Machina**, move from the current **CMOS** to another one.
# Parallel systems

| pro                                                                                  | cons                                                     |
| ------------------------------------------------------------------------------------ | -------------------------------------------------------- |
| effective use of all (or most of) transistors in  a chip                             | performance improvement is only **theoretical**          |
| **scaling** is very easy                                                             | Needs of **synchronization** between processors          |
| each processor can be **less powerful**                                              | Developer should write code that can be **parallelized** |
| designing is easy, because once a processor has been tested it can be **replicated** |                                                          |
**Taxonomy of parallel computers**
![[Schermata del 2024-06-14 10-19-12.png]]
## SIMD
> Execute the same instruction on a large dataset. *(scalar-vector sum or multiplication)*.

``` python
for (int i = 0; i < N; i++)
  x[i] += s
```

In this case there is no need to stick to the Von Neumann instruction processing, because when has been decoded once, it can be executed **multiple times**.
![[Schermata del 2024-06-14 10-28-58.png]]
```
for each 4 members in array
{
  load 4 members of x to the SIMD register
  calculate 4 additions in one operation
  write the result from register to memory
}
```
SIMD needs adjacent values in memory. **Loop unrolling** can help revealing more data-level parallelism.
``` assembly
L1: ld    s0, 0(a0)
    ld    s1, 8(a0)
    ld    s2, 16(a0)
    ld    s3, 24(a0)
--------------------
    add   s1, s1, a1
    add   s2, s1, a1
    add   s0, s0, a1
    add   s3, s1, a1
--------------------
    sd    s0, 0(a0)
    sd    s1, 8(a0)
    sd    s2, 16(a0)
    sd    s3, 24(a0)
--------------------
    addi  a0, a0, 32
    bne   a0, t0, L1
```
Unrolling is done automatically by the compiler, are unrolled by a factor equal to the width of the SIMD unit of the architecture.
## MIMD
> **Multiple independent** pipelines, each one with its own **program counter**.

![[Schermata del 2024-06-14 11-13-16.png]]
Each processing node is interconnected through a *communication network*.
- **Thread-level parallelism**, each thread can use data-level parallelism ([[#SIMD]]).

Pattern of execution:
1. Multiple program, multiple data (MPMD), each thread executes a **different** program;
2. Multiple threads, multiple data (MTMD), each thread executes different part of the **same program**.

![[Schermata del 2024-06-14 11-21-25.png]]
## Memory architecture in multiprocessor system
![[Schermata del 2024-06-14 11-30-03.png]]
### Shared memory
- Each processor can address every physical location in the machine;
- Each process can address al data it shares with others;
- ***communication happens through load and stores***;
- **memory hierarchy**, multiple levels of cache.
![[Schermata del 2024-06-14 11-32-40.png]]
*Physically shared memory is more performante, because plain load/stores can be used to access data. In distributed shared memory must be used a message-passing protocol.*
> **NUMA architecture**, access to memory is **non-uniform**. Different processors take different time to accesso the memory, because of the **physical distance** between the processor and its own memory.

### Distributed memory
*Communication happens through **message exchange**. (If P1 needs to access P2's value of X:*
1. P1 sends a read request for X to P2;
2. P2 sends a copy of its X to P1;
3. P1 stores the received value in its own memory.

Requires **synchronization** between sender and receiver.
![[Schermata del 2024-06-14 11-45-52.png]]
Message are processed through a **FIFO queue**:
![[Schermata del 2024-06-14 11-47-44.png]]
```
// processor 1 - sequential
for (i=1 to 4)
	for (j=1 to 4)
		C[i][j] = distance(A[i], B[j])

--------------------------------------

// processor 1 - parallel w/ messages
A[n] = {...}
B[n] = {...}

Send(A[n/2+1...n], B[1...n])

for (i=1 to n/2)
	for (j=1 to n)
		C[i][j] = distance(A[i], B[j])
		
Receive(C[n/2+1...n][1...n])

// processor 2 - parallel w/ messages
A[n] = {...}
B[n] = {...}

Receive(A[n/2+1...n], B[1...n])

for (i=n/2+1 to n)
	for (j=1 to n)
		C[i][j] = distance(A[i], B[j])

Send(C[n/2+1...n][1...n])
```
