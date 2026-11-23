# What can we do if the program has overflow vuln?
1. Remote Code execution
2. Privilege of escalation
3. Unexpected function execution
4. Denial of Service(DoS)
5. and whatever your imagination

overflow vuln gives us to modify memory area. and as we know, all of program work in memory. So if we've a chance to modify it, we can control the target computer or program.
But, nowaday, it's not simply exploit overflow vulns to get target system due to several security midigations so there's other impact than Remote code execution such as DoS, Unexpected Function Execution.

# What is buffer overflow?
Before explain buffer overflow, we need to know about program workflow.
If we installed program, it will saved in HDD or SSD. we called that Storage.
when we execute the program which is saved on storage, it will going on memory and than, translate to machine language by interpreter. finally, CPU execute the machine language instructions

```
+-------------------------------------------------------+
|  1. STORAGE (HDD / SSD)                               |
|     [ Installed Program File ]                        |
+-------------------------------------------------------+
                           |
                           | Execute program
                           v
+-------------------------------------------------------+
|  2. MEMORY (RAM)                                      |
|  +-------------------------------------------------+  |
|  |  Allocated Space (Buffer)  |  Adjacent Memory   |  |
|  |  [ Program Data =======> ] |  [ OVERFLOW! ]     |  |
|  +-------------------------------------------------+  |
|     * Memory overflow happens here when data exceeds  |
|       the allocated space and spills over.            |
+-------------------------------------------------------+
                           |
                           | Translate to machine code
                           v
+-------------------------------------------------------+
|  3. INTERPRETER                                       |
|     [ Source Code ---> Machine Language (0s & 1s) ]   |
+-------------------------------------------------------+
                           |
                           | Send instructions
                           v
+-------------------------------------------------------+
|  4. CPU                                               |
|     [ Executes Machine Language instructions ]             |
+-------------------------------------------------------+
```

OK! but almost understand how our program work on our computer.

To abuse the program, we should know where is going though our input value and where use our input value.

For example, If memory(RAM) use and accept our input in raw. that means, we can write memory what we want.

If we can command to CPU using our input, we can take the computer.

let's move to about how to work memory.

This is a memory(RAM) structure. the areas divided into stack, heap, datasegment, text and code segment. we don't need to understand all of it for now. to understatn in easy, treat it all area as memory. memory role is a saving data. 
```
High Memory Address (e.g., 0xFFFFFFFF)
+-------------------------------------------------------+
|  1. STACK                                             |
|     - Stores local variables, function calls, &       |
|       return addresses.                               |
|     - Managed automatically (LIFO: Last-In, First-Out)|
|     - Grows DOWNWARD  |                               |
|                       v          
|  - - - - - - - - - - - - - - - - - - - - - - - - - -  |
|                 (Free / Unallocated Space)            |
|  - - - - - - - - - - - - - - - - - - - - - - - - - -  |
|                                                       |
|                       ^  
|     - Grows UPWARD    |   
|     - Stores dynamically allocated memory (e.g.,      |
|       objects, large arrays via malloc/new).          |
|     - Managed manually or by Garbage Collector.       |
|  2. HEAP                                              |
+-------------------------------------------------------+
|  3. DATA SEGMENT (BSS & Initialized Data)             |
|     - Stores global and static variables that stay    |
|       alive for the entire program runtime.           |
+-------------------------------------------------------+
|  4. TEXT / CODE SEGMENT                               |
|     - Stores the compiled machine language            |
|       instructions (Read-Only so code isn't altered). |
+-------------------------------------------------------+
Low Memory Address (e.g., 0x00000000)
```

Let's see 
```
#include <stdio.h>
#include <string.h>

void check_login(char *input) {
    int is_admin = 0;       // admin plag
    char buffer[16];        // 16 byte's buffer size

    strcpy(buffer, input);  // strcpy function doesn't has input length validation

    if (is_admin)
        printf("Admin access granted! (is_admin = 0x%x)\n", is_admin);
    else
        printf("Access denied.\n");
}

int main(int argc, char *argv[]) {
    if (argc < 2) {
        printf("Usage: %s <input>\n", argv[0]);
        return 1;
    }
    check_login(argv[1]);
    return 0;
}
```

```
char *strcpy(char *dest, char *src) {
    int i = 0;
    while (src[i] != '\0') {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';
    return dest;
}
```



Our input from the program will going to Stack area in memory. (Not all of our input. it depends on program function)






