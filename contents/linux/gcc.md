### gcc简介  

gcc、g++分别是gnu的c、c++编译器，gcc/g++在执行编译工作的时候，总共需要4步:

- 预处理： 生成预处理文件，后缀名 .i (预处理器cpp)
- 编译： 预处理后的文件编译生成汇编语言文件，后缀名 .s(编译器egcs)
- 汇编： 汇编语言文件汇编生成目标代码(机器代码)文件，后缀名.o (汇编器as)
- 链接： 链接目标代码, 生成可执行文件 (链接器ld)

### 参数详解  

-x language filename 设定文件所使用的语言，使后缀名无效，对以后的多个文件有效，直到下一个参数的使用。language可以是：c, objective-c, c-header, c++, cpp-output, assembler, assembler-with-cpp。  
例如: gcc -x c hello.pig

-x none filename 关掉上一个选项，即让gcc根据文件名后缀，自动识别文件类型
例如： gcc -x c hello.pig -x none hello2.c

-c 只激活**预处理，编译和汇编**，生成后缀名.o的obj文件
例如： gcc -c hello.c

-S 只激活**预处理和编译**，生成后缀名.s的汇编代码文件。
例如： gcc -S hello.c

-E 只激活**预处理**，不生成文件，你需要把它重定向到一个输出文件里面。
例如： gcc -E hello.c > pianoapan.txt

-o 指定目标名称，缺省的时候，gcc 编译出来的文件是a.out
例如： gcc -o hello hello.c，gcc -o hello.asm -S hello.c

-pipe 使用管道代替编译中临时文件，在使用非gnu汇编工具的时候，可能有些问题
例如： gcc -pipe -o hello.exe hello.c

-ansi 关闭gnu c中与ansi c不兼容的特性，激活ansi c的专有特性
(包括禁止asm，inline，typeof关键字，以及unix，vax等预处理宏)。具体参考man page

-fno-asm 此选项实现ansi选项的功能的一部分，它禁止将asm,inline和typeof用作关键字。

-fno-strict-prototype 只对g++起作用，使用这个选项，g++将对不带参数的函数，都认为是没有显式的对参数的个数和类型说明，而不是没有参数。 而gcc无论是否使用这个参数，都将对没有带参数的函数，认为是没有显式说明的类型。

-fthis-is-varialble 就是向传统c++看齐，可以使用this当一般变量使用。

-fcond-mismatch 允许条件表达式的第二和第三参数类型不匹配，表达式的值将为void类型

-funsigned-char， -fno-signed-char， -fsigned-char， -fno-unsigned-char
这四个参数是对char类型进行设置，决定将char类型设置成unsigned char(前两个参数)或者 signed char(后两个参数)

-include file 包含某个代码，简单来说，就是当某个文件需要另一个文件的时候，就可以用它设定，功能就相当于在代码中使用`#include<filename>`
例如： gcc hello.c -include /root/pianopan.h

-imacros file 将file文件的宏，扩展到gcc/g++的输入文件，宏定义本身并不出现在输入文件中。这个选项跟-include选项很像，但是任何通过扫描file产生的输出都被丢弃，定义的选项仍被定义，这允许你获得所有头文件的宏定义但不处理他的声明。所有该选项指定的文件将在-include选项指定的文件之前处理。

-Dmacro 相当于C语言中的#define macro

-Dmacro=defn 相当于C语言中的#define macro=defn

-Umacro 相当于C语言中的#undef macro

-undef 取消对任何非标准宏的定义

-Idir 在你使用#include "file"的时候，gcc/g++会先在当前目录查找你所指定的头文件，如果没有找到，他会到缺省的头文件目录找，如果使用-I指定了目录，他会先在你所指定的目录查找，然后再按常规的顺序去找。对于`#include <file>`，gcc/g++会到-I指定的目录查找，查找不到，然后将到系统的缺省的头文件目录查找。

-I- 就是取消前一个参数的功能,所以一般在-Idir之后使用

-idirafter dir 在-I的目录里面查找失败，将到这个目录里面查找。

-iprefix prefix，-iwithprefix dir 一般一起使用，当-I的目录查找失败，会到prefix+dir下查找

-nostdinc 使编译器不在系统缺省的头文件目录里面找头文件，一般和-I联合使用，明确限定头文件的位置

-nostdinc++ 规定不在g++指定的标准路经中搜索，但仍在其他路径中搜索，此选项在创建libg++库使用

-C 在预处理的时候,不删除注释信息，一般和-E一起使用，有时候分析程序，用这个很方便的

-M 生成文件关联的信息。包含目标文件所依赖的所有源代码，你可以用gcc -M hello.c 来测试一下，很简单。

-MM 和上面的那个一样，但是它将忽略由`#include<file>`造成的依赖关系。

-MD 和-M相同，但是输出将导入到.d的文件里面

-MMD和-MM相同，但是输出将导入到.d的文件里面

-Wa,option 此选项传递option给汇编程序；如果option中间有逗号，就将option分成多个选项，然后传递给汇编程序

-Wl.option 此选项传递option给链接程序；如果option中间有逗号，就将option分成多个选项，然后传递给链接程序.

-llibrary 指定编译的时候使用的库 例如： gcc -lcurses hello.c

-Ldir 指定编译的时候，搜索库的路径。如果不指定，编译器将只在标准库的目录找。

-O0，-O1，-O2，-O3 编译器的优化选项的4个级别，-O0表示没有优化，-O1为缺省值，-O3优化级别最高

-g 指示编译器，在编译的时候，产生调试信息。

-gstabs 此选项以stabs格式生成调试信息，但是不包括gdb调试信息。

-gstabs+此选项以stabs格式生成调试信息，并且包含仅供gdb使用的额外调试信息。

-ggdb 此选项将尽可能的生成gdb可以使用的调试信息。

-static 此选项将禁止使用动态库。

-share 此选项将尽量使用动态库。

-traditional 试图让编译器支持传统的C语言特性

-w 不生成任何警告信息。默认选项

-Wall 生成所有警告信息。

-save-temps 它会保存所有生成的中间文件，包括预处理后的代码、汇编代码和目标代码。

GCC 可同时用来编译 C 程序和 C++ 程序。一般来说，C 编译器通过源文件的后缀名来判断是 C 程序还是 C++ 程序。在 Linux 中，C 源文件的后缀名为 .c，而 C++ 源文件的后缀名为 .C 或 .cpp。但是，gcc 命令只能编译 C++ 源文件，而不能自动和 C++ 程序使用的库链接。因此，通常使用 g++ 命令来完成 C++ 程序的编译和链接，该程序会自动调用 gcc 实现编译。