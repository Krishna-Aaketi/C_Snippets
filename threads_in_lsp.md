# Function in C - Notes

## Function - Simple Definition

A **function** is a block of code that performs a specific task. It helps reduce code repetition, improves readability, and makes the program easier to manage.

---

## C Program: Add, Subtract, Multiply, Divide (main first)

```c
#include <stdio.h>

// Function declarations (prototypes)
int add(int a, int b);
int subtract(int a, int b);
int multiply(int a, int b);
float divide(float a, float b);

int main() {
    int num1 = 10, num2 = 5;

    printf("Addition: %d\n", add(num1, num2));
    printf("Subtraction: %d\n", subtract(num1, num2));
    printf("Multiplication: %d\n", multiply(num1, num2));
    printf("Division: %.2f\n", divide(num1, num2));

    return 0;
}

// Function definitions

int add(int a, int b)
{
    printf("Add function execution Started..\n");
    return a + b;
}

int subtract(int a, int b)
{
    printf("Subtract function execution Started..\n");
    return a - b;
}

int multiply(int a, int b)
{
    printf("Multiply function execution Started..\n");
    return a * b;
}

float divide(float a, float b)
{
    printf("Division function execution Started..\n");
    if (b == 0) {
        printf("Error: Division by zero!\n");
        return 0;
    }
    return a / b;
}
```

---

## What Happens When a Function is Executed (System-Level View)

When a function is executed, the **CPU** and **Operating System (OS)** coordinate to:

- Allocate memory (stack frame)
- Pass control to the function code
- Execute instructions line by line
- Return control to the caller after execution

But where this happens—**user space** vs. **kernel space**—makes a big difference.

---

## Two Main Areas of Execution in a System

| Space            | Description                      | Who Runs Here?                 |
| ---------------- | -------------------------------- | ------------------------------ |
| **User Space**   | Where user applications run      | Your programs (`main()`, etc.) |
| **Kernel Space** | Core of the OS; manages hardware | OS code, device drivers        |

---

## Function Execution Flow (In User Space)

1. Your program starts in `main()`.
2. When a function is called:
   - The address of the next instruction is saved.
   - Arguments are pushed to the **stack** or **registers**.
   - The CPU jumps to the function code.
   - Function executes and may return a value.
   - The return value goes to the calling function (like `main()`).

 All of this happens **inside user space** unless the function uses OS services.

---

## When Kernel Space is Involved

You **leave user space and enter kernel space** if the function performs:

- `printf()` → internally uses `write()`
- `scanf()`  → internally uses `read()`
- `malloc()` → uses memory management system calls (`brk()`, `mmap()`)

This transition is done via **system calls**, which act as **gates** into kernel space.

---

## Example: `printf("Hello")`

1. You call `printf()` → part of user space (libc).
2. `printf()` uses `write()` system call → enters kernel space.
3. Kernel sends output to terminal (hardware).
4. Returns to user space and resumes your code.

---
## The C Standard Library (libc)

The C Standard Library (`libc`) is a critical part of any C program's runtime. Here's a breakdown of where it is present and how it works:

---

### Where is `libc` Present?

#### Conceptually:
`libc` is a collection of precompiled functions written in C (and some assembly) that provide standard functionality like:

- `printf`, `scanf`, `malloc`, `free`, `exit`, `write`, `read`, etc.

#### Physically (on Linux systems):

| Location                               | Description                                      |
|----------------------------------------|--------------------------------------------------|
| `/lib/x86_64-linux-gnu/libc.so.6`      | Shared dynamic library used by most programs    |
| `/usr/include/stdio.h`, etc.           | Header files for compiling (not actual code)    |
| `/usr/lib/.../libc.a`                  | Static version of libc for static linking       |

---

### During Compilation and Execution

- **Compilation**:  
  When you run `gcc myprog.c`, the compiler uses header files like `#include <stdio.h>` to reference standard functions.

- **Linking**:  
  By default, `gcc` links dynamically with `libc.so` (shared library).

- **Runtime**:  
  When your program runs, the dynamic linker (`ld-linux`) loads `libc.so.6` into memory and your program uses it to execute standard functions.

# Thread in C - Notes

## Thread - Simple Definition

A **thread** is the smallest unit of execution in a process. Threads share the same memory space but execute independently. Multiple threads in a program can run concurrently.

---

## C Program: Add, Subtract, Multiply, Divide Using Threads (main first)

```c
#include <stdio.h>
#include <pthread.h>

// Input numbers (global for simplicity)
int num1 = 10, num2 = 5;

// Thread function declarations
void* add(void* arg);
void* subtract(void* arg);
void* multiply(void* arg);
void* divide(void* arg);

int main() {
    pthread_t t1, t2, t3, t4;

    // Create threads for each operation
    pthread_create(&t1, NULL, add, NULL);
    pthread_create(&t2, NULL, subtract, NULL);
    pthread_create(&t3, NULL, multiply, NULL);
    pthread_create(&t4, NULL, divide, NULL);

    // Wait for all threads to finish
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    pthread_join(t3, NULL);
    pthread_join(t4, NULL);

    return 0;
}

// Thread function definitions

void* add(void* arg) {
    printf("Addition: %d\n", num1 + num2);
    return NULL;
}

void* subtract(void* arg) {
    printf("Subtraction: %d\n", num1 - num2);
    return NULL;
}

void* multiply(void* arg) {
    printf("Multiplication: %d\n", num1 * num2);
    return NULL;
}

void* divide(void* arg) {
    if (num2 != 0)
        printf("Division: %.2f\n", (float)num1 / num2);
    else
        printf("Error: Division by zero!\n");
    return NULL;
}
```

