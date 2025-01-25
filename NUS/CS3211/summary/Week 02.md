# Week 2: Tasks and Threads (in Modern C++)

## Program Execution

### Decomposition

- Break the problem into smaller, independent tasks or computations that can run concurrently.

### Assigning

- Distribute these tasks to available threads for parallel execution.

### Orchestration

- Structuring communication.
- Adding synchronization to preserve dependencies.
- Organizing data structures in memory.
- Scheduling tasks.

### Mapping

- Mapping from threads to hardware core.
- Mostly mapped by Operating System (OS).

## Distributing Work to Threads

### Task Paralelism

- Divide the work into **tasks**.
    - Divide the work by task type to separate concern.
    - Same type of tasks assigned to the same thread.
- Dividing a sequence of tasks among threads to achieve complex solutions.
    - Pipeline: Each thread is responsible for a stage in a pipeline.
- Use task pool and the number of tasks to serve the task pool.

### Data Paralelism

- Dividing data in the same type of task to be assigned into tasks.

## C++ History

### C++98

- Does not acknowledge the existence of threads.
- Effects of language elements assume a sequential abstract machine.
- Multithreading was dependent on compiler-specific extensions (library).
    - C APIs, such as POSIX C standard and Microsoft Windows API, used in C++.
    - Very few formal multithreading-aware memory models provided by compiler vendors.
    - Application framework, such as Boost and ACE, wrap the underlying platform-specific APIs.

### C++11 Multithreading

- Write portable multithreaded code with guaranteed behavior.
- Thread-aware memory model.
- Includes classes for managing threads, protecting shared data, synchronization between threads, low-level atomic operations.
- Use of concurrency to **improve application performance**.

## Creating and Managing Thread in C++

### Creating Thread with a Function

1. **Function**
    
    ```cpp
    #include <iostream>
    #include <thread>
    
    // Function to be run by the thread
    void printMessage(const std::string& message) {
        std::cout << "Message from thread: " << message << std::endl;
    }
    
    int main() {
        // Create a thread that executes the function
        std::thread t(printMessage, "Hello from a thread!");
    
        // Wait for the thread to finish
        t.join();
    
        return 0;
      }
    ```
    
    - `std::thread` is used to create a new thread.
    - The thread executes the function `printMessage` with the argument `"Hello from a thread!"`.
    - `t.join()` ensures the main thread waits for the child thread to finish.
2. **Function Object**
    
    ```cpp
    class background_task {
    	public:
    		void operator()() const {
    			do_something();
    			do_something_else();
    		}
    };
    background_task f;
    std:thread my_thread(f);
    ```
    
    - `std::thread my_thread(background_task());` will not start or initialize a new thread. It will be treated by the compiler as a function declaration named `my_tread` that returns an object of type `std::thread` and takes a single parameter, which is a pointer to a function that takes no arguments and return an object of type `background_task`.
    - Alternative: `std::thread my_thread((background_task()));` and `std:thread my_thread{background_task()};` ({} is more recommended) are the way to create a new thread with a function object.
    - Callable object is needed to maintain a state information.

### Creating Thread with a Lambda Expression

```cpp
std::thread my_thread([] {
	do_something();
	do_something_else();
});
```

- A lambda function is used as the task for the thread.

### Waiting a Thread

- Use `join()` on the thread instance, exactly once.
- Use `joinable()` to check that a thread can be joined.
- Make sure to join the thread even when there is an exception.
    
    ```cpp
    #include <iostream>
    #include <thread>
    #include <chrono>
    
    // Function to be executed by the thread
    void performTask() {
        std::cout << "Task started...\n";
        std::this_thread::sleep_for(std::chrono::seconds(3)); // Simulate work
        std::cout << "Task finished!\n";
    }
    
    int main() {
        // Create a thread
        std::thread t(performTask);
    
        // Wait for the thread to complete
        std::cout << "Waiting for the thread to finish...\n";
        t.join(); // Block until thread finishes
    
        std::cout << "Main thread resumes after the task is done.\n";
        return 0;
    }
    ```
    

