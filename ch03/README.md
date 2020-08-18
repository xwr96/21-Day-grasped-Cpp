## 第三章

**1、多态**

多态（Polymorphism）是面向对象语言的一种特征，让你能够以类似的方式处理不同类型的对象。

**1.1、使用虚函数实现多态行为**

可通过Fish指针或Fish引用访问Fish对象，这种指针或引用可指向Fish、Carp等对象。但你不需要知道也不关心它们指向的是哪种对象。可以用下面代码所示：
```
pFish->Swim();
myFish.Swim();
```
你希望通过这种指针或引用调用Swim()时，如果它们指向的是Tuna对象，则可像Tuna那样游泳，若指向的是Carp对象，则可以像Carp那样游泳，若指向的是Fish，则可像Fish那样游泳。为此，可在基类Fish中将Swim声明为虚函数;
```
class Base
{
    virtual ReturnType FunctionName (parameter List);
};
class Derived
{
    FunctionName (parameter List);
};
```
通过使用关键字virtual，可确保编译器调用覆盖版本。也就是说，如果Swim()被声明为虚函数，则将参数myFish（其类型为Fish&）设置为一个Tuna对象时，myFish.Swim()将执行Tuna::Swim()，程序如下所示：
```
#include<iostream>
#include<string>
using namespace std;
class Fish
{
public:
        virtual void Swim()
        {
               cout << "Fish swims!" << endl;
        }
};
class Tuna :public Fish
{
public:
        void Swim()
        {
               cout << "Tuna swims!" << endl;
        }
};
class Carp :public Fish
{
public:
        void Swim()
        {
               cout << "Carp swims!" << endl;
        }
};
void MakeFishSwim(Fish& InputFish)
{
        InputFish.Swim();
}
int main()
{
        Tuna myDinner;
        Carp myLunch;
        MakeFishSwim(myDinner);
        MakeFishSwim(myLunch);
        return 0;
}
```
输出为：

Tuna swims!
Carp swims!

首先，根本没有调用Fish::Swim() ，因为存在覆盖版本 Tuna::Swim()和 Carp::Swim() ，它们优先于被声明为虚函数的Fish::Swim()。这很重要，它意味着在MakeFishSwim()中，可通过Fish&参数调用派生类定义的Swim()，而无需知道该参数指向的是哪种类型的对象。
 这就是多态：将派生类对象视为基类对象，并执行派生类的Swim()实现。
 
 **1.2、为什么需要虚构函数**
 
 上面的代码如果加入析构函数释放内存，对于使用new在自由储存区中实例化的派生类对象，如果将其赋值给基类指针，并通过该指针调用delete，**将不会调用派生类的析构函数**，这可能导致资源未释放、内存泄露等问题，必须引起重视。要避免这种问题，可将基类析构函数声明为虚函数。如下面代码所示：
 
```
class Fish
{
public:
    Fish()
    {
        cout<<"construct Fish"<<endl;
    }
    virtual ~Fish()
    {
        cout<<"Destroy Fish"<<endl;
    }
};
```
输出还表明，无论Tuna对象是使用new在自由存储区中实例化的，还是以局部变量的方式在栈中实例化的，构造函数和析构函数的调用顺序都相同。

**1.3、抽象基类和纯虚函数**

不能实例化的基类被称为抽象基类，这样的基类只有一个用途，那就是从它派生出其他类。在 C++中，要创建抽象基类，可声明纯虚函数。
以下述方式声明的虚函数被称为纯虚函数：
```
class AbstractBase
{
public:
    virtual void DoSomething()=0;  //纯虚函数
};
该声明告诉编译器， AbstractBase的派生类必须实现方法DoSomething（）;
class Derived : public AbstractBase
{
public:
    void DoSomething()
    {
        cout<<"Implemented virtual function"<<endl;
    }
};
```
AbstractBase类要求Derived类必须提供虚方法DoSomething()的实现。这让基类可指定派生类中方法的名称和特征（Signature），即指定派生类的接口。
虽然不能实例化抽象基类，但可将指针或引用的类型指定为抽象基类。抽象基类提供了一种非常好的机制，让您能够声明所有派生类都必须实现的函数。抽象基类常被简称为ABC。 ABC有助于约束程序的设计。

**1.4、使用虚继承解决菱形问题**

