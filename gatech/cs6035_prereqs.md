# CS 6035 Pre-requesites Gameplan

## Computer Organization and Architecture

### **1. Stack and Heap (High-Level Memory Organization)**
- **Stack:** Memory region used for static memory allocation. Holds function calls, local variables, and control flow (e.g., return addresses). It follows LIFO (Last In, First Out) order.
- **Heap:** Memory region for dynamic allocation. The programmer controls when to allocate and free memory. It grows dynamically, unlike the stack.
  - Example: `int* ptr = malloc(sizeof(int));` in C allocates memory on the heap.
  - Stack vs Heap: Stack is faster but limited in size; heap is slower but larger.

- **Resources:**
  - [Article: Memory Layout of C Programs](https://aticleworld.com/memory-layout-of-c-program/)
  - [YouTube: Stack vs Heap Memory](https://www.youtube.com/watch?v=5OJRqkYbK-4)

---

### **2. Registers, Virtual Memory, and Processes**
- **Registers:** Small, fast storage in the CPU for immediate values and instructions.
  - Examples: Program Counter (PC), Stack Pointer (SP), Base Pointer (BP).
- **Virtual Memory:** An abstraction layer for managing memory, allowing processes to believe they have their own contiguous address space. Uses techniques like paging to map virtual to physical addresses.
- **Processes:** Instances of a program in execution. Each has its own memory space (stack, heap, code, and data segments).

- **Resources:**
  - [Registers and Memory Basics](https://www.geeksforgeeks.org/register-memory/)
  - [Operating Systems: Virtual Memory](https://www.geeksforgeeks.org/virtual-memory-in-operating-system/)
  - [Processes in OS](https://www.geeksforgeeks.org/processes-in-operating-system/)

---

### **3. Stack’s Role in Program Execution**
- The stack stores:
  1. Function arguments.
  2. Return addresses.
  3. Local variables.
- As functions are called, their data is "pushed" onto the stack. When the function exits, data is "popped" off.
- **Example in C:**
  ```c
  int factorial(int n) {
      if (n == 0) return 1;
      return n * factorial(n - 1);  // Recursive calls add to the stack
  }
  ```

- **Resources:**
  - [YouTube: Call Stack and Function Execution](https://www.youtube.com/watch?v=5ROEFrcBnCM)

---

### **4. Static and Dynamic Memory Allocation**
- **Static Allocation:** Memory size is determined at compile time.
  - Example: Global and local variables.
- **Dynamic Allocation:** Memory is allocated during runtime using functions like `malloc`, `calloc`, and `realloc` in C. Requires explicit deallocation with `free`.

- **Resources:**
  - [C Programming: Memory Allocation](https://www.geeksforgeeks.org/dynamic-memory-allocation-in-c-using-malloc-calloc-free-and-realloc/)
  - [YouTube: Memory Allocation in C](https://www.youtube.com/watch?v=XVHyh3hr-_M)

---

### **5. Basic Assembly Language (As It Applies to Compiled Code)**
- Understand assembly instructions like `MOV`, `ADD`, `PUSH`, `POP`, `CALL`, and `RET`.
- Know how function calls, local variables, and return addresses are handled in assembly.
- Recognize how variables and memory are referenced (`EBP`, `ESP`, offsets).
  - Example: A function's assembly might look like:
    ```asm
    PUSH EBP        ; Save base pointer
    MOV EBP, ESP    ; Establish stack frame
    SUB ESP, 4      ; Allocate space for local variable
    MOV [EBP-4], 10 ; Assign 10 to local variable
    ```
- Tools like **Compiler Explorer (Godbolt)** let you compile C code and view its assembly.

- **Resources:**
  - [Introduction to Assembly Language](https://www.tutorialspoint.com/assembly_programming/assembly_basic_syntax.htm)
  - [Compiler Explorer](https://godbolt.org/)
  - [YouTube: Assembly Language Basics](https://www.youtube.com/watch?v=AxmAlqZWx6I)

---

### Suggested Plan
1. **Read about memory organization** (stack vs. heap, registers, virtual memory).
2. **Experiment with assembly and C** using Compiler Explorer.
3. Practice tracing **function calls and memory allocation** in small programs.
4. Ask if you need deeper help on any topic!

## Programming

Got it! Let’s focus on C programming and GDB, as those will be key for your course:

---

### **C Programming**
C is a lower-level language compared to Python or Java, focusing on:
- **Manual memory management** using `malloc`, `free`, and pointers.
- **Data structures** like arrays, structs, and linked lists.
- **Control structures** (loops, conditionals) are similar to higher-level languages.

#### Key Concepts to Learn:
1. **Pointers and Memory Management:**
   - Pointers hold memory addresses:
     ```c
     int a = 10;
     int *p = &a; // p points to the address of a
     printf("%d", *p); // Dereferences p to print the value of a
     ```
   - Allocate memory dynamically with `malloc` and deallocate with `free`.

2. **Functions and Stack Management:**
   - Learn how arguments are passed (by value in C).
   - Understand the role of the stack for local variables and function calls.

3. **File I/O and Libraries:**
   - Basic file handling:
     ```c
     FILE *file = fopen("example.txt", "r");
     if (file) {
         char buffer[100];
         while (fgets(buffer, 100, file)) {
             printf("%s", buffer);
         }
         fclose(file);
     }
     ```

4. **Practice Writing Small Programs:**
   - Reverse a string.
   - Implement linked lists or basic data structures.
   - Solve simple algorithms in C (e.g., Fibonacci, prime numbers).

#### Resources:
- [The C Programming Language (K&R Book)](https://en.wikipedia.org/wiki/The_C_Programming_Language)
- [Learn-C.org](https://www.learn-c.org/)
- [YouTube: C Programming for Beginners](https://www.youtube.com/watch?v=KJgsSFOSQv0)

---

### **GNU Debugger (GDB)**
GDB is essential for debugging C programs and analyzing execution paths.

#### Key Commands:
1. **Starting a Debugging Session:**
   - Compile with `-g` to include debugging symbols:
     ```bash
     gcc -g program.c -o program
     ```
   - Launch GDB:
     ```bash
     gdb ./program
     ```

2. **Breakpoints and Running Code:**
   - Set breakpoints:
     ```bash
     break main
     ```
   - Run the program:
     ```bash
     run
     ```

3. **Inspecting Memory and Variables:**
   - Print variable values:
     ```bash
     print var_name
     ```
   - Dump registers:
     ```bash
     info registers
     ```
   - View the stack:
     ```bash
     backtrace
     ```

4. **Stepping Through Code:**
   - Step line-by-line:
     ```bash
     step
     ```
   - Continue execution until the next breakpoint:
     ```bash
     continue
     ```

5. **Analyzing Assembly:**
   - Disassemble a function:
     ```bash
     disassemble main
     ```
   - Examine memory at a specific address:
     ```bash
     x/10xw 0xaddress
     ```

#### Practice Exercises:
1. Debug a simple program with a segmentation fault (`NULL` pointer dereference).
2. Use breakpoints to analyze the flow of a recursive function.
3. Disassemble and analyze a function’s assembly output.

#### Resources:
- [GDB Tutorial](https://darkdust.net/files/GDB%20Cheat%20Sheet.pdf)
- [GDB Beginner’s Guide](https://www.cs.cmu.edu/~gilpin/tutorial/)
- [YouTube: GDB Basics](https://www.youtube.com/watch?v=PorfLSr3DDI)

### Additional Resources

- [gdb-example-ncurses.html](https://www.brendangregg.com/blog/2016-08-09/gdb-example-ncurses.html) (GDB)
- http://phrack.org/issues/49/14.html (Buffer Overflow Concepts)
- https://www.cprogramming.com/gdb.html (GDB)
- https://docs.python.org/3/tutorial/
- https://www.hacksplainingke.com (For SQL injection / Javascript / XSS/ CSRF  Vulnerabilities)

---

### Suggested Plan
1. Write small C programs and debug them using GDB.
2. Use GDB to set breakpoints, inspect variables, and analyze stack traces.
3. Gradually tackle more complex scenarios, like handling segmentation faults or analyzing assembly.

## Mathematics

Here’s a tailored breakdown to refresh your knowledge and clarify key concepts:

---

### **1. Public Key Cryptography**
Public key cryptography (PKC) relies on a pair of keys:
- **Public key:** Shared openly for encryption.
- **Private key:** Kept secret for decryption.
- **Example: RSA**
  - Based on the difficulty of factoring large numbers.
  - Involves modular arithmetic and prime numbers.

#### Key Concepts:
1. **Key Generation:**
   - Pick two large primes \( p \) and \( q \), compute \( n = p \times q \).
   - Compute \( \phi(n) = (p-1)(q-1) \).
   - Choose \( e \) (public exponent) such that \( \text{gcd}(e, \phi(n)) = 1 \).
   - Compute \( d \) (private exponent) such that \( e \times d \equiv 1 \pmod{\phi(n)} \).

2. **Encryption and Decryption:**
   - Encryption: \( c = m^e \mod n \)
   - Decryption: \( m = c^d \mod n \)

#### Resource:
- [Interactive RSA Example](https://cryptohack.org/challenges/)

---

### **2. Hashing Basics**
A **hash function** maps data of arbitrary size to fixed-size output (hash). Properties include:
- **Deterministic:** Same input always produces the same output.
- **Fast Computation:** Efficient to compute a hash for any input.
- **Pre-image Resistance:** Difficult to find input \( x \) for a given hash \( h(x) \).
- **Collision Resistance:** Difficult to find two distinct inputs \( x_1, x_2 \) such that \( h(x_1) = h(x_2) \).

#### Why It Doesn't Produce "Unique Keys":
- Hashes aren't guaranteed to be unique because inputs vastly outnumber hash values (finite size).
- **Collision Example:**
  - For a 128-bit hash function, there are \( 2^{128} \) possible outputs.
  - Probability of collision grows with more inputs (**Birthday Paradox**).

#### Resource:
- [YouTube: Hashing Explained](https://www.youtube.com/watch?v=b4b8ktEV4Bg)

---

### **3. Modular Arithmetic**
Key to cryptography, involving operations with a modulus \( n \):
- \( a \bmod n \): Remainder when \( a \) is divided by \( n \).
- **Properties:**
  - \( (a + b) \bmod n = [(a \bmod n) + (b \bmod n)] \bmod n \)
  - \( (a \times b) \bmod n = [(a \bmod n) \times (b \bmod n)] \bmod n \)

#### Resource:
- [Modular Arithmetic Primer](https://brilliant.org/wiki/modular-arithmetic/)

---

### **4. Discrete Math and Number Theory Refresher**
- **Greatest Common Divisor (GCD):** Basis for RSA key generation.
- **Modular Inverses:**
  - For \( a \) and \( n \), \( x \) is the modular inverse if \( a \times x \equiv 1 \pmod{n} \).
  - Computed via **Extended Euclidean Algorithm**.

- **Euler's Theorem:**
  - For \( a \) and \( n \) where \( \text{gcd}(a, n) = 1 \):
    \( a^{\phi(n)} \equiv 1 \pmod{n} \)

#### Resource:
- [Number Theory Basics](https://crypto.stanford.edu/pbc/notes/numbertheory/)

---

### **5. Practical Tools and Simulations**
1. **Python Cryptography Libraries:**
   - `pycryptodome` for RSA, hashing.
   - Example:
     ```python
     from Crypto.PublicKey import RSA
     key = RSA.generate(2048)
     print(key.publickey().export_key())
     ```

2. **Cryptographic Playground:**
   - [Cryptography Toolkit](https://cryptotools.net/)
