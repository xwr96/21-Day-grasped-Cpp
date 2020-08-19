## 第四章

**1、类型转换运算符**

C++提供了一种新的类型转换运算符，专门用于基于继承的情形，这种情形在C语言编程中并不存在。4个C++转换类型如下;

* static_cast
* dynamic_cast
* reinterpret_cast
* const_cast
这4个类型转换运算符的使用语法相同：
`destnation_type resulr = cast_type<destination_type> (object_to_be_casted);`

**使用static_cast**

使用static_cast可将指针向上转换为基类类型，也可向下转换为派生类型，如下面的示例代码所示：
```
Base* pBase=new Derived ();   //constract a Derived object
Derived* pDerived=static_cast<Derived*>(pBase);   //ok!
```
PS : 将 Derived*转换为 Base*被称为向上转换，无需使用任何显式类型转换运算符就能进行这种转换：
```
Derived objDerived;
Base* pBase = &objDerived;   //ok
```
将 Base*转换为 Derived*被称为向下转换，如果不使用显式类型转换运算符，就无法进行这种转换.
除用于向上转换和向下转换外，static_cast还可在很多情况下将隐式类型转换为显式类型，以引起程序员或代码阅读人员的注意：
```
double dPi = 3.14159265;
int Num = static_cast<int>(dPi);
```
**使用dynamic_cast和运行阶段类型识别**

顾名思义，与静态类型转换相反，动态类型转换在运行阶段（即应用程序运行时）执行类型转换。可检查 dynamic_cast操作的结果，以判断类型转换是否成功。使用 dynamic_cast运算符的典型语法如下：
```
destination_type* pDest = dynamic_cast <class_type*> (pSource);
if (pDest)    //check for sucess of the casting operation before using pointer
    pDest->CallFunc();
```
实例代码如下：
```
#include<iostream>
#include<string>
using namespace std;
class Fish
{
public:
        virtual void Swim()
        {
               cout << "Fish swims in water" << endl;
        }
        virtual ~Fish() {}
};
class Tuna:public Fish
{
public:
        void Swim()
        {
               cout << "Tuna swims real fast in the sea" << endl;
     }
        void BecomeDinner()
        {
               cout << "Tuna become dinner in sushi" << endl;
     }
};
class Carp :public Fish
{
public:
        void Swim()
        {
               cout << "Carp swims real slow in the lake" << endl;
        }
        void Talk()
        {
               cout << "Carp talked crap" << endl;
        }
};
void DetectFishType(Fish* InputFish)
{
        Tuna* pIsTuna = dynamic_cast <Tuna*>(InputFish);
        if (pIsTuna)
        {
               cout << "Detect Tuna. making Tuna Dinner:" << endl;
               pIsTuna->BecomeDinner();
        }
        Carp* pIsCarp = dynamic_cast <Carp*>(InputFish);
        if (pIsCarp)
        {
               cout << "Detect Carp. making Carp Talk:" << endl;
               pIsCarp->Talk();
        }
        cout << "Verifing type using virtual Fish::Swim:" << endl;
        InputFish->Swim();
}
int main()
{
        Carp myLunch;
        Tuna myDinner;
        DetectFishType(&myDinner);
        cout << endl;
        DetectFishType(&myLunch);
        return 0;
}
```
程序输出结果：
```
Detect Tuna. making Tuna Dinner:
Tuna become dinner in sushi
Verifing type using virtual Fish::Swim:
Tuna swims real fast in the sea

Detect Carp. making Carp Talk:
Carp talked crap
Verifing type using virtual Fish::Swim:
Carp swims real slow in the lake
```
**使用reinterpret_cast**

reinterpret_cast是C++中与C风格类型转换最接近的类型转换运算符。它让程序员能够将一种对象类型转换为另一种，不管它们是否相关；也就是说，它使用如下所示的语法强制重新解释类型：
```
Base* pBase=new Derived ();
CUnrelated * pUnrelated = reinterpret_cast<CUnrelated*>(pBase);
```
这种类型转换实际上是强制编译器接受static_cast通常不允许的类型转换，通常用于低级程序（如驱动程序）.

*注意*：使用reinterpret_cast时，程序员将收到类型转换不安全（不可移植）的警告。应尽量避免在应用程序中使用 reinterpret_cast。

**使用const_cast**

