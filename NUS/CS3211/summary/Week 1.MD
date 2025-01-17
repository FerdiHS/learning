# CS3211: Parallel and Concurrent Programming Week 1 (Introduction to Concurrency)

## Introduction

Welcome to CS3211: Parallel and Concurrent Programming. This module covers the fundamental concepts and techniques used in parallel and concurrent programming. You will learn how to design, implement, and analyze parallel and concurrent programs to improve performance and efficiency.

## Topics Covered

- Introduction to Parallel and Concurrent Programming
- Parallel Architectures and Models
- Synchronization and Communication
- Parallel Algorithms and Patterns
- Performance Analysis and Optimization
- Concurrent Data Structures
- Case Studies and Applications

## Learning Outcomes

By the end of this module, you will be able to:

1. Explain the concurrent programming challenges.
2. Define and correctly use different syncrhonization mechanisms.
3. Apply concurrent programming languages in program in different programming languages.
4. Define and identify different issues with concurrent programs.
5. Adapt the concurrent programming principles to new programming languages and paradigms.

## Timeline
| Week | Material Learned |
|------|------------------|
| 1    | Introduction to Concurrency |
| 2    | Tasks and Threads in Modern C++ |
| 3    | Synchronization, Modules, and Condition Variables in C++ |
| 4    | Atomic and Memory Model in C++ |
| 5    | Concurrent Data Structures in C++ |
| 6    | Testing and Debugging Concurrent C++ Programs |
| 7    | Concurrency in Go |
| 8    | Concurrency Patterns in Go |
| 9    | Classic Synchronization Problems in C++ and Go |
| 10   | Safety in Rust |
| 11   | Asynchronous Programming in Rust |
| 12   | Formal Verification and Model Checking |
| 13   | Recap |

## Resources

- Textbook: "C++ Concurrency in Action" by Anthony Williams

## Concurrency vs Parallelism

### Concurrency
- Concurrency is the ability of executing multiple (two or more) activities at the same time.
- For computer, concurrency refers to single system performing multiple independent activities in parallel, rather than sequentially.
- Multiple tasks can start, run, and complete in overlapping time periods.
- The tasks might not be running at the same instant.
- Multiple execution flows make progress at the same time by interleaving their execution.

### Parallelism
- Parallelism is the ability of executing multiple activities simultaneously.
- Multiple tasks can run simultaneously at the same time.
- Tasks do not only make progress at the same time, but also execute simultaneously.

## Process Interaction with OS

### Exceptions
- Executing a machine level instruction can cause an exception.
- Example: Overflow, Underflow, Division by Zero, etc.
- Occur due to program execution (synchronous).
- Can be handled by Exception Handlers.

### Interrupts
- External events can cause an interrupt.
- Example: Timer, Mouse Movement, Keyboard Input, etc.
- Occur due to external events independent of the program (asynchronous).
- Can be handled by Interrupt Handlers.

## Processes and Threads

### Disadvantages of Processes
- Process creation is costly.
  - Overhead system calls.
  - All data structures need to be initialized or copied. 
- Process communication is expensive.

### Threads
- Lightweight processes.
- Consist multiple independent control flows.
- The thread defines a sequential execution stream within a process (PC, SP, registers).
- Threads share the same address space and resources.
- Threads generation is faster since memory copying is not required.
- Different threads of a process can be assigned to different cores of a multi-core processor.
- User usually create threads using libraries, the threads are called user-level threads.
- Threads can be created using the OS, the threads are called kernel-level threads.

### Process and Thread Illustration

![Process and Thread Illustration](../images/proccess_threads_illustration.png)

## Problems in Concurrent Programs

### Race Condition
- Occurs when the timing or order of events affects the outcome of executing concurrent code.
- Multiple threads (or processes) access a shared resource (memory address) without any synchronization and at least one thread (or process) modifies the shared resource.
- Example: Two threads incrementing a shared variable at the same time.
- Race condition can be avoided by using critical sections and utilizing mutual exclusion (mutex).

#### Mutual Exclusion (Mutex)
- Use mutual exclusion (mutex) to synchronize access to shared resources.
- Lock the mutex before accessing the shared resource and unlock the mutex after accessing the shared resource.
- Only one thread can hold (lock) the mutex at a time.
- Other threads must wait until the mutex is unlocked.

#### Mechanisms
- Locks
- Semaphores
- Monitors
- Messages

### Deadlock
- Occurs when two or more threads are waiting for each other to release a resource but none of them release the resource:
    - When threads compete for access to limited resources.
    - When threads are incorrectly synchronized.
    - When threads need multiple locks to access some shared resources.
- Also known as "Circular Wait".

### Livelock
- Occurs when two or more threads are actively trying to resolve a deadlock where their states are constantly changing with respect to each other but no progress is being made.

### Starvation
- Occurs when a thread is prvented from making progress because some other threads have the resource it requires, and the initial thread does not get a chance to acquire the resource.
- Usually, starvation is caused by a scheduling algorithm that does not provide fairness.

## Concurrency Disadvantages
- Concurrency issues.
- Maintenance difficulties.
    - Debugging challenges.
    - Complicated code.
- Threading overhead.
    - Context switching.
    - Synchronization overhead.

## Concurrency Advantages
- Separation of concerns.
- Performance improvement.
    - Take advantage of hardware.
    - Optimization strategies.

## Concurrency Challenges
- Finding enough parallelism (Amdahl's Law). [Covered in CS3210]
- Granularity of tasks. [Covered in CS3210]
- Locality. [Covered in CS3210]
- Coordination and synchronization. [Covered in CS3211]
- Debugging. [Covered in CS3211]
- Performance and monitoring. [Covered in CS3210]
