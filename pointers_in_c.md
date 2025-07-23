# Pointers in C

## 🔹 What is a Pointer?

A pointer is a variable that stores the memory address of another variable.

```c
int a = 10;
int *p = &a; // 'p' holds the address of variable 'a'
```

---

## 🔹 Why Use Pointers?

- To manipulate data using address references
- Dynamic memory allocation
- Efficient array and string handling
- For passing large structures or arrays to functions
- Used in data structures like linked lists, trees, etc.

---

## 🔹 Pointer Declaration Syntax

```c
int *ptr;  // pointer to an integer
char *cptr; // pointer to a character
float *fptr; // pointer to a float
```

---

## 🔹 Types of Pointers

### 1. **Null Pointer**

A pointer that doesn't point to any memory location.

```c
int *p = NULL;
```

### 2. **Void Pointer**

A generic pointer that can point to any data type.

```c
void *ptr;
int a = 10;
ptr = &a;
```

### 3. **Wild Pointer**

Uninitialized pointer that may point to unknown memory.

```c
int *p; // dangerous!
```

### 4. **Dangling Pointer**

Pointer pointing to freed memory.

```c
int *ptr = malloc(sizeof(int));
free(ptr);  // ptr is now dangling
```

### 5. **Function Pointer**

Used to point to a function.

```c
void greet() {
    printf("Hello!\n");
}
void (*fptr)() = greet;
fptr();
```

### 6. **Double Pointer**

Pointer to a pointer.

```c
int a = 5;
int *p = &a;
int **pp = &p;
```

---

## 🔹 Pointer Arithmetic

Pointers can be incremented or decremented to point to the next or previous memory location.

```c
int arr[3] = {10, 20, 30};
int *ptr = arr;
printf("%d", *(ptr + 1)); // prints 20
```

---

## 1. `char *` — Character Pointer

### Passing to a function:

```c
void printChar(char *ch) {
    printf("Char = %c\n", *ch);
}
```

### Returning from a function:

```c
char* getCharPointer(char *ch) {
    return ch;
}
```

---

## 2. `short *` — Short Integer Pointer

### Passing to a function:

```c
void printShort(short *s) {
    printf("Short = %d\n", *s);
}
```

### Returning from a function:

```c
short* getShortPointer(short *s) {
    return s;
}
```

---

## 3. `int *` — Integer Pointer

### Passing to a function:

```c
void printInt(int *p) {
    printf("Int = %d\n", *p);
}
```

### Returning from a function:

```c
int* getIntPointer(int *p) {
    return p;
}
```

---

## 4. `float *` — Float Pointer

### Passing to a function:

```c
void printFloat(float *f) {
    printf("Float = %.2f\n", *f);
}
```

### Returning from a function:

```c
float* getFloatPointer(float *f) {
    return f;
}
```

---

## 5. `long *` — Long Integer Pointer

### Passing to a function:

```c
void printLong(long *l) {
    printf("Long = %ld\n", *l);
}
```

### Returning from a function:

```c
long* getLongPointer(long *l) {
    return l;
}
```

---

## 6. `double *` — Double Precision Pointer

### Passing to a function:

```c
void printDouble(double *d) {
    printf("Double = %.2lf\n", *d);
}
```

### Returning from a function:

```c
double* getDoublePointer(double *d) {
    return d;
}
```
---

# Real-Time Applications of Pointers in C

## 1. Dynamic Memory Allocation

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n;
    printf("Enter number of students: ");
    scanf("%d", &n);

    int *marks = (int *)malloc(n * sizeof(int));
    if (!marks) {
        printf("Memory allocation failed\n");
        return 1;
    }

    for (int i = 0; i < n; i++) {
        printf("Enter marks for student %d: ", i + 1);
        scanf("%d", &marks[i]);
    }

    for (int i = 0; i < n; i++) {
        printf("Student %d: %d\n", i + 1, marks[i]);
    }

    free(marks);
    return 0;
}
```

## 2. Arrays and Strings Handling

```c
#include <stdio.h>