const_cast让程序员能够关闭对象的访问修饰符 const。您可能会问：为何要进行这种转换？在理想情况下，程序员将经常在正确的地方使用关键字const。不幸的是，现实世界并非如此。如下面代码所示
```
class SomeClass
{
public:
    //……
    void DisplayMembers;   //a display function ought to be const
};
```
然而，DisplayMembers()本应为 const 的，但却没有这样定义。如果 SomeClass 归您所有，且源代码受您控制，则可对DisplayMembers()进行修改。然而，在很多情况下，它可能属于第三方库，无法对其进行修改。在这种情况下，const_cast将是您的救星。
```
void DisplayMembers(const SomeClass& mData)
{
    mData.DisplayMembers();   //编译错误
    //reason for failure：call to a non-const member using a const refernence
}
```
所以应该这样修改：
```
void DisplayMembers(const SomeClass& mData)
{
    SomeClass& refData = const_cast<SomeClass&>(mData)
    refData.DisplayMembers():  //Allowed!
}
```
除非万不得已，否则不要使用const_cast来调用非const函数。一般而言，使用const_cast来修改const对象可能导致不可预料的行为。

**2、宏和模板简介**

**2.1 预处理器与编译器**

**定义常量**

`#define identifier value`

**使用宏避免多次包含**

在预处理器看来，两个头文件彼此包含对方会导致递归问题。为避免这种问题，可结合使用宏以及预处理器编译指令#ifndef和#endif。
包含<header2.h>的head1.h类似于下面这样：
```
#ifndef HEADER1_H_
#define HEADER1_H_
#include<header2.h>
class Class1
{
    //code
};
#endif   //end of header1.h
```
header2.h与1差不多，替换一下就可以了。

预处理器首次处理header1.h并遇到#ifndef后，发现宏HEADER1_H_还未定义，因此继续处理。#ifndef后面的第一行定义了宏HEADER1_H_，确保预处理器再次处理该文件时，将在遇到包含#ifndef的第一行时结束，因为其中的条件为false。

**2.2、使用define定义宏函数**

典型代码：`#define SQUARE(x) ((x)*(x))`
宏函数经常进行简单的计算，有助于改善代码的性能。

**使用assert（）宏验证表达式**

assert宏让您能够插入检查语句，对表达式或变量的值进行验证。要使用assert宏，需要包含<assert.h>，其语法如下：
```
#include<assert.h>
int main()
{
    char* SayHello = new char [25];
    assert(SayHello!=NULL);
    //other code;
    delete[] SayHello;
    return 0;
}
```
上述代码在指针无效时可以指出来。

**2.3、模板简介**

模板声明以关键字template打头，接下来是类型参数列表。这种声明的格式`template <parameter list>`

示例代码如下：
```
#include<iostream>
#include<string>
using namespace std;
template <typename Type>
const Type& GetMax(const Type& value1, const Type& value2)
{
        if (value1 > value2)
               return value1;
        else
               return value2;
}
template <typename Type>
void Displaycomparison(const Type& value1, const Type& value2)
{
        cout << "GetMax(" << value1 << "," << value2 << ")=";
        cout << GetMax(value1, value2) << endl;
}
int main()
{
        int a1 = -101, int a2 = 2011;
        Displaycomparison(a1, a2);
        double d1 = 3.14, double d2 = 3.1415926;
        Displaycomparison(d1, d2);
        string Name1("Jack"), Name2("john");
        Displaycomparison(Name1, Name2);
        return 0;
}
```
模板函数不仅可以重用（就像宏函数一样），而且更容易编写和维护，还是类型安全的。 请注意，调用Displaycomparison时，也可显式地指定类型，如下所示：
`Displaycomparison<int>(Int1,Int2)`;
然而，调用模板函数时没有必要这样做。您无需指定模板参数的类型，因为编译器能够自动推断出类型；但使用模板类时，需要这样做。

**2.4、模板类**

