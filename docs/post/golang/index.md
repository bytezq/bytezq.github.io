
golang中包名与路径的关系：
- import后面的是目录（包所在绝对或相对路径）
- 包名和目录名没有关系，但是包名最好与目录名相同
- 同一个目录下最多只能有一种包名  

Go 包的概念
	
1. 把相同的功能放到一个目录，称之为包
2. 包可以被其他的包引用
3. main包用来生成可执行文件，每个程序只有一个main包
4. 包可以提高代码的可复用性
Go 包 的特征
一个文件夹下只能有一个package。
	
• import后面的其实是GOPATH开始的相对目录路径，包括最后一段。但由于一个目录下只能有一个package，所以import一个路径就等于是import了这个路径下的包。
• 注意，这里指的是“直接包含”的go文件。如果有子目录，那么子目录的父目录是完全两个包。 	
		
• 比如你实现了一个计算器package，名叫calc，位于calc目录下；但又想给别人一个使用范例，于是在calc下可以建个example子目录（calc/example/），这个子目录里有个example.go（calc/example/example.go）。此时，example.go可以是main包，里面还可以有个main函数。
	
一个package的文件不能在多个文件夹下。
在 Golang 的文档中，Language Specification 页面，Package clause 下，指明了
A set of files sharing the same PackageName form the implementation of a package. An implementation may require that all source files for a package inhabit the same directory.也就是说，一个包所有的文件，必须位于同一个目录下
	
• 如果多个文件夹下有重名的package，它们其实是彼此无关的package。
• 如果一个go文件需要同时使用不同目录下的同名package，需要在import这些目录时为每个目录指定一个package的别名。
包名自然可以和文件夹名不一样，毕竟一个是导入路径，一个是包名
但不建议这么做，这样容易造成调用这个包的人，无法快速知道这个包的名称是什么
至于为什么不用目录名作为包名，我想也正如大家所说，为了避免目录中出现奇怪的字符，也为了调用者方便使用
在 Golang 的文档中， Language Specification 页面，Import declarations 下，有这样的说明
在Go 语言规范官方文档中有对PackageName和ImportPath的具体描述：
ImportDecl       = "import" ( ImportSpec | "(" { ImportSpec ";" } ")" ) .
ImportSpec       = [ "." | PackageName ] ImportPath .
ImportPath       = string_lit .
The PackageName is used in qualified identifiers to access exported identifiers of the package within the importing source file. It is declared in the file block. If the PackageName is omitted, it defaults to the identifier specified in the package clause of the imported package. If an explicit period (.) appears instead of a name, all the package’s exported identifiers declared in that package’s package block will be declared in the importing source file’s file block and must be accessed without a qualifier.
也就是说，在执行导入的时候，若不手动定义包名，则从导入路径的源码文件中的 package 行获取包名，也即目录名和包名没有直接的关系。
结论
1、import 导入的参数是路径，而非包名。
2、尽管习惯将包名和目录名保证一致，但这不是强制规定；
3、在代码中引用包成员时，使用包名而非目录名；
4、同一目录下，所有源文件必须使用相同的包名称（因为导入时使用绝对路径，所以在搜索路径下，包必须有唯一路径，但无须是唯一名字）；
5、至于文件名，更没啥限制（扩展名为.go）;

[错误处理](https://coolshell.cn/articles/21140.html)

[忽略错误](https://learnku.com/articles/23577/gos-error-handling-strategy-notes)