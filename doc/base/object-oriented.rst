########
面对对象
########

js 中的对象很令人困惑,
因为它没有明显的 ``class`` 等关键字来进行定义
(ES6 才加入 class 关键字, 见 `class`_).
也没有相关的语法来标识.

js 使用了一种称为 "原型继承" 的机制.

对象的定义
==========

要创建一个对象, 可以直接使用 ``{}``::

    var person = {
        age: 18,
        name: "nobody",
        sayHello: () => {console.log("Hello");}
    };

这个所谓的 "对象", 怎么看起来那么像 C 语言中的 "结构体" 呢?

这个对象直接绑定到了 ``persion`` 这个标识符上,
它可以像一个变量那样直接使用.

js 当中, 这样的 "对象" 被称为一个 "原型",
有且仅有一个实例, 还不能称之为 "类".

另外, 可以先定义一个对象的 "构造函数" 来进行对象的定义::

    function Person (name, age) {
        this.name = name;
        this.age = age;
        this.sayHello = () => {console.log("Hello");};
    }

构造函数其实就是普通的函数,
只不过在里面使用的 ``this`` 和调用时使用的 ``new`` 关键字暗藏玄机.

.. note::

    -   ``this`` 是一个关键字, 在任何时候都表示 "当前所处的对象"
    -   ``new`` 关键字在调用构造函数时使用.
        它将让该函数新建一个对象, 将 this 绑定到该对象上,
        并在函数结束后将这个对象返回.

        因此, 不需要在构造函数中显式地 ``return this``.

定义了构造函数后, 就可以这样新建一个实例::

    var someone = new Person(name="LLL", age=20);

这终于有点 "类" 的样子了.

.. warning::

    如果在调用构造函数时, 不加 ``new`` 关键字,
    那么它将表现得和普通函数一样.
    并且 ``this`` 将指向 ``window`` 对象.

    在 strict 模式下, 将直接报错.

继承
====

通过 ``new`` 一个构造函数,
得到的新对象就是 "继承" 了原型对象.

新对象的一切属性都是从原型对象处复制过来的.

对于新对象的数据成员, 这一点倒是理所应当.
但是对于对象的方法(函数成员),
继承时, 函数也是独立复制的.
也就是说, 创建了多少个实例,
这个函数就被复制了多少份.

这对于程序的执行效率来说是很不利的.

对于不需要重写的方法,
可以直接在对象的原型处进行定义,
而非在构造函数中定义.

原型对象可以通过构造函数的 ``prototype`` 属性来找到.

例如, 所有通过 ``Person`` 构造函数继承的对象,
其原型都是 ``Person.prototype``::

    function Person(name, age) {
        this.name = name;
        this.age = age;
        // this.hello 不在构造函数中定义.
    }

    Person.prototype.hello = () => {console.log("Hello");};

通过构造函数继承的对象, 在继承关系上保持了这样的一个链条::

    LaoWang -> Person.prototype -> Object.prototype -> null
    实例       实例的原型          基础原型            源头

``Person`` 等只是构造函数, 不要与原型 ``Person.prototype`` 混淆.

原型继承
========

要从一个原型, 继承到另一个原型,
可以如此定义构造函数::

    function Student (name, age) {
        Person.call(this, name, age);
        this.job = "study"
    }

任何函数对象都具有 ``call`` 方法, 这个和直接调用该函数是一样的.
将 this 传入 ``Person.call``, 则在 new ``Student`` 时,
会先初始化 ``Person`` 对象的属性, 并将属性绑定在 this 上.

原型继承, 仅仅是复制了原型的所有特征到新的对象之上.
而没有处理继承关系::

    XiaoJun -> Student.prototype -> Object.prototype -> null

要保持以下继承关系::

    ... -> Student.prototype -> Person.prototype -> ...

需要在中间使用一个中继, 常用的方法是使用一个空函数:

.. code-block:: javascript

    // 空函数
    function Blank () {}

    // 父原型
    function Person(name, age) {
        this.name = name;
        this.age = age;
    }

    // 子原型
    function Student(name, age) {
        Person.call(this, name, age);
        this.job = "study";
    }

    // 把 Blank 的原型指向父原型:
    Blank.prototype = Person.prototype;

    // 把子原型指向一个新的 Blank 对象，
    // Blank 对象的原型正好指向父原型.
    Student.prototype = new Blank();

    // 修复子原型的构造函数
    Student.prototype.constructor = Student;

经过这些繁琐的步骤, 才能完成一次完整的继承.

我们可以把这个步骤封装到一个函数里面去:

.. code-block:: javascript

    function inherit(Child, Parent) {
        var Blank = () => {};

        Blank.prototype = Parent.prototype;
        Child.prototype = new Blank();
        Child.prototype.constructor = Child;
    }

class
=====

class 是 ES6 才引入的概念, 而大量的 js 代码是基于 ES5 的.
(ES6 2015 年发布, 版本在这之前的浏览器毫无疑问只支持 ES5).
因此, 在之前花费了大量篇幅讲解原型继承.

class 用法就和大部分支持面对对象特性的语言一致了:

.. code-block:: javascript

    class Person {
        // 构造函数必须如此命名
        constructor (name, age) {
            this.name = name;
            this.age = age;
        }

        // 定义方法
        // 不需要 function 关键字
        hello () {
            window.alert("Hello, I'm " + this.name);
        }
    }

不过, 属性只能属于实例,
因此, 不能在类的定义中定义类数据成员:

.. code-block:: javascript

    // 报错
    class Counter {
        count = 0;
        constructor(name) {
            this.name = name;
            this.__proto__.count += 1
        }
    }

创建实例的方式同原型继承的方式:

.. code-block:: javascript

    XiaoMing = new Person("小明", 18);

继承就方便了很多, 使用了和 Java 相似的语法:

.. code-block:: javascript

    class Student extends Person {
        constructor (name, age) {
            // 使用 super 调用父类构造函数, 并且不需要传入 this
            super(name, age);
            this.job = "Study";
        }
    }

如果需要兼容老浏览器, 可以使用 `Babel <https://github.com/babel/babel>`_
将代码转换为浏览器识别的格式.
