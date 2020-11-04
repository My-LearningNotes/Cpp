C++ ``this``\ 指针
==================

``this``\ 是C++中的一个关键字，也是一个\ ``const``\ 指针，它指向当前对象，通过它可以访问当前对象的所有成员。

所谓当前对象，是指正在使用的对象。

Example:

.. code-block:: cpp
    :emphasize-lines: 20, 25, 30    

    #include <iostream>

    using namespace std;

    class Student
    {
    public:
        void setName(char *name);
        void setAge(int age);
        void setScore(float score);

    private:
        char *name;
        int age;
        float score;
    };

    void Student::setName(char *name)
    {
        this->name = name;
    }

    void Student::setAge(int age)
    {
        this->age = age;
    }

    void Student::setScore(float score)
    {
        this->score = score;
    }

``this``\ 只能在类的内部使用，通过\ ``this``\ 可以访问类的所有成员，包括\ ``private``\ 、\ ``protected``\ 和\ ``public``\ 属性的。

在上面的例子中，成员函数的参数和成员变量重名，只能通过\ ``this``\ 区分。

.. attention::

    ``this``\ 是一个指针，要用\ ``->``\ 来访问成员变量或成员函数。

``this``\ 虽然用在类的内部，但是只有在对象被创建之后才会给\ ``this``\ 赋值，并且这个赋值的过程是编译器自动完成的，不需要用户干预，用户也不能显式地给\ ``this``\ 赋值。

需要注意的几点：

* ``this``\ 是\ ``const``\ 指针，它的值是不能被修改的，一切企图修改该指针的操作，如赋值、递增、递减等都是不允许的。
* ``this``\ 只能在成员函数内部使用，用在其它地方没有意义，也是非法的。
* 只有对象被创建之后\ ``this``\ 才有意义，因此不能在\ ``static``\ 成员函数中使用。


``this``\ 到底是什么
--------------------

``this``\ 实际上是成员函数的一个形参，在调用成员函数时将对象的地址作为实参传递给\ ``this``\ 。
不过\ ``this``\ 这个形参是隐式的，它并不出现在代码中，而是在编译阶段由编译器隐式将它添加到参数列表中。

``this``\ 作为隐式形参，本质上是成员函数的局部变量，所以只能在成员函数内部使用，并且只有在通过对象调用成员函数时才给\ ``this``\ 赋值。

