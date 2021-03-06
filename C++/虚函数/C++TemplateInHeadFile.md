﻿# [**C++模板类代码只能写在头文件？**](https://www.cnblogs.com/findumars/p/9302615.html)

这个问题，实际上我几年前就遇到了。最近写个模板类玩的时候，再次遇到。

当我非常仔细的将定义和实现分开，在头文件中保留了最少的依赖后，一切就绪.cpp单独编过。但是当使用的时候，就会报告所有的函数调用都没有实现。按常规.h/.cpp而言这是不可能的。但是模板类就是这么独特。简单说说他的原因，也备自己将来遗忘：

从语法角度而言，是没有强制要求说模板代码的声明和实现不可以分开。那么当分离的声明和实现写好后，单独编译.cpp是可以通过的，但是生成的.o文件却非常小，只有一个原因：确实没有任何实现代码！————不知道用什么类型参数套用模板。

因为模板类需要在使用到的地方利用声明模板的typename或者class参数的时候，才会即时生成代码。那么当我把模板声明和实现分开的时候，这个即时过程因为编译器只能通过代码include“看到”头文件而找不到模板实现代码，所以会产生链接问题。这也是为什么几乎都会建议模板类和声明和实现都写在头文件。

如果刚接触c/c++编写的朋友可能还不是很明白。编译器面对巨量代码的时候，也是以一个一个的.cpp/.c文件作为基本单元，根据代码的include包含找到声明，翻译代码产生.o文件。注意他们每个cpp/c文件都是相互独立完成自己工作的，对于缺少的部分，如果妥善声明，会留待链接过程的时候产生引用关系。 那么刚才说的模板类实现代码，编译它的时候因为不知道套用什么参数，实际上没有任何有用的内容存在于.o文件当中。而在使用模板类的地方指定了类型参数，编译器这才开始根据模板代码产生有用的.o编码，可是这些内容放在了使用模板的代码产生的.o文件当中。如果编使用模板代码的时候，通过include包含“看不到”模板的实现代码，这些所有的缺失，到链接阶段就无法完成。

所以最后的结论是：请老老实实把模板的实现和声明都写在头文件吧。如果你很有想法有个性，可以坚持，然后试试#include “xxxx.cpp” 这样屌炸的代码。

<https://blog.csdn.net/jinzeyu_cn/article/details/45795923>