---

## What Happens When a Thread is Created (System-Level View)

When a thread is created using `pthread_create()`:

- A new thread control block (TCB) is created in memory.
- Thread shares the same data segment, heap, and file descriptors.
- A new stack is allocated for the thread.
- CPU registers are set up for thread context.

Execution continues concurrently with other threads.

---

## Memory Segments in Multithreading

| Segment   | Shared Between Threads? | Description                    |
| --------- | ----------------------- | ------------------------------ |
| **Text**  |   Yes                   | Binary code (instructions)     |
| **Data**  |   Yes                   | Global/static initialized data |
| **Heap**  |   Yes                   | Dynamic memory (`malloc`)      |
| **Stack** |   No                    | Each thread has its own stack  |

---

## Kernel Role in Threading

- Threads are managed by the **kernel scheduler** (in Linux: `NPTL` – Native POSIX Thread Library).
- Thread creation may involve system calls (e.g., `clone()`, `futex`).
- Context switching between threads can happen based on priority/time slice.

---

### Example: No pthread_join() Used
```c
#include <stdio.h>
#include <pthread.h>

void* print_message(void* arg);

int main()
{
    pthread_t t1, t2;

    pthread_create(&t1, NULL, print_message, "Thread 1");
    pthread_create(&t2, NULL, print_message, "Thread 2");

    printf("Main thread exiting...\n");

    //No pthread_join() → main may exit before threads complete
    return 0;
}

void* print_message(void* arg)
{
    char* msg = (char*)arg;
    for (int i = 0; i < 5; i++)
    {
        printf("%s: %d\n", msg, i);
    }
    return NULL;
}

```
### Return String from Thread (Main Function First)

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <string.h>

int main() {
    pthread_t t;
    char* result;

    pthread_create(&t, NULL, NULL, NULL); // Will replace NULL after function definition
    pthread_create(&t, NULL, return_string, NULL);  // Corrected call

    pthread_join(t, (void**)&result);  // Capture the return value

    printf("Main thread received: %s\n", result);

    free(result);  // Clean up

    return 0;
}

// Thread function placed AFTER main
void* return_string(void* arg) {
    char* msg = malloc(50);
    if (msg == NULL) {
        perror("malloc failed");
        pthread_exit(NULL);
    }

    strcpy(msg, "Hello from thread!");
    return (void*)msg;
}
```
### Pass Integer to Thread and Return Result (Main First)

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

int main() {
    pthread_t thread;
    int input = 7;

    // Create thread and pass address of input
    pthread_create(&thread, NULL, square_thread, &input);

    // Wait for thread and collect result
    int* output;
    pthread_join(thread, (void**)&output);

    printf("Square of %d is %d\n", input, *output);

    free(output);  // Free the memory allocated in thread
    return 0;
}

// Thread function defined after main
void* square_thread(void* arg) {
    int num = *(int*)arg;
    int* result = malloc(sizeof(int));
    *result = num * num;
    return result;
}
```
### Pass Two Integers to a Thread and Return Result (Main First)
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

// Structure to hold two input values
struct input_data {
    int a;
    int b;
};

int main() {
    pthread_t thread;
    struct input_data values = {10, 20};

    // Create thread, pass address of struct
    pthread_create(&thread, NULL, add_thread, &values);

    // Wait for result from thread
    int* result;
    pthread_join(thread, (void**)&result);

    printf("Sum: %d + %d = %d\n", values.a, values.b, *result);

    free(result); // Don't forget to free memory
    return 0;
}

// Thread function defined after main
void* add_thread(void* arg) {
    struct input_data* data = (struct input_data*)arg;
    int* sum = malloc(sizeof(int));
    *sum = data->a + data->b;
    return sum;
}
```
### Using exit() in a Thread

### If a thread calls exit(), it will:
- Terminate the entire process, not just the thread.
- All threads will be forcibly stopped.
- No cleanup of other threads will happen.

This is very different from pthread_exit(), which safely terminates only the calling thread.

### Example: exit() inside a Thread
Below is an example showing what happens when a thread calls exit():

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void* thread_func(void* arg);

int main() {
    pthread_t t1;

    printf("Main: Creating thread...\n");
    pthread_create(&t1, NULL, thread_func, NULL);

    printf("Main: Sleeping...\n");
    sleep(2);

    printf("Main: This line may not execute if thread calls exit().\n");

    pthread_join(t1, NULL);  // May not reach here
    printf("Main: Thread joined.\n");

    return 0;
}

// Thread defined after main
void* thread_func(void* arg) {
    printf("Thread: Calling exit().\n");
    exit(0); // Terminates entire process
    return NULL; // Never reached
}
```
