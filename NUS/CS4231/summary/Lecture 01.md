# Lecture 01: Mutual Exclusion (Mutex)

# Mutual Exclusion Problem

## Context

- Found in **shared memory system**, such as multi-processor computers or multi-threaded programs.
- **Goal**: Prevent multiple processes from accessing the critical section simultaneously.

## Key Properties

- **Mutual Exclusion**: At most one process in the critical section.
- **Progress**: If no process is in the critical section, one of the waiting processes must enter.
- **No Starvation**: Every process that wants to enter will eventually enter.

## Attempts to Solve Mutual Exclusion Problem

### Unsuccessful Approaches

1. **Shared Boolean Variable**
    - A single shared boolean variable, `openDoor`, represents whether a process can enter the critical section.
    - <details>
        <summary> Pseudocode </summary>
        
        ```cpp
        bool openDoor = true; // Shared variable
        
        void RequestCS(int processId) {
            while (!openDoor) // Busy waiting
            openDoor = false; // Close the door after entering
        }
        
        void ReleaseCS(int processId) {
            openDoor = true; // Open the door after leaving
        }
        ```
        </details>
        
    - This approach will fail when two processes simultaneously check `openDoor == true`, both can enter the critical section, violating the mutual exclusion property.
2. **Turn-Based Solution**
    - Processes alternate turns using a shared `turn` variable.
    - <details>
        <summary>Pseudocode</summary>
        
        ```cpp
        int turn = 0; // Shared variable
        
        void RequestCS(int processId) {
            while (turn != processId // Busy waiting
        }
        
        void ReleaseCS(int processId) {
            turn = (processId + 1) % n; // Pass the turn to the next process
        }
        ```
        </details>
        
    - This approach will fail when a process holding the `turn` is delayed indefinitely, others cannot proceed (starvation).
3. **Array of Flags Without Synchronization**
    - Each process indicates its desire to enter the critical section with an array of flags, `wantCS`.
    - <details>
        <summary>Pseudocode</summary>
        
        ```cpp
        bool wantCS[2] = {false, false}; // Shared array
        
        void RequestCS(int processId) {
            wantCS[processId] = true;
            while (wantCS[1 - processId]) // Busy waiting
        }
        
        void ReleaseCS(int processId) {
            wantCS[processId] = false;
        }
        ```
        </details>
        
    - This approach will fail when both processes call `RequestCS()` and set their own `wantCS` to `true` at the same time. They will block each other indefinitely (deadlock).

### Successful Approaches

1. **Peterson’s Algorithm**
    - Combines the `wantCS` array of flags with a `turn` variable to ensure that only one process can enter.
    - <details>
        <summary>Pseudocode</summary>
        
        ```cpp
        bool wantCS[2] = {false, false}; // Shared array
        int turn = 0; // Shared variable
        
        void RequestCS(int processId) {
            int other = 1 - processId; // The other process
            wantCS[processId] = true;
            turn = other;
            while (wantCS[other] && turn == other) // Busy waiting
        }
        
        void ReleaseCS(int processId) {
            wantCS[processId] = false;
        }
        ```
        </details>
        
    - This approach will only work for two processes.
    - <details>
        <summary>Why it works?</summary>
        
        - **Mutual Exclusion**
            - Proof by contradiction.
            - Assume both processes are requesting access to the critical section. After both processes execute `turn = other`, the turn variable will hold either `0` or `1`. Consequently, for at least one of the processes, the condition `wantCS[other] && turn == other` will evaluate to `false`. This process will then proceed into the critical section, ensuring that only one process gains access to the critical section.
            - Assume both processes are requesting access to the critical section.
                - This means both processes execute the statement `turn = other`.
            - After executing `turn = other`:
                - The `turn` variable will hold either `0` or `1`.
            - For at least one of the processes:
                - The condition `wantCS[other] && turn == other` will evaluate to `false`.
            - The process for which the condition evaluates to `false`:
                - Proceeds into the critical section.
            - **Conclusion**:
                - This mechanism ensures that only one process gains access to the critical section at a time.
        - **Progress**
            - Proof by contradiction.
            - When `turn == 0`, then *P0* can enter. Otherwise, *P1* can enter.
        - **No Starvation**
            - Proof by contradiction.
            - Assume, without loss of generality, that **P0** is waiting indefinitely.
                - This implies **P0** is stuck in the while loop, meaning `wantCS[1] = true` and `turn = 1` at all times.
                - For this to occur, **P1** must continuously maintain `wantCS[1] = true`, indicating it is always trying to enter the critical section.
            - From the **Mutual Exclusion** property:
                - Only one of **P0** or **P1** can enter the critical section at a time.
                - Since **P0** is assumed stuck, **P1** must enter the critical section.
            - Upon exiting the critical section:
                - **P1** sets `wantCS[1] = false`, breaking the while loop for **P0**.
            - To keep **P0** stuck indefinitely:
                - **P1** would need to immediately reset `wantCS[1] = true`.
                - However, **P1** would also set `turn = 0`, giving **P0** priority.
            - **Contradiction**:
                - **P0** cannot remain indefinitely waiting because the turn mechanism ensures fairness.
            - **Conclusion**: Starvation is impossible in Peterson’s Algorithm.
        </details>
