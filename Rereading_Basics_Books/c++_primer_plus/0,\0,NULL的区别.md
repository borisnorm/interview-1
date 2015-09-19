1.0 is an integer constant

2.'\0' is a character constant

3.NULL is a macro（宏） defined in several standard headers（标准头文件）

4.nul is the name of the character constant. （这个貌似一般很少见把。。反正我没见过 = = ）

All of these are *not* interchangeable（不可交换使用）

各自的用法如下：
1.0 can be used anywhere, it is the generic symbol for each type's zero value and the compiler will sort things out.

2.'\0' should be used only in a character context.

3.NULL is to be used for pointers only since it may be defined as ((void *)0), this would cause problems with anything but pointers.

4.nul is not defined in C or C++, it shouldn't be used unless you define it yourself in a suitable manner, like:#define nul '\0'
