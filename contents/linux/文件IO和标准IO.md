## 第一部分：文件I/O

文件 I/O 由 POSIX.1 和 Single UNIX Specification 标准定义，不是ISO C的组成部分。

文件 I/O 通常称为不带缓冲的I/O（unbuffered I/O），不带缓冲是指每个read和write都直接调用内核中的一个系统调用。

UNIX系统中的大多数文件I/O只需要用到5个函数：open、read、write、lseek、close，不同的缓冲区长度会对read和write的效率产生影响。

文件 I/O 涉及到在多个进程间共享文件，原子操作十分重要，将讨论在多个进程间如何共享文件，以及所涉及的内核有关数据结构，dup、fcntl、sync、fsync和ioctl函数。
### 文件描述符

所有的文件I/O操作都是围绕文件描述符的，文件描述符在形式上是一个非负整数。实际上，它是一个索引值，指向内核为每一个进程所维护的该进程打开文件的记录表。当程序打开一个现有文件或者创建一个新文件时，内核向进程返回一个文件描述符。  
每个Unix进程（除了可能的守护进程）应均有三个标准的POSIX文件描述符，对应于三个标准流：

- STDIN_FILENO
- STDOUT_FILENO
- STDERR_FILENO
### 文件读写相关函数

#### open() 和 openat() 函数

```less
#include <fcntl.h>

int open(const char *path, int oflag, ... /* mode_t mode */);
int openat(int f d, const char *path, int oflag, ... /* mode_t mode */ );
```

- path 表示要打开或要创建的文件路径；
- oflag 表示打开文件的方式，常用的有 O_RDONLY​、O_WRONLY、O_RDWR等。
- mode_t mode 新文件的访问权限位，新创建文件（oflag为O_CREAT）时用到。
 
除此之外，openat() 比open()多了一个fd参数，有三种用法：
（1）path为是绝对路径，fd参数被忽略，此时openat函数就相当于open函数。
（2）path为相对路径名，fd参数指出了相对路径名在文件系统中的开始地址，通过打开相对路径名所在的目录来获取。
（3）path为相对路径，fd参数具有特殊值 AT_FDCWD，此时，路径名在当前工作目录中获取。

#### creat() 函数
```less
#include <fcntl.h>

int creat(const char *path, mode_t mode);
```
等效于 `open(path, O_WRONLY｜O_CREAT｜O_TRUNC, mode)` ，在早期的UNIX系统版本中，open的第二个参数只能是0、1或2。无法打开一个尚未存在的文件，因此需要另一个系统调用creat以创建新文件。现在，open函数提供了选项O_CREAT和O_TRUNC，于是也就不再需要单独的creat函数。
#### close() 函数
```less
#include <unistd.h>

int close (int fd);
```
关闭一个文件并且释放该进程加在该文件上的所有记录锁。当一个进程终止时，内核自动关闭它所有的打开文件。很多程序都利用了这一功能而不显式地用close关闭打开文件。
#### lseek() 函数
#### read() 函数
#### write() 函数



### 缓冲区大小对I/O效率的影响
### 文件共享和原子操作及相关函数

## 第二部分：标准I/O

标准I/O库由ISO C标准定义，Single UNIX Specification 对 ISO C 标准进行了扩充。

标准I/O库处理很多细节，如缓冲区分配、以优化的块长度执行I/O等。这些处理使用户不必担心如何选择使用正确的块长度（如3.9节中所述）

### 流和FILE对象

所有的标准I/O操作都是围绕流（stream）进行的，使用标准库打开或创建一个文件时，系统将一个流与一个文件相关联。

对于ASCII字符集，一个字符用一个字节表示，对于国际字符集，一个字符可用多个字节表示。标准I/O文件流可用于单字节或多字节（“宽”）字符集。

流的定向（stream's orientation）决定了所读、写的字符是单字节还是多字节的。当一个流最初被创建时，它并没有定向。在未定向的流上使用多字节 I/O 函数，流的定向将设置为宽定向的；使用单字节I/O函数，则流的定向设为字节定向的。

只有两个函数可改变流的定向，freopen函数清除一个流的定向；fwide函数可用于设置流的定向。


对一个进程预定义了 3 个流，分别是：标准输入、标准输出和标准错误。这些流引用的文件与之前提到的文件描述符 STDIN_FILENO、STDOUT_FILENO 和 STDERR_FILENO 所引用的文件相同。
这3个标准 I/O 流通过预定义文件指针 stdin、stdout 和 stderr 加以引用。这3个文件指针定义在头文件<stdio.h>中。


## 第三部分：文件和目录