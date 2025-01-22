# Lecture 02: Synchronization Primitives

## Synchronization Primitives

- **Why Needed**:
    - Busy waiting wastes CPU cycles.
    - Synchronization primitives (e.g., semaphores, monitors) allow processes to wait without consuming CPU resources.
    - Requires OS support and atomic execution.

## Semaphores

- **Definition**: A synchronization primitive with:
    - A boolean value (indicates the state).
    - A queue of blocked processes.
- **Operations**:
    - `P()`: Decrements the semaphore; blocks the process if value is `false`.
    - `V()`: Increments the semaphore; wakes up one waiting process if present.
    - Both operations execute atomically.
    - <details>
        <summary>Pseudocode</summary>

        ```cpp
        // Semaphore structure
        struct Semaphore {
            bool value = true; // Initial value of the semaphore
            std::queue<int> queue; // Queue to manage blocked processes
        };
        
        // P() operation (Wait)
        void P(Semaphore &sem) {
            // Executed atomically
            if (!sem.value) {
                // Add the process to the queue and block
                sem.queue.push(getProcessId());
                blockProcess(); // Simulates process blocking (implementation OS-specific)
            }
            sem.value = false; // Acquire the semaphore
        }
        
        // V() operation (Signal)
        void V(Semaphore &sem) {
            // Executed atomically
            sem.value = true; // Release the semaphore
            if (!sem.queue.empty()) {
                // Wake up one arbitrary process
                int processId = sem.queue.front();
                sem.queue.pop();
                wakeProcess(processId); // Simulates waking the process (implementation OS-specific)
            }
        }
        ```
        </details>
        
- **Usage**:
    - Solve mutual exclusion without busy waiting as only one process is waken up in `V()`:
        
        ```cpp
        void RequestCS() { P(); }
        void ReleaseCS() { V(); }
        
        ```
        
- **Example**:
    - **Dining Philosophers Problem**:
        - Five philosophers need two chopsticks each.
        - **Naive Implementation:**
            - All philosophers pick one stick and wait for the other, forming a cycle.
            - <details>
                <summary>Pseudocode</summary>
                
                ```cpp
                Semaphore chopstick[5] = {Semaphore(), Semaphore(), Semaphore(), Semaphore(), Semaphore()};
                
                void philosopher(int i) {
                    while (true) {
                        // Thinking
                        chopstick[i].P(); // Pick up left chopstick
                        chopstick[(i + 1) % 5].P(); // Pick up right chopstick
                        // Eating
                        chopstick[i].V(); // Put down left chopstick
                        chopstick[(i + 1) % 5].V(); // Put down right chopstick
                    }
                }
                ```
                </details>
                
            - **Why it fails:**
                - Deadlock occurs if all philosophers pick up their left chopstick at the same time, as no one can proceed to pick up the right chopstick.
        - **Example Solution**:
            - The philosophers alternating picking both chopsticks. This can be achieved by ask the odd-indexed philosophers to pick the right chopstick before the left and the even-indexed philosophers to pick the left chopstick before the right.
            - <details>
                <summary>Pseudocode</summary>
                
                ```cpp
                Semaphore chopstick[5] = {Semaphore(), Semaphore(), Semaphore(), Semaphore(), Semaphore()};
                
                void philosopher(int i) {
                    while (true) {
                        // Thinking
                        if (i % 2 == 0) {
                            // Even-numbered philosopher
                            chopstick[i].P(); // Pick up left chopstick
                            chopstick[(i + 1) % 5].P(); // Pick up right chopstick
                        } else {
                            // Odd-numbered philosopher
                            chopstick[(i + 1) % 5].P(); // Pick up right chopstick
                            chopstick[i].P(); // Pick up left chopstick
                        }
                        // Eating
                        chopstick[i].V(); // Put down left chopstick
                        chopstick[(i + 1) % 5].V(); // Put down right chopstick
                    }
                }
                ```
                </details>
                
## Monitors

- **Definition**:
    - High-level synchronization primitive.
    - Easier to use than semaphores.
    - Often integrated into languages like Java (via `synchronized`).
- **Semantics**:
    - Provides mutual exclusion for monitor blocks.
    - Includes operations:
        - `wait()`: Blocks the process and adds it to the wait-queue.
        - `notify()`: Wakes up one process from the wait-queue.
        - `notifyAll()`: Wakes up all processes in the wait-queue.
    - Supports atomic transitions between states.
- **Types**:
    - **Hoare-Style Monitor**
        - When a process calls notify():
            - The process currently executing inside the monitor (e.g., P1) **immediately yields** the monitor to the waiting process (e.g., P2) from the wait queue.
            - The notified process (e.g., P2) resumes its execution **exactly where it was blocked** in the monitor.
            - Once the notified process (e.g., P2) completes, the notifying process (e.g., P1) resumes its execution.
    - **Java-Style Monitor**
        - When a process calls notify():
            - The notifying process (e.g., P1) **continues its execution** inside the monitor until it exits the synchronized block.
            - The notified process (e.g., P2) must wait for the monitor to become available again.
            - After the notifying process (e.g., P1) exits, the notified process (e.g., P2) must **reacquire the monitor lock** before resuming.