模板类是模板化的 C++类，是蓝图的蓝图。使用模板类时，可指定要为哪种类型具体化类。这让您能够创建不同的Human对象，即有的年龄存储在 long long成员中，有的存储在 int成员中，还有的存储在short成员中。
对于模板，术语实例化的含义稍有不同。用于类时，实例化通常指的是根据类创建对象。但用于模板时，实例化指的是根据模板声明以及一个或多个参数创建特定的类型。
对于下面的模板声明：
```
template <typename T>
class TemplateClass
{
    T m_member;
};
```
使用模板是将这样编写代码
```
TemplateClass <int> IntTemplate;
```
这种实例化创建的特定类型称为具体化。
**声明包含多个参数的模板**
如下面代码所示：
```
template <typename T1, typename T2>
class HoldsPair
{
private:
    T1 Value1;
    T2 Value2;
public:
    HoldsPair(const T1& value1,const T2& value2)
    {
        Value1=value1;
        Value2=value2;
    }
    //other function declarations
};
```
实例化如下：
```
HoldsPair<int,double> pairIntDouble(6,1.99);
HoldsPair<int,int> pairIntDouble(6,500);
```

**包含默认参数的模板**
```
template <typename T1=int,typename T2=int>
class HoldsPair
{
    //……method declare
};
```
实例化可简写为：
```
HoldsPair <> PairIntDouble(6,500);
```
实例代码如下：
```
#include<iostream>
using namespace std;
template <typename T1=int,typename T2=double>
class HoldsPair
{
private:
        T1 Value1;
        T2 Value2;
public:
        HoldsPair(const T1& value1, const T2& value2)
        {
               Value1 = value1;
               Value2 = value2;
        };
        const T1 & GetFirstvalue() const
        {
               return Value1;
        };
        const T2& GetSecondvalue() const
        {
               return Value2;
        };
};
int main()
{
        HoldsPair<> mIntFloatPair(300, 10.09);
        HoldsPair<short,const char*> mshortstringPair(25, "learn template,love c++");
        cout << "the first object conntains-" << endl;
        cout << "Value 1:" << mIntFloatPair.GetFirstvalue() << endl;
       cout << "Value 2:" << mIntFloatPair.GetSecondvalue() << endl;
        cout << "the second object contains-:" << endl;
       cout << "Value 1:" << mshortstringPair.GetFirstvalue() << endl;
       cout << "Value 2:" << mshortstringPair.GetSecondvalue() << endl;
        return 0;
}
```
输出为：
```
the first object conntains-
Value 1:300
Value 2:10.09
the second object contains-:
Value 1:25
Value 2:learn template,love c++
```
**模板类和静态成员**

实例代码如下：
```
#include<iostream>
using namespace std;
template <typename T>
class TestStatic
{
public:
        static int StaticValue;
};
template<typename T> int TestStatic<T>::StaticValue;
int main()
{
        TestStatic<int> Int_Year;
        cout << "setting staticvalue for Int_Year to 2011" << endl;
        Int_Year.StaticValue = 2011;
        TestStatic<int> Int_2;
        TestStatic<double> Double_1;
        TestStatic<double> Double_2;
        cout << "setting staticvalue for Double_2 to 1011" << endl;
        Double_2.StaticValue = 1011;
        cout << "Int_2.StaticValue= " << Int_2.StaticValue << endl;
        cout << "Double_1.StaticValue= " << Double_1.StaticValue << endl;
        return 0;
}
```
输出为：
```
setting staticvalue for Int_Year to 2011
setting staticvalue for Double_2 to 1011
Int_2.StaticValue= 2011
Double_1.StaticValue= 1011
```
上述有一条程序初始化模板类的静态成员：`template<typename T> int TestStatic<T>::StaticValue`

*对于模板类的静态成员，通用的初始化语法如下：*
```
template<typename parameters> staticType ClassName<Template Arugments>::StaticVarName;
```

**使用static_assert执行编译阶段检查**

tatic_assert是C++11新增的一项功能，让您能够在不满足指定条件时禁止编译。语法如下：
`static_assert(expression being validated, "error message when check fails"`
要禁止针对类型int实例化模板类，可使用static_assert()，并将sizeof(T)与sizeof(int)进行比较，如果它们相等，就显示一条错误消息：
```
static_assert(sizeof(T)!=sizeof(int), "No int please!");
```
示例代码如下
```
#include<iostream>
using namespace std;
template <typename T>
class everythingbutInt
{
public:
        everythingbutInt()
        {
               static_assert(sizeof(T) != sizeof(int), "Not int please!");
        }
};
int main()
{
        everythingbutInt<int> test;
        return 0;
}
```