如图程序所示，如果没有使用虚继承，则会输出：
```
Animal constructor
Animal constructor
Animal constructor
Platypus constructor
```
输出表明，由于采用了多继承，且 Platypus 的全部三个基类都是从 Animal 类派生而来的，因此第38行创建Platypus实例时，自动创建了三个Animal实例。Animal 有一个整型成员——Animal::Age，为方便说明问题，将其声明成了公有的。如果您试图通过Platypus 实例访问 Animal::Age（如第 42 行所示），将导致编译错误，因为编译器不知道您要设置Mammal::Animal::Age、Bird::Animal::Age还是Reptile::Animal::Age。更可笑的是，如果您愿意，可以分别设置这三个属性：
```
duckBilledp.Mamal::Animal::Age=25;
duckBilledp.Bird::Animal::Age=25;
duckBilledp.Reptile::Animal::Age=25;
```
显然，鸭嘴兽应该只有一个Age属性，但您希望Platypus类以公有方式继承 Mammal、Bird 和Reptile。解决方案是使用虚继承。如果派生类可能被用作基类，派生它是最好使用关键字virtual：
```
class Derived1 : public virtual Base
{
    //members and funnctions
};
class Derived2 : public virtual Base
{
    //members and funnctions
};
```
在继承层次结构中，继承多个从同一个类派生而来的基类时，如果这些基类没有采用虚继承，将导致二义性。这种二义性被称为菱形问题（Diamond Problem）。其中的“菱形”可能源自类图的形状。

**注意**：C++关键字virtual的含义随上下文而异（我想这样做的目的很可能是为了省事），对其含义总结如下：
在函数声明中，virtual意味着当基类指针指向派生对象时，通过它可调用派生类的相应函数。从Base类派生出Derived1和Derived2类时，如果使用了关键字virtual，则意味着再从Derived1和Derived2派生出Derived3时，每个Derived3实例只包含一个Base实例。也就是说，关键字virtual被用于实现两个不同的概念。

**1.5、可将复制构造函数声明为虚函数吗**

根本不可能实现虚复制构造函数，因为在基类方法声明中使用关键字virtual时，表示它将被派生类的实现覆盖，这种多态行为是在运行阶段实现的。而构造函数只能创建固定类型的对象，不具备多态性，因此C++不允许使用虚复制构造函数。虽然如此，存在一种不错的解决方案，就是定义自己的克隆函数来实现上述目的：

虚函数Clone模拟了虚复制构造函数，但需要显式地调用，如下面程序所示：

输出为：
```
Tuna swims fast in the sea
Carp swims slow in the lake
Tuna swims fast in the sea
Carp swims slow in the lake
```
在main()中，第 40～44行声明了一个静态基类指针（Fish*'）数组，并各个元素分别设置为新创建的Tuna、Carp、Tuna和Carp对象。注意到myFishes数组能够存储不同类型的对象，这些对象都是从Fish派生而来的。这太酷了，因为为本书前面的大部分数组包含的都是相同类型的数据，如int。如果这还不够酷，您还可以在循环中使用虚函数Fish::Clone将其复制到另一个Fish*'数组（myNewFishes）中，如第48行所示。注意到这里的数组很小，只有4个元素，但即便数组长得多，复制逻辑也差别不大，只需调整循环结束条件即可。第 52 行进行了核实，它通过新数组的每个元素调用虚函数 Swim()，以验证Clone()复制了整个派生类对象。

**2、运算符类型和运算符重载**

C++运算符从语法层面来看，除使用关键字operator外，运算符与函数几乎没有差别。
`return_type operator operator_symbol(……parameter list……)`
其中operator_symbol是程序员可以定义的几种运算符之一。C++运算符分为单目运算符和双目运算符。

**2.1、单目运算符**

在类声明中编写单目前缀递增运算符（++），可采用如下语法：
```
Date& operator ++ ( )
{
    //operator implementation code
    return *this;
}
```
而后缀递增运算符（++）的返回值不同，且有一个输入参数
```
Date opeator ++ (int)
{
    //store a copy of the current state of the object,before incrementing date
    Date Copy (*this);
    //operator implementation code
    return Copy;
}
```
前缀和后缀递减运算符的声明语法与递增运算符类似，只是将声明中的++替换成--。
示例代码如下：
```
#include<iostream>
#include<string>
using namespace std;
class Date
{
private：
        int Day;
        int Month;
        int Year;
public:
        Date(int InputDay, int InputMonth, int InputYear)
               :Day(InputDay), Month(InputMonth), Year(InputYear) {};
        Date& operator ++ ()
        {
               ++Day;
               return *this;
        }
        Date operator ++ (int)
        {
               Date Copy(Day, Month, Year);
               ++Day;
               return Copy; 
        }
        Date& operator -- ()
        {
               --Day;
               return *this;
        }
        Date operator -- (int)
        {
               Date Copy(Day, Month, Year);
               --Day;
               return Copy;
        }
        void DispalyDate()
        {
              cout << Day << " / " << Month << " / " << Year << endl;
        }
};
int main()
{
        Date Holiday(25, 12, 2011);
        cout << "The Day Holiday is:" ;
        Holiday.DispalyDate();
        ++Holiday;
        cout << "day after:";
        Holiday.DispalyDate();
        --Holiday;
        cout << "day before:";
        Holiday.DispalyDate();
        return 0;
}
```
输出为：
```
The Day Holiday is:25 / 12 / 2011
day after:26 / 12 / 2011
day before:25 / 12 / 2011
```
在上面的代码中，如果cout<<Holiday将出现编译错误，因为cout不知如何解读Holiday对象，cout能够很好地显示 const char※，因此，要让 cout能够显示Date对象，只需添加一个返回 const char*的运算符：*
```
operator const char*()
{
    //operator impletmentation that returns a char*
}
```
所以上述代码可以改为：
```
#include<sstream>
#include<string>
class Date
{
    operator const char*()
    {
        ostringstream formattedDate;
        formattedDate<<Day<<" / "<<Month<<" / "<<Year<<endl;
        DateInString=formattedDate.str();
        return DateInString.c_str();
    }
};
```
就可以直接输出cout<<Holiday。