## Synchronization Problems Solved with Monitors

### The Producer-Consumer Problem

- Involves:
    - A **producer** adding items to a shared buffer.
    - A **consumer** removing items from the buffer.
- **Challenges**:
    - Prevent the producer from adding items when the buffer is full.
    - Prevent the consumer from removing items when the buffer is empty.
    - Ensure no race conditions occur when accessing the shared buffer.
- **Solution**:
    - Use a **monitor** with `wait()` and `notify()` mechanisms to synchronize the producer and consumer.
    - <details>
        <summary>Pseudocode</summary>
        
        **Program**
        
        ```cpp
        #include <queue>
        #include <mutex>
        #include <condition_variable>
        
        class Monitor {
        private:
            std::queue<int> buffer; // Shared buffer
            int maxSize;            // Maximum buffer size
            std::mutex mtx;         // Mutex for synchronization
            std::condition_variable notFull, notEmpty; // Conditions for producer and consumer
        
        public:
            Monitor(int size) : maxSize(size) {}
        
            // Producer function
            void produce(int item) {
                std::unique_lock<std::mutex> lock(mtx);
        
                // Wait if the buffer is full
                notFull.wait(lock, [this]() { return buffer.size() < maxSize; });
        
                buffer.push(item); // Add item to the buffer
                notEmpty.notify_one(); // Notify a waiting consumer
            }
        
            // Consumer function
            int consume() {
                std::unique_lock<std::mutex> lock(mtx);
        
                // Wait if the buffer is empty
                notEmpty.wait(lock, [this]() { return !buffer.empty(); });
        
                int item = buffer.front(); // Remove item from the buffer
                buffer.pop();
                notFull.notify_one(); // Notify a waiting producer
                return item;
            }
        };
        ```
        
        **Example Usage**
        
        ```cpp
        #include <thread>
        #include <iostream>
        #include <chrono>
        
        void producer(Monitor &monitor, int itemCount) {
            for (int i = 0; i < itemCount; ++i) {
                monitor.produce(i);
                std::cout << "Produced: " << i << "\n";
                std::this_thread::sleep_for(std::chrono::milliseconds(100));
            }
        }
        
        void consumer(Monitor &monitor, int itemCount) {
            for (int i = 0; i < itemCount; ++i) {
                int item = monitor.consume();
                std::cout << "Consumed: " << item << "\n";
                std::this_thread::sleep_for(std::chrono::milliseconds(150));
            }
        }
        
        int main() {
            Monitor monitor(5); // Buffer size = 5
            const int itemCount = 10;
        
            // Create producer and consumer threads
            std::thread producerThread(producer, std::ref(monitor), itemCount);
            std::thread consumerThread(consumer, std::ref(monitor), itemCount);
        
            producerThread.join();
            consumerThread.join();
        
            return 0;
        }
        ```
        </details>

### The Reader-Writer Problem

- **Concept:**
    - Multiple **readers** can access the resource (e.g., file) simultaneously.
    - Only one **writer** can access the resource at a time.
- **Rules**:
    - A writer must have exclusive access (no readers or other writers).
    - Multiple readers can access the resource concurrently.
- **Solution**:
    - Readers wait if a writer is active, while writers wait until no readers or other writers are active, ensuring proper synchronization using condition variables and a mutex.
    - <details>
        <summary>Pseudocode</summary>
        
        ```cpp
        #include <mutex>
        #include <condition_variable>
        
        class ReaderWriterMonitor {
        private:
            int numReaders = 0; // Count of active readers
            bool writerActive = false; // Indicates if a writer is active
            std::mutex mtx; // Mutex for synchronization
            std::condition_variable canAccess; // Condition variable for readers and writers
        
        public:
            // Reader enters the critical section
            void startRead() {
                std::unique_lock<std::mutex> lock(mtx);
                canAccess.wait(lock, [this]() { return !writerActive; }); // Wait if a writer is active
                numReaders++; // Increment active readers
            }
        
            // Reader exits the critical section
            void endRead() {
                std::unique_lock<std::mutex> lock(mtx);
                numReaders--; // Decrement active readers
                if (numReaders == 0) {
                    canAccess.notify_one(); // Notify a waiting writer
                }
            }
        
            // Writer enters the critical section
            void startWrite() {
                std::unique_lock<std::mutex> lock(mtx);
                canAccess.wait(lock, [this]() { return !writerActive && numReaders == 0; }); // Wait if readers or writers are active
                writerActive = true; // Mark writer as active
            }
        
            // Writer exits the critical section
            void endWrite() {
                std::unique_lock<std::mutex> lock(mtx);
                writerActive = false; // Mark writer as inactive
                canAccess.notify_all(); // Notify all waiting readers and writers
            }
        };
        ```
        </details>
