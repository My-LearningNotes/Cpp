C++对象数组
===========

C++允许数组的每个元素都是对象，这样的数组称为\ **对象数组**\ 。

对象数组中的每个元素都需要用构造函数初始化。
具体哪些元素用哪些构造函数初始化，取决于定义数组时的写法。

Example:

.. code-block:: cpp
    :emphasize-lines: 22, 25, 28, 31

    #include <iostream>

    using namespace std;

    class CSample
    {
    public:
        CSample()
        {
            cout << "Constructor 1 called" << endl;
        }

        CSample(int n)
        {
            cout << "Constructor 2 called" << endl;
        }
    };

    int main()
    {
        cout << "step 1" << endl;
        CSample array1[2];

        cout << "step 2" << endl;
        CSample array2[2] = {4, 5};

        cout << "step  3" << endl;
        CSample array3[3] = {3};

        cout << "step 4" << endl;
        CSample *array4 = new CSamaple[2];
        delete [] array4;

        return 0;
    }

    // 运行结果：
    // step 1
    // Constructor 1 called
    // Constructor 1 called
    // step 2
    // Constructor 2 called
    // Constructor 2 called
    // step 3
    // Constructor 2 called
    // Constructor 1 called
    // step 4
    // Constructor 1 called
    // Constructor 1 called

* **如果没有为元素指定参数，那么默认调用无参构造函数初始化；**
* **如果指定了参数，则调用相应的构造函数初始化。**

**在构造函数有多个参数时，数组的初始化列表中要显式地创建对象, 或者以一个子初始化列表的形式传递参数。**

Example:

.. code-block:: cpp
    :emphasize-lines: 16, 18

    #include <iostream>

    using namespace std;

    class CTest
    {
    public:
        CTest() {}
        CTest(int n) {}
        CTest(int m, int n) {}
    };

    int main()
    {
        // 显式的创建一个对象，或者通过子初始化列表传递参数
        CTest array2[3] = {CTest(2, 3}, {1, 2},  1};
        // 对象指针数组
        CTest *pArray3[3] = {new CTest(4), new CTest(1, 2)};

        return 0;
    }

.. note::

    在对象数组的初始化列表中，创建对象和通过子初始化列表传递参数的区别：

    * 在初始化列表中创建一个对象，然后通过复制构造函数创建数组元素；
    * 通过子初始化列表传递参数，直接通过该参数来构造数组元素。

    // 以上待验证