**2.1.2、解除引用运算符和成员选择运算符（->）**

智能指针使用了引用运算符和（->）成员选择运算符，需要添加头文件`#include<memory>`。智能指针类std::unique_ptr实现了上述用法。

**2.2、双目运算符**

对两个操作数进行操作的运算符称为双目运算符。以全局函数或静态成员函数的方式实现的双目运算符的定义如下：
```
return_type operator_type(parameter1,parameter2);
```
以类成员实现的双目运算符如下：
```
return_type operator_type(parameter);
```
以类成员的方式实现的双目运算符只接受一个参数，其原因是第二个参数通常是从类属性获得的。

**双目加法与双目减法运算符**

示例代码如下：
```
#include<iostream>
#include<string>
using namespace std;
class Date
{
private:
        int Day,Month,Year;
public:
        Date(int InputDay, int InputMonth, int InputYear)
               :Day(InputDay), Month(InputMonth), Year(InputYear) {};
        Date operator + (int DaysToAdd)
        {
               Date newDate(Day + DaysToAdd, Month, Year);
               return newDate;
        }
        Date operator - (int DayToSub)
        {
               return Date(Day-DayToSub,Month,Year); 
        }
        void DispalyDate()
        {
              cout << Day << " / " << Month << " / " << Year << endl;
        }
};
int main()
{
        Date Holiday(25, 12, 2011);
        cout << "The Day Holiday is:" ;
        Holiday.DispalyDate()；
        Date PreviousHoliday(Holiday - 16);
        cout << "PreviousHoliday is:";
        PreviousHoliday.DispalyDate();
        Date NextHoliday(Holiday + 6);
        cout << "Nextholiday on:";
        NextHoliday.DispalyDate();
        return 0;
}
```
输出为：
```
The Day Holiday is:25 / 12 / 2011
PreviousHoliday is:9 / 12 / 2011
Nextholiday on:31 / 12 / 2011
```
**运算符提高了类的可用性**，但实现的运算符必须合理。

一般运算符都是下面这种代码格式：
```
void operator += (int DaysToAdd)
{
    Day+=DaysToAdd;
}
```
**重载等于运算符（==）和不等于运算符（！=）**
```
bool operator == (const Date& compareTo)
{
    return ((Day==compareTo.Day)&&(Year==compareTo.Year)
             &&(Month==compareTo.Month));
}
bool operator != (const Date& compareTo)
{
    return !(this->operator==(compareTo));
}
```

**重载赋值运算符（=）**

跟复制构造函数一样，为确保进行深复制，需要提供复制赋值运算符
```
Classtype& operator = (const Classtype& CopySource)
{
    if (this != &CopySource)  //protection against copy into self
    {
        //Assignment operator impletment
    }
    return *this;
}
```

**2.3、下标运算符**

下标运算符让您能够像访问数组那样访问类，其典型语法如下：
```
return_type& operator [] (subscript_type& subscript);
```
经典代码如下：
```
const char& operator [] (int Index) const
{
    if(Index<GetLength())
      return Buffer[Index];
}
```
**2.4、函数运算符operator**

operator()让对象像函数，被称为函数运算符。函数运算符用于标准模板库（STL）中，通常是 STL算法中。其用途包括决策。根据使用的操作数数量，这样的函数对象通常称为单目谓词或双目谓词。
示例代码如下：
```
#include<iostream>
#include<string>
using namespace std;
class CDisplay
{
public:
        void operator () (string Input) const
        {
               cout << Input << endl;
        }
};
int main()
{
        CDisplay mDisplayFuncObject;
        mDisplayFuncObject("Dispaly this string!");
        return 0;
}
```
这个运算符也称为operator()函数，对象CDisplay也称为函数对象或functor。

**用于高性能编程的移动构造函数和移动复制函数**

移动构造函数和移动赋值运算符乃性能优化功能，属于C++11标准的一部分，旨在避免复制不必要的临时值（当前语句执行完毕后就不再存在的右值）。对于那些管理动态分配资源的类，如动态数组类或字符串类，这很有用。
典型代码如下：
```
//移动构造函数声明
Myclass(Myclass&& MoveSource)   //重要&&
{
    PtrResource = MoveSource.PtrResource;
     MoveSource.PtrResource = NULL;
}
//移动赋值函数
MyClass& operator= (MyClass&& MoveSource)
{
    if (this != &MoveSource)
    {
        delete[] PtrResource;
        PtrResource = MoveSource.PtrResource;
        MoveSource.PtrResource; = NULL;
    }
}
```
