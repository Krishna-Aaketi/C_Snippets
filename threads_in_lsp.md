# 📘 Function in C - Notes

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