void printArray(int *arr, int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", *(arr + i));
    }
    printf("\n");
}

int main() {
    int data[] = {10, 20, 30, 40};
    printArray(data, 4);
    return 0;
}
```

## 3. Function Arguments (Call by Reference)

```c
#include <stdio.h>

void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 5, y = 10;
    swap(&x, &y);
    printf("After swap: x=%d, y=%d\n", x, y);
    return 0;
}
```

## 4. Data Structures (Linked List Example)

```c
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *next;
};

int main() {
    struct Node *head = (struct Node *)malloc(sizeof(struct Node));
    head->data = 100;
    head->next = NULL;

    printf("Node data = %d\n", head->data);
    free(head);
    return 0;
}
```

## 5. File Handling

```c
#include <stdio.h>

int main() {
    FILE *fp = fopen("sample.txt", "r");
    if (fp == NULL) {
        printf("File not found.\n");
        return 1;
    }

    char ch;
    while ((ch = fgetc(fp)) != EOF) {
        putchar(ch);
    }

    fclose(fp);
    return 0;
}
```

## 6. Hardware Access / Embedded Systems

```c
#define GPIO_BASE_ADDR 0x40021000
#define GPIO_OUTPUT    (*(volatile unsigned int *)(GPIO_BASE_ADDR))

int main() {
    GPIO_OUTPUT = 0x01;  // Set GPIO pin high
    return 0;
}
```

## 7. Function Pointers

```c
#include <stdio.h>

void greet() { printf("Hello!\n"); }
void bye()   { printf("Goodbye!\n"); }

int main() {
    void (*func_ptr)();

    func_ptr = greet;
    func_ptr();

    func_ptr = bye;
    func_ptr();

    return 0;
}
```

## 8. Buffer Management in Networks

```c
#include <stdio.h>
#include <string.h>

int main() {
    char buffer[1024];
    char *ptr = buffer;

    strcpy(ptr, "GET /index.html HTTP/1.1\r\nHost: example.com\r\n\r\n");

    printf("HTTP Request:\n%s", buffer);
    return 0;
}
```

## 9. Memory-Mapped I/O

```c
#include <stdint.h>

#define DEVICE_REG_ADDR 0xFF200000
volatile uint32_t *device_reg = (uint32_t *)DEVICE_REG_ADDR;

int main() {
    *device_reg = 0xABCD1234;  // Send command/data to device
    return 0;
}
```

## 10. Shared Memory in IPC

```c
#include <stdio.h>
#include <string.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/types.h>

