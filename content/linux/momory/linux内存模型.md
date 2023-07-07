![](assets/linux-memory-model.png)

[stackflow关于stack space的解释](https://stackoverflow.com/questions/44858528/where-are-the-stacks-for-the-other-threads-located-in-a-process-virtual-address)

一个新线程的栈空间是由父线程或进程调用 [`mmap(MAP_ANONYMOUS|MAP_STACK)`](http://man7.org/linux/man-pages/man2/mmap.2.html)方法创建，在内存的`memory map segment`位置，如图中所示。可以位于一个大的`malloc()`能创建的任何地方。(glibc `malloc(3)` 使用 `mmap(MAP_ANONYMOUS)` 分配大空间。)

(`MAP_STACK` is currently a no-op, and exists in case some future architecture needs special handling)。

把指向新线程栈空间的指针传给
[the `clone(2)` system call](http://man7.org/linux/man-pages/man2/clone.2.html)，这里才是真正创建线程的地方。(尝试在一个多线程的进程中使用 `strace -f` 观察)，参考 [通过raw Linux syscalls创建新线程](http://nullprogram.com/blog/2015/05/15/)。
更多关于 mmaping stacks的细节可以参考 [this answer on a related question](https://stackoverflow.com/a/5369022/224132) 

`MAP_GROWSDOWN` doesn't prevent another `mmap()` from picking the address right below the thread stack, so you can't depend on it to dynamically grow a small stack the way you can for the main thread's stack (where the kernel reserves the address space even though it's not mapped yet).

So even though `mmap(MAP_GROWSDOWN)` was designed for allocating stacks, [it's so bad that Ulrich Drepper proposed removing it in 2.6.29](https://lwn.net/Articles/294001/).
Also, note that your memory-map diagram is for a 32-bit kernel. A 64-bit kernel doesn't have to reserve any user virtual-address space for mapping kernel memory, so a 32-bit process running on an amd64 kernel can use the full 4GB of virtual address space. (Except for the low 64k by default (sysctl `vm.mmap_min_addr = 65536`), so NULL-pointer dereference does actually fault. And [the top page is also reserved as error codes](https://stackoverflow.com/questions/47712502/why-cant-i-mmapmap-fixed-the-highest-virtual-page-in-a-32-bit-linux-process-o), not valid pointers.)