2. **Lamport’s Bakery Algorithm**
    - Processes “take a ticket” and wait for their turn.
    - <details>
        <summary>Pseudocode</summary>
        
        ```cpp
        bool choosing[n] = {false}; // Shared array
        int number[n] = {0};        // Shared array
        
        void RequestCS(int myId) {
            choosing[myId] = true;
            number[myId] = 1 + *max_element(number, number + n); // Take the next ticket
            choosing[myId] = false;
        
            for (int j = 0; j < n; ++j) {
                while (choosing[j]) // Wait if another process is choosing
                while (number[j] != 0 && (number[j] < number[myId] || 
                      (number[j] == number[myId] && j < myId))) // Wait for processes with smaller tickets
            }
        }
        
        void ReleaseCS(int myId) {
            number[myId] = 0; // Release the ticket
        }
        ```
        </details>
        
    - This approach will also work for multiple (more than two) processes.
    - <details>
        <summary>Why it works?</summary>
        
        - **Mutual Exclusion**
            - Assume two processes, $P_i$ and $P_j$, are in the critical section simultaneously.
                - This means both passed the *for* loop checks in the `RequestCS()` function.
                    
                    ```cpp
                    while (choosing[j]);
                    while (number[j] != 0 && (number[j] < number[myId] || 
                          (number[j] == number[myId] && j < myId)));
                    ```
                    
            - For both $P_i$ and $P_j$ to be in the critical section:
                - $number[j] ≥ number[i]$ (for $P_i$).
                - $number[i] ≥ number[j]$ (for $P_j$).
                - If $number[i] = number[j]$, their indices must satisfy:
                    - $i < j$ (for $P_i$).
                    - $j < i$ (for $P_j$).
            - **Contradiction:**
                - It is impossible for $P_i$ and $P_j$ to satisfy all equations above at the same time.
            - **Conclusion:**
                - $P_i$ and $P_j$ cannot be in the critical section at the same time.
                - **Mutual Exclusion** is guaranteed.
        - **Progress**
            - Assume there are k processes requesting the critical section.
                - Each process $P_i$ in the set has its `choosing[i] = false` and a valid `number[i] > 0`.
            - The process with the smallest ticket (`number[i]`) satisfies:
                - All other processes $P_j$ in the *for* loop find:
                    
                    ```cpp
                    while (number[j] != 0 && (number[j] < number[myId] || 
                          (number[j] == number[myId] && j < myId)));
                    ```
                    
                - This condition evaluates to `false` for $P_i$, allowing it to proceed.
            - Ticketing guarantees fairness:
                - Processes take tickets in increasing order (`number[myId] = 1 + max(...)`).
                - A process with the smallest ticket will not be blocked indefinitely.
            - **Conclusion**:
                - The process with the smallest ticket will enter the critical section when it becomes free.
                - **Progress** is guaranteed.
            
        - **No Starvation**
            - Assume a process $P_k$ is waiting indefinitely in the for loop:
                
                ```cpp
                while (choosing[j]);
                while (number[j] != 0 && (number[j] < number[myId] || 
                      (number[j] == number[myId] && j < myId)));
                ```
                
            - $P_k$  waits because:
                - Some other process  $P_j$  has a smaller ticket (`number[j] < number[k]`) or:
                - $P_j$ has the same ticket but higher priority ($j < k$).
            - Processes with smaller tickets:
                - Will eventually complete their critical section, set `number[j] = 0`, and no longer block $P_k$.
            - New requests:
                - Any process requesting the critical section after $P_k$ will receive a larger ticket (`number[new] > number[k]`).
                - This ensures $P_k$’s priority is preserved.
            - **Contradiction**:
                - If $P_k$ waits indefinitely, it implies an infinite loop of processes with smaller tickets, which is impossible because there are a finite number of processes.
            - **Conclusion**:
                - Every process, including $P_k$, will eventually enter the critical section.
                - **No Starvation** is guaranteed.
            </details>
3. **Hardware-Based Solutions**
    1. **Disabling Interrupts**
        - Prevent context switches during the critical section.
        - **Limitations**: Impractical for multi-processor systems and inefficient for long critical sections.
        - Why it works?
            - Ensures mutual-exclusion by disabling multitasking.
    2. **Special Machine-Level Instructions**
        - Uses a hardware-supported **atomic instructions** to test and set a variable.
        - <details>
            <summary>Pseudocode</summary>
            
            ```cpp
            bool lock = false; // Shared variable
            
            // Atomic TestAndSet function: executed atomically
            bool TestAndSet(bool &target, bool value) {
                bool oldValue = target;
                target = value;
                return oldValue;
            }
            
            void RequestCS() {
                while (TestAndSet(lock, true)) // Busy waiting
            }
            
            void ReleaseCS() {
                lock = false; // Release the lock
            }
            ```
            </details>
            
        - Why it works?
            - The atomic `TestAndSet` ensures only one process acquires lock at a time.