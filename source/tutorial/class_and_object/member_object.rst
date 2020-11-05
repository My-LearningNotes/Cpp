C++成员对象和封闭类
===================

一个类的成员变量如果是另一个类的对象，就称之为\ **成员对象**\ 。包含成员对象的类叫\ **封闭类（enclosed class）**\ 。


成员对象的初始化
----------------

创建封闭类的对象时，它包含的成员对象也需要被创建，这就会引发成员对象构造函数的调用。
如何让编译器知道，成员对象到底是用哪个构造函数初始化的呢？这就需要借助封闭类构造函数的初始化列表。

构造函数初始化列表的写法如下：

.. code-block:: text

    类名::构造函数名(参数表) : 成员变量1(参数表), 成员变量2(参数表), ...
    {
        // ...
    }

对于基本类型的成员变量，“参数表”中只有一个值，就是初始值，在调用构造函数时，会把这个初始值直接赋给成员变量。
但是对于成员对象，“参数表”中存放的是构造函数的参数，它可能是一个值，也可能是多个值，它指明了该成员对象如何被初始化。

.. attention::

    如果没有在封闭类的构造函数的初始化列表中\ **显式**\ 地初始化成员对象，则调用成员对象的默认构造函数。

Example:

.. code-block:: cpp
    :emphasize-lines: 55
    
    #include <iostream>

    using namespace std;

    class Tyre
    {
    public:
        Tyre(int radius, int width);
        void show() const;

    private:
        int m_radius;
        int m_width;
    };

    Tyre::Tyre(int radius, int width) : m_radius(radius), m_width(width)
    {}

    void Tyre::show() const
    {
        cout << "radius: " << m_radius << ", width: " << m_width << endl;
    }

    class Engine
    {
    public:
        Engine(float displacement = 2.0);
        void show() const;

    private:
        float m_displacement;
    };

    Engine::Engine(float displacement) : m_displacement(displacement)
    {}

    void Engine::show() const
    {
        cout << "displacement: " << m_displacement << endl;
    }

    class Car
    {
    public:
        Car(int price, int radius, int width);
        void show() const;

    private:
        int m_price;
        Tyre tyre;
        Engine engine;
    };

    // 在封闭类的构造函数中，指定如何初始化成员对象
    Car::Car(int price, int radius, int width) : m_price(price), tyre(radius, width)
    {}

    void Car::show() const
    {
        cout << "price: " << m_price << endl;
        tyre.show();
        engine.show();
    }

    int main()
    {
        Car car(200000, 19, 245);
        car.show();

        return 0;
    }

    // 运行结果:
    // price: 200000
    // radius: 19, width: 245
    // displacement: 2
    
总之，在封闭类的构造函数中，要让编译器知道如何构建成员对象，否则编译就会报错。


成员对象的消亡
==============

封闭类对象生成时，先执行所有成员对象的构造函数，然后才执行封闭类自己的构造函数。
成员对象的构造函数的执行次序和成员对象给在类定义中的次序一致，与它们在构造函数初始化列表中列出的次序无关。

当封闭类消亡时，先执行封闭类的析构函数，然后再执行成员对象的析构函数，成员对象析构函数的执行次序和构造函数的执行次序相反，即先构造的后析构，这是C++处理此类问题的一般规律。

.. note::

    可以想象构造函数和析构函数之间有一条中间线，构造和析构的顺序，以这条中间线对称。

Example:

.. code-block:: cpp
    :emphasize-lines: 27, 28

    #include <iostream>

    using namespace std;

    class Tyre
    {
    public:
        Tyre() {cout << "Tyre constructor" << endl;}
        ~Tyre() {cout << "Tyre destructor" << endl;}
    };

    class Engine
    {
    public:
        Engine() {cout << "Engine constructor" << endl;}
        ~Engine() {cout << "Engine destructor" << endl;}
    };

    class Car
    {
    public:
        Car() {cout << "Car constructor" << endl;}
        ~Car() {cout << "Car destructor" << endl;}

    private:
        // 在类中的定义顺序，决定了构造时的初始化顺序
        Engine engine;
        Tyre tyre;
    };

    int main()
    {
        Car car;

        return 0;
    }

    // 运行结果:
    // Engine constructor
    // Tyre constructor
    // Car constructor
    // Car destructor
    // Trye destructor
    // Engine destructor

在上面的例子中，成员对象engine定义在前，tyre定义在后，所以创建对象时先执行Engine的构造函数，再执行Tyre的构造函数，最后执行Car的构造函数。
析构时的顺序与其相反，先执行封闭类car的析构函数，再执行成员对象tyre的析构函数，最后执行成员对象engine的析构函数。