### Detaching a Thread

- Use `detach()`.
- The new created thread will run independently.
- Once detached, a thread is not joinable.
- Extra care is needed with local variable passed to the thread.
- We need to ensure that the thread finishes safely, especially if it accesses shared resources.
    
    ```cpp
    #include <iostream>
    #include <thread>
    #include <chrono>
    
    void backgroundTask() {
        std::this_thread::sleep_for(std::chrono::seconds(2));
        std::cout << "Background task completed!" << std::endl;
    }
    
    int main() {
        std::thread t(backgroundTask);
    
        // Detach the thread to run independently
        t.detach();
    
        std::cout << "Main thread continues..." << std::endl;
    
        // Give enough time for the detached thread to finish
        std::this_thread::sleep_for(std::chrono::seconds(3));
        return 0;
    }
    ```
    
- <details>
    <summary>Possible Issue</summary>
    
    ```cpp
    #include <thread>
    
    void do_something(int& i) {
        ++i;
    }
    
    struct func {
        int& i;
    
        func(int& i_) : i(i_) {}
    
        void operator()() {
            for (unsigned j = 0; j < 1000000; ++j) {
                do_something(i);
            }
        }
    };
    
    void oops() {
        int some_local_state = 0;
        func my_func(some_local_state);
        std::thread my_thread(my_func); // Start a thread with my_func
        my_thread.detach();             // Detach the thread
    }
    
    int main() {
        oops();
        return 0;
    }
    ```
    
    **What happened?**
    
    - In the function `oops()`, a thread is created with the callable object `my_func` and immediately detached.
    - The detached thread operates on a reference to the local variable `some_local_state`.
    - Once `oops()` exits, the local variable `some_local_state` goes out of scope and is destroyed.
    - The detached thread continues execution and tries to access the now-destroyed variable, leading to **undefined behavior**.
    
    **Why is This a Problem?**
    
    - The detached thread does not synchronize with the main thread, and there’s no guarantee the referenced variable (`some_local_state`) will be valid by the time the detached thread accesses it.
    - This could lead to:
        - Segmentation faults.
        - Corrupted data.
        - Unpredictable program behavior.
    </details>

### Passing Arguments to a Thread

1. **Passing a function and arguments by value**
    
    ```cpp
    void f(int i, std::string const& s);
    std::thread t(f, 3, "hello");
    ```
    
    - If copy of the argument is passed, modification in the thread will not update the value of the original variable (in the parent thread).
    - <details>
        <summary>Possible Issue</summary>
        
        ```cpp
        void f(int i, std::string const& s);
        void oops(int some_param) {
            char buffer[1024];
            sprintf(buffer, "%i", some_param);
            std::thread t(f, 3, buffer);
            t.detach();
        }
        ```
        
        - buffer is a local array storing a formatted string.
        - A thread `t` is created to run `f`, passing `buffer` as a reference.
        - **The Issue**:
            - The function `oops()` might finish and destroy `buffer` before the detached thread starts or completes its execution.
            - This causes **undefined behavior** because the detached thread accesses a dangling reference.
        - <details>
            <summary><b>Solution</b>:</summary>
            
            ```cpp
            void f(int i, std::string const& s);
            void not_oops(int some_param) {
                char buffer[1024];
                sprintf(buffer, "%i", some_param);
                std::thread t(f, 3, std::string(buffer));
                t.detach();
            }
            ```
            
            - Instead of passing `buffer` directly, it is explicitly converted to an `std::string` before being passed to the thread.
            - The `std::string` created within the `not_oops` function is **copied** into the thread, ensuring the thread has a valid and independent copy even if `not_oops()` exits.
            </details>
        </details>
