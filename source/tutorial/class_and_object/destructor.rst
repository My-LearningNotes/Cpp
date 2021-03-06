C++析构函数
===========

创建对象时系统会自动调用构造函数进行初始化工作，同样，销毁对象时系统也会自动调用一个函数来进行清理工作，
例如释放分配的内存、关闭打开的文件等，这个函数就是析构函数。

析构函数（Destructor）也是一种特殊的成员函数，没有返回值，不需要程序员显式调用，而是在销毁对象时自动执行。
构造函数的名字和类名相同，而析构函数的名字是在类名前面加上一个\ ``~``\ 符号。

析构函数没有参数，不能被重载，因此一个类只能有一个析构函数。
如果用户没有定义，编译器会自动生成一个默认的析构函数（空的函数体，什么都不做）。

函数名是标识符的一种，原则上标识符的命名中不允许出现\ ``~``\ 符号，在析构函数的名字中出现的\ ``~``\ 可以认为是一种特殊情况。
目的是为了和构造函数的名字加以对比和区分。


析构函数的执行时机
------------------

析构函数在对象被销毁时调用，而对象的销毁实际与它所在的内存区域有关。

* 在所有函数之外创建的对象是全局对象，它和全局变量类似，位于内存分区中的全局数据区，程序在结束执行时会调用这些对象的析构函数。
* 在函数内部创建的对象是局部对象，它和局部变量类似，位于栈区，函数执行结束时（离开局部作用域时）会调用这些对象的析构函数。
* ``new``\ 创建的对象位于堆区，通过\ ``delete``\ 删除时调用析构函数, 如果没有\ ``delete``\ ，析构函数就不会执行。

.. note::

    C++中的\ ``new``\ 和\ ``delete``\ 分别用来分配和释放内存，它们与C语言中的\ ``malloc()``\ 和\ ``free()``\ 最大的一个不同之处在于：
    用\ ``new``\ 分配内存时会调用构造函数，用\ ``delete``\ 释放内存时会调用析构函数。
    构造函数和析构函数对于类来说是不可或缺的，所以在C++中应该尽量使用\ ``new``\ 和\ ``delete``\ 。


virtual析构函数
---------------

// Todo

