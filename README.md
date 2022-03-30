# A_garbage_collector_for_C

Dynamic memory (i.e. memory accessed via the malloc and free family of commands) is essential to many programs in the freedom, control, and interpretability it allows. We've talked about 2 different types of management for dynamic memory: implicit allocators and explicit allocators. This program focuses on the implicit allocator, perhaps better known as the garbage collector. Intuitively, we want this tool to find the blocks (or chunks as we refer to them) that are not being used and return them to the process as free chunks.

Memory layout review

As a quick review, we can visualize dynamic memory (aka the heap) as a list of chunks. From class, we know that each allocated or free block is started with header (shown as 'h' in the figure below), contains a payload ('x'), and has some form of padding at the end.

----------------------------------------------------------------------
| h  | x  | x  | h  |    | h  | x  | h  |    |    |    |    |    |    ...
----------------------------------------------------------------------
^              ^         ^         ^
|              |         |         |
a              b         c         the rest of the heap...

In the figure, 'a' represents an allocated section of the heap with a payload + padding taking up 2 words worth of space. Similarly, 'b' is a free node of size 1 word.

Chunk structure

typedef struct chunk {
         size_t size;
         struct chunk* next;
         struct chunk* prev;
} chunk;

The program

a basic, conservative garbage collector for C programs. Starting from the set of root pointers present in the stack and global variables,  traverse the “object graph” (in the case of malloc, a chunk graph) to find all chunks reachable from the root. These are marked using the third lowest order bit in the size field of each chunk. implemented several helpful functions for you ranging from checking for marked chunks to navigation of the heap.  implemented the sweeping functionality of the garbage collector, the pointer-confirmation functionality, and a portion of the marking functionality.

Note that all of the tests have descriptions in main.c!