int main() {
    int shmid = shmget(IPC_PRIVATE, 1024, IPC_CREAT | 0666);
    if (shmid == -1) {
        perror("shmget");
        return 1;
    }

    char *shm_ptr = (char *)shmat(shmid, NULL, 0);
    if (shm_ptr == (char *)(-1)) {
        perror("shmat");
        return 1;
    }

    strcpy(shm_ptr, "Hello from shared memory!");

    printf("Message written: %s\n", shm_ptr);
    return 0;
}
```

---

## Pointer Guess-the-Output Snippets (C)

## 🔹 1.
```c
int a = 10;
int *p = &a;
*p = *p + 1;
printf("%d\n", a);
```

## 🔹 2.
```c
int a = 5;
int *p = &a;
int **q = &p;
**q = 10;
printf("%d\n", a);
```

## 🔹 3.
```c
int a = 100;
int *p = &a;
int *q = p;
*p = 50;
printf("%d %d\n", a, *q);
```

## 🔹 4.
```c
int a = 20;
int *p = &a;
(*p)++;
printf("%d\n", a);
```

## 🔹 5.
```c
int a = 5;
int *p = &a;
int b = (*p)++;
printf("%d %d\n", a, b);
```

## 🔹 6.
```c
int a = 5;
int *p = &a;
printf("%p %d\n", p, *p);
```

## 🔹 7.
```c
int arr[] = {1, 2, 3, 4};
int *p = arr;
printf("%d %d\n", *(p+1), p[2]);
```

## 🔹 8.
```c
int a[] = {10, 20, 30};
int *p = a;
printf("%d\n", *(++p));
```

## 🔹 9.
```c
char str[] = "Hello";
char *p = str;
printf("%c\n", *(p + 1));
```

## 🔹 10.
```c
char *p = "Hello";
p++;
printf("%c\n", *p);
```

## 🔹 11.
```c
int a = 5;
int *p = &a;
*p = *p * *p;
printf("%d\n", *p);
```

## 🔹 12.
```c
int x = 100;
int *ptr = &x;
*ptr += 10;
printf("%d\n", x);
```

## 🔹 13.
```c
int a = 5;
int *p = &a;
*p = ++(*p);
printf("%d\n", a);
```

## 🔹 14.
```c
int arr[] = {10, 20, 30};
int *p = arr + 1;
printf("%d\n", *(p - 1));
```

## 🔹 15.
```c
int x = 5;
int *p = &x;
int y = *p++ + 1;
printf("%d %d\n", y, x);
```

## 🔹 16.
```c
int a = 10;
int *p = &a;
int **q = &p;
int ***r = &q;
***r = 25;
printf("%d\n", a);
```

## 🔹 17.
```c
char str[] = "Test";
char *p = str;
printf("%s\n", p + 1);
```

## 🔹 18.
```c
int a = 10;
int *p = &a;
int b = (*p)++ + a;
printf("%d\n", b);
```

## 🔹 19.
```c
int a = 2;
int *p = &a;
int b = (*p)++ + ++(*p);
printf("%d %d\n", a, b);
```

## 🔹 20.
```c
int x = 1;
int *p = &x;
x = x++ + ++(*p);
printf("%d\n", x);
```

## 🔹 21.
```c
int a = 5;
int *p = &a;
int b = *p++ + *p;
printf("%d\n", b);
```

## 🔹 22.
```c
char *str = "ABCDEF";
printf("%c\n", *(str + 3));
```

## 🔹 23.
```c
int a[] = {1, 2, 3, 4};
int *p = a;
printf("%d\n", *(p + *(p+1)));
```

## 🔹 24.
```c
int x = 5;
int *p = &x;
*p = *p / 2;
printf("%d\n", x);
```

## 🔹 25.
```c
int arr[] = {5, 10, 15};
int *p = arr;
printf("%d\n", *(p++));
printf("%d\n", *p);
```

## 🔹 26.
```c
int a = 10;
int b = 20;
int *p = &a;
*p = b;
printf("%d %d\n", a, b);
```

## 🔹 27.
```c
int a = 10;
int *p = &a;
int **q = &p;
*p += **q;
printf("%d\n", a);
```

## 🔹 28.
```c
int a = 100;
int *p1 = &a;
int *p2 = p1;
*p2 = 50;
printf("%d\n", a);
```

## 🔹 29.
```c
char ch = 'A';
char *p = &ch;
*p += 1;
printf("%c\n", *p);
```

## 🔹 30.
```c
int a = 3, b = 4;
int *p = &a, *q = &b;
*p = *q;
printf("%d %d\n", a, b);
```

## 🔹 31.
```c
char s[] = "abc";
char *p = s;
while(*p)
    printf("%c ", *p++);
```

## 🔹 Pointer Questions

1. What is the size of a pointer on a 32-bit/64-bit system?
2. What happens if you dereference a NULL pointer?
3. Explain pointer to pointer.
4. What is the difference between `const int *ptr`, `int *const ptr`, and `const int *const ptr`?
5. How are arrays and pointers related in C?

---

> ✅ Practice with small snippets daily to master pointers. Let me know if you want visuals or quiz questions!

