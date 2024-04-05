### Notes

- First 10 minutes or so went through latency numbers
  - Separated volatile and non-volatile
  - Do as much as we can on memory for performance.
- Virtual spaces are where the OS provides pointers for programs to access disk pages
  - OS loads page from disk to physical memory, and links it to the virtual memory

#### MMap
- Mmap is a system call that lets the OS load pages from the disk to physical space and link to 
virtual space.
- DBMS can use mmap to store file contents on virtual space for program, thereby delegating to OS.
  - For read-only, can still use mmap and some DBs do
- This has problems:
  - Transactions: OS can flush dirty pages. It can take locks but it cannot guarantee it won't flush out
  a page in the middle of a transaction.
  - I/O stalls can happen because the page isn't in physical memory (called a page fault), and the physical memory is 
  full requiring eviction 
    - The thread accessing the page will block
  - Error handling: OS will send interrupts like `SIGBUS` which DBMS needs to handle. 
    - These are seemingly more complex than just using error codes.
  - Performance issues due to OS data structure contention which it also needs to manage.

##### Advises for using MMAP if had to
- madvise: advises OS to 
