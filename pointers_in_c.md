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
## 🔹 Common Interview Snippets

### Q1: Swap two numbers using pointers

```c
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}
```

### Q2: Reverse array using pointers

```c
void reverse(int *arr, int size) {
    int *start = arr;
    int *end = arr + size - 1;
    while(start < end) {
        int temp = *start;
        *start = *end;
        *end = temp;
        start++;
        end--;
    }
}
```

### Q3: Accessing 2D array with pointers

```c
int matrix[2][2] = {{1,2},{3,4}};
int *ptr = &matrix[0][0];
printf("%d", *(ptr + 2)); // prints 3
```

---

## 🔹 Tips

- Always initialize pointers.
- Be careful with pointer arithmetic.
- Use `const` with pointers when data should not be modified.
- Avoid memory leaks by freeing allocated memory.

---

## 🔹 Pointer Questions

1. What is the size of a pointer on a 32-bit/64-bit system?
2. What happens if you dereference a NULL pointer?
3. Explain pointer to pointer.
4. What is the difference between `const int *ptr`, `int *const ptr`, and `const int *const ptr`?
5. How are arrays and pointers related in C?

---

> ✅ Practice with small snippets daily to master pointers. Let me know if you want visuals or quiz questions!

