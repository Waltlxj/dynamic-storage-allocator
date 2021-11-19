# Dynamic Storage Allocator
A dynamic storage allocator for C programs that implements the malloc and free routines.
 - mdriver.c – testing driver
 - mm.c – memory allocator implementation

## Author
Walt Li, Eric Gassel

## Strategy
LIFO explicit free lists (implemented as a double-linked list), first fit search, and boundary tag coalescing.

## Heap Structure

```
 * Each block has header and footer of the form:
 *
 *      63                  4  3  2  1  0
 *      ------------------------------------
 *     | s  s  s  s  ... s  s  0  0  0  a/f |
 *      ------------------------------------
 *
 * where s are the meaningful size bits and a/f is 1
 * if and only if the block is allocated. The list has the following form:
 *
 * begin                                                             end
 * heap                                                             heap
 *  -------------------------------------------------------------------------------
 * |  pointer to head  | hdr(16:a) | ftr(16:a) | zero or more usr blks | hdr(0:a) |
 *  -------------------------------------------------------------------------------
 * | pointer to first  |       prologue        |                       | epilogue |
 * |    free block     |         block         |                       | block    |
 *
 * The allocated prologue and epilogue blocks are overhead that
 * eliminate edge conditions during coalescing.
 * 
 * 
 * 
 * Each free block has the following structure
 * 
 * begin                                                             end
 * block                                                            block
 *  ---------------------------------------------------------------------
 * |  hdr(size:f) |  prev address  |  next address  | ... |  ftr(size:f) |
 *  ---------------------------------------------------------------------
```