2. **Passing arguments by reference**
    
    ```cpp
    void update_data_for_widget(widget_id w, widget_data& data);
    
    void oops_again(widget_id w) {
        widget_data data;
        // std::thread t(update_data_for_widget, w, data); // Passes `data` by value (copy)
        std::thread t(update_data_for_widget, w, std::ref(data)); // Pass by reference
        display_status();
        t.join(); // Wait for thread to finish
        process_widget_data(data);
    }
    ```
    
    - Wrapping data in `std::ref` ensures the original object is passed **by reference** to the thread function instead of creating a copy.
    - This allows the thread to modify the original data object, and the changes will be visible after `t.join()`.

## Lifetime

- The lifetime of an object begins when:
    - storage with the proper alignment and size for its type is obtained, and
    - its initialization (if any) is complete (including default initialization via no constructor or trivial default constructor) (except that of union and allocations by `std::allocator::allocate`)
- The lifetime of an object ends when:
    - if it is of a non-class type, the object is destroyed (maybe via a pseudo-destructor call), or
    - if it is of a class type, the destructor call starts, or
    - the storage which the object occupies is released, or is reused by an object that is not nested within it.
- Lifetime of an object is equal to or is nested within the lifetime of its storage.
- The lifetime of a reference begins when its initialization is complete and ends as if it were a scalar object.
    - Note: the lifetime of the referred object may end before the end of the lifetime of the reference, which makes dangling references possible.

## Ownership in C++

- An **owner** is an object containing a pointer to an object allocated by `new` for which a `delete` is required
- Every object on the free store (heap, dynamic store) must have exactly one owner (this rule is not enforced, but advisable)
- C++’s model of resource management is based on the use of constructors and destructors
    - For scoped objects, destruction is implicit at scope exit
    - For objects placed in the free store (heap, dynamic memory) using `new`, `delete` is required.

## RAII - Resource Acquisition Is Initialization

- RAII is a C++ programming technique that ties the **lifecycle of a resource** (e.g., memory, file handles, locks) to the **lifetime of an object**. It ensures that resources are properly released when the owning object goes out of scope.
- Resources are acquired during object creation.
- Resources are automatically released when the object is destroyed.

## Ownership of a thread

- `std::thread` instances own a resource.
- Instances of `std::thread` are *movable* but not *copyable*.
- <details>
    <summary>Cannot be moved into another <code>std::thread</code> instance that already own a resource.</summary>
    
    ```cpp
    void some_function();
    void some_other_function();
    std::thread t1(some_function);
    std::thread t2 = std::move(t1); // t1 owns nothing and t2 owns the thread managing some_function
    t1 = std::thread(some_other_function)
    std:thread t3;
    t3 = std::move(t2); // t2 empty and t3 owns the thread managing some_function
    t1 = std::move(t3); // fail since t1 have already owned the thread managing some_other_function
    ```
    
    ```cpp
    void some_function();
    void some_other_function();
    std::thread t1(some_function);
    std::thread t2 = std::move(t1);
    t1 = std::thread(some_other_function)
    std:thread t3;
    t3 = std::move(t2);
    t1.detach(); // t1 becomes empty
    t1 = std::move(t3); // successful
    ```
    </details>
    

## Transferring Ownership of a Thread

### Transfer ownership of thread out of a function

```cpp
std::thread f() {
    void some_function();
    return std::thread(some_function);
}

std::thread g() {
    void some_other_function(int);
    std::thread t(some_other_function, 42);
    return t;
}
```

### Transfer ownership of thread into a function

```cpp
void f(std::thread t);
void g() {
    void some_function();
    f(std::thread(some_function));          // Creates and transfers ownership of a thread to f()
    std::thread t(some_function);           // Creates a thread
    f(std::move(t));                        // Transfers ownership of t to f()
}
```

## Synchronizing Multiple Threads

### Synchronizing Concurrent Accesses

- Programs must be designed to ensure that changes to a chosen data structure are correctly synchronized among threads.
    - Data structure are immutable, or
    - Protect data using some locking mechanism, externally (using mutex) or internally (using atomic).