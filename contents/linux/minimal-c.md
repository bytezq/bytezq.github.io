
#### 通过构建最小的c程序，能够了解什么？

计算机的本质是状态机，程序是指令的集合。

`gcc -c hello.c` 尽管有警告，还是执行。

`objdump -d hello.o`观察二进制指令。


c语言执行的步骤：
1. 预处理，替换宏、处理条件编译、去除注释等。 gcc -E hello.o 
2. 编译，生成汇编代码 gcc -x c -S hello.out
3. 汇编 gcc -c hello.s 或 as -s hello.o
4. 链接 ld hello.o -e main -lc

预处理（Preprocessing）：gcc首先会调用预处理器（cpp）对源文件进行预处理，包括宏展开、头文件包含等操作。预处理后会生成一个经过预处理的中间文件（通常以.i或.ii为扩展名）。

编译（Compiling）：接下来，gcc会调用编译器（cc1）将预处理后的文件编译成汇编代码。编译器会对代码进行语法解析、语义分析，并进行优化。编译后会生成一个汇编代码文件（通常以.s为扩展名）。

汇编（Assembling）：gcc将调用汇编器（as）将汇编代码转换为机器码目标文件（可重定位文件）。这个目标文件通常以.o为扩展名。

链接（Linking）：最后，gcc会调用链接器（ld）将所有编译生成的目标文件和相应的库文件进行链接，生成最终的可执行文件或共享库。在链接过程中，链接器会处理符号解析、地址重定位、库依赖等操作，确保程序的各个部分能正确地连接起来。

需要注意的是，gcc命令会自动处理上述过程，并传递适当的参数给对应的工具（预处理器、编译器、汇编器和链接器）。这样可以简化用户的编译流程并隐藏底层细节。

总结起来，gcc执行ld的过程可以概括为：预处理 -> 编译 -> 汇编 -> 链接。这一过程将源文件转换为最终的可执行文件或共享库。


```
hello.o:     file format elf64-x86-64

Disassembly of section .text:

0000000000000000 <main>:
   0:   f3 0f 1e fa             endbr64
   4:   55                      push   %rbp    # 将 %rbp 寄存器的值压入栈中
   5:   48 89 e5                mov    %rsp,%rbp    # 用于将栈指针寄存器 %rsp 的值复制给基指针寄存器 %rbp
   8:   48 8d 05 00 00 00 00    lea    0x0(%rip),%rax        # f <main+0xf> 用于将 RIP 相对寻址的结果加载到 RAX 寄存器中
   f:   48 89 c7                mov    %rax,%rdi    # 用于将寄存器 %rax 中的值复制给寄存器 %rdi
  12:   e8 00 00 00 00          call   17 <main+0x17>   # 用于调用位于偏移量 17 处的代码
  17:   b8 00 00 00 00          mov    $0x0,%eax
  1c:   5d                      pop    %rbp
  1d:   c3                      ret
  ```


gcc -S hello.c
as -s 

ld -lc hello.o -e main
objdump -d hello.o


通过三条指令。



```
ld作为一个强大且灵活的链接器，具有许多参数选项以满足不同的链接需求。下面列举了一些常用的ld参数：

-o <output>：指定输出文件的名称。
<input>：输入的目标文件，可以是可重定位文件或共享库。
-L <dir>：添加额外的库文件搜索路径。
-l <library>：链接指定的库文件。
-rpath <dir>：指定运行时库搜索路径。
-dynamic-linker <loader>：指定动态链接器（运行时加载程序）。
-nostdlib：禁止使用标准系统库。
-static：强制静态链接，不使用共享库。
-Bstatic：仅在后续库文件中使用静态链接。
-Bdynamic：仅在后续库文件中使用动态链接。
-T <script>：使用指定的链接脚本（Linker Script）进行链接。
-e <entry>：指定程序的入口点地址。
-O<level>：设置优化级别。（例如：-O0表示无优化，-O2表示较高的优化级别）
-Xlinker <option>：将选项传递给底层链接器。
这只是一小部分常用的ld参数，实际使用中可能还有其他更具体的参数选项，可以通过man ld命令或者在终端中运行ld --help查看完整的参数列表和说明。
```


ldd是一个用于查看可执行文件或共享库所依赖的动态链接库的命令。
ldd a.out
`#` ldd a.out 
linux-vdso.so.1 =>  (0x00007ffeccb13000)
libc.so.6 => /lib64/libc.so.6 (0x00007f7190c7f000)
/lib/ld64.so.1 => /lib64/ld-linux-x86-64.so.2 (0x00007f719104d000)



参考：
[【编译、链接、装载二】/lib/ld64.so.1: bad ELF interpreter: 没有那个文件或目录](https://blog.csdn.net/junxuezheng/article/details/130139706)