1.0 is an integer constant
1.0是一个整型常量
2.'\0' is a character constant
2。'\0'是字符串常量
3.NULL is a macro（宏） defined in several standard headers
3.NULL是一个在多个标准头文件定义的一个宏。
4.nul is the name of the character constant. （这个貌似一般很少见把。。反正我没见过 = = ）

All of these are not interchangeable（不可交换使用）
所有这些都是不互换
各自的用法如下：
1.0 can be used anywhere, it is the generic symbol for each type's zero value and the compiler will sort things out.
1.0可用于任何地方，它是对每种类型清零并且编译器将初始数值的通用符号。
2.'\0' should be used only in a character context.
2.'\ 0'应该只用于在一个字符的上下文。
3.NULL is to be used for pointers only since it may be defined as ((void *)0), this would cause problems with anything but pointers.
3.NULL只能用于指针，因为它可被定义成（（viod*）0），这会引起一些问题与任何东西除了指针。
4.nul is not defined in C or C++, it shouldn't be used unless you define it yourself in a suitable manner, like:#define nul '\0'
