### 第二章
**1、类和对象**
声明类使用关键字class，并在他后面依次包含类名、一组放在{ }内的成员属性和方法以及结尾的分号。
```
class Human
{
    //Data attributes:
    string Name;
    string Gender;
    //Methods:
    void Talk(string TextToTalk);
    void IntroduceYouself( );
    ……
} ;
```
就像int分配动态内存一样，也可以使用new为Human对象动态的分配内存;
```
Human* pAnotherHuman = new Human();  //动态的分配内存
delete pAnotherHuman;
```
使用句点运算符来访问成员
```
Human Tom;
Tom.DateBirth="1970";
```
使用指针运算符（->）访问成员
如果对象是使用new在自有储存区中实例化的，或者有指向对象的指针，则可以使用指针运算符（->）来访问成员属性和方法。
```
Human* pTom=new Human();
pTom->DataOfBirth="1970";
pTom->IntroduceYouself();
delete pTom;
```
下面结合代码来看一下：
```
#include<iostream>
#include<string>
using namespace std;
        class Human
        {
        private:
               string Name;
               int Age;
        public:
               void SetName(string HumansName)
               {
                       Name = HumansName;
               }
               void SetAge(int HumansAge)
               {
                       Age = HumansAge;
               }
               void IntroduceSelf()
               {
                       cout << "I am" + Name << "and am";
                       cout << Age << "years old" << endl;
               }
        };
        int main()
        {
               Human FirstMan;
               FirstMan.SetName("Adam");
               FirstMan.SetAge(30);
               FirstMan.IntroduceSelf();
        }
```
**2、构造函数**
构造函数是一种特殊的函数，它与类同名且不返回任何值。因此，Human类在声明内的构造函数声明类似于下面：
```
class Human
{
 public:
    Human( )
    {
          //代码
    }
}；
```
在类声明外定义构造函数的代码如下：
```
class Human
{
public:
    Human( );
 };
 Human :: Human()
 {
    //代码
 }
```
：：被称为作用域解析运算符。例如：Human::DateOfBirth指的是在Human类中声明的变量DateOfBirth，而::DateOfBirth表示全局作用域中的变量DateOfBirth。
包含初始化列表的构造函数的代码如下：
```
class Human
{
public:
  Human(string InputName = "Adam" , int Age = 25 )
            : Name(InputName) , Age(InputAge)
  {
        //代码
  }
}；
```
初始化列表由包含在括号当中的参数声明后面的冒号标识，冒号后面列出了各个成员变量及其初始化值。初始化值可以是参数，也可以是固定的值。
**3、析构函数**
析构函数在对象销毁时自动被调用。析构函数看起来也像一个与类同名的函数，但前面有一个波浪号（~）。因此，Human类的析构函数声明类似于下面这样,这是在类里面声明：
```
class Human
{
public:
    ~Human( )
    {
        // code here
    }
};
```
在类声明外定义析构函数：
```
class Human
{
public:
    ~Human();
};
Human::~Human()
{
    //code here
}
```
析构函数与构造函数的作用完全相反。析构函数是重置变量以及释放动态分配的内存和其他资源的理想场所。
析构函数典型代码如下：
```
#include<iostream>
#include<string>
using namespace std;
class MyString
{
private:
        char* Buffer;
public:
        MyString(const char* InitialInput)
        {
               if (InitialInput != NULL)
               {
                       Buffer = new char[strlen(InitialInput + 1)];
                       strcpy(Buffer, InitialInput);
               }
               else
                       Buffer = NULL;
        }
        ~MyString()
        {
               cout << "Invoking destructor,cleaning up" << endl;
               if (Buffer != NULL)
                       delete[] Buffer;
        }
        int GetLength()
        {
               return strlen(Buffer);
        }
        const char* GetString()
        {
               return Buffer;
        }
};
int main()
{
        MyString SayHello("Hello from string class:");
        cout << "string buffer in MyString is:" << SayHello.GetLength();
        cout << "characters long" << endl;
        cout << "Buffer contains:";
        cout << "Buffer contains:" << SayHello.GetString();
}
```
程序运行输出为：
```
string buffer in MyString is:24characters long
Buffer contains:Buffer contains:Hello from string class:Invoking destructor,cleaning up
```
析构函数不能重载，每个类都只能有一个析构函数。如果你忘记实现一个析构函数，编译器将创造一个伪（dummy）析构函数并调用他。伪析构函数为空，既不释放动态分配的内存。
**复制构造函数**
浅复制：复制类对象时，将复制其指针成员，都不复制指针指向的缓冲区，造成两个对象指向同一块动态分配的内存，会威胁程序的稳定性。
深复制：所以要将浅复制的参数复制变成地址传递，即按参数引用传递而不是进行二进制复制。代码示例如下
```
#include<iostream>
#include<string>
using namespace std;
class MyString
{
private:
        char* Buffer;
public:
        MyString(const char* InitialInput)
        {
               cout << "Constructor:creating new String" << endl;
               if (InitialInput != NULL)
               {
                       Buffer = new char[strlen(InitialInput + 1)];
                       strcpy(Buffer, InitialInput);
                       cout << "Buffer points to:0x" << hex;
                       cout << (unsigned int*)Buffer << endl;
               }
               else
                       Buffer = NULL;
        }
        //复制构造函数
        MyString(const MyString& CopySource)
        {
               cout << "copy constructeor:copy from MyString" << endl;
               f (CopySource.Buffer != NULL)
               {
                       Buffer = new char[strlen(CopySource.Buffer) + 1];
                       strcpy(Buffer, CopySource.Buffer);
                       cout << "Buffer points to:0x" << hex;
                       cout << (unsigned int*)Buffer << endl;
               }
               else
                       Buffer = NULL;
        }
        ~MyString()
        {
               cout << "Invoking destructor,cleaning up" << endl;
               if (Buffer != NULL)
                       delete[] Buffer;
        }
        int GetLength()
        {
               return strlen(Buffer);
        }
        const char* GetString()
        {
               return Buffer;
        }
};
void UseMyString(MyString Input)
{
        cout << "Sting Buffer in mystring is" << Input.GetLength();
        cout << "characters long" << endl;
        cout << "Buffer contains:" << Input.GetString() << endl;
        return;
}
int main()
{
        MyString SayHello("Hello from string class:");
        UseMyString(SayHello);
}
```
程序运行输出：
```
Constructor:creating new String
Buffer points to:0x004BD5A0
copy constructeor:copy from MyString
Buffer points to:0x004BD5E8
Sting Buffer in mystring is18characters long
Buffer contains:Hello from string class:
Invoking destructor,cleaning up
Invoking destructor,cleaning up
```
MyString包含原始指针成员char* Buffer，一般不要为类成员声明原始指针，而应该使用std::string。在没有原始指针的情况下，都不需要编写复制构造函数，这是因为编译器添加的默认复制构造函数将调用成员对象（如：std::string）的复制构造函数。
**只能有一个实例的单例类**
单例的概念使用私有构造函数、私有赋值函数和静态实例成员。要创建单例类，关键字static必不可少。
**重点知识点**
参数是引用，如果不加&的话就是平常参数，也就是传值参数。传值参数，如果实参在函数中被修改时，外面的这个变量并不会改变。
引用参数，也就是在形参加上&，如果实参在函数中被修改的同时，外面的这个变量也会被修改。例：
```
int a=10;
void add1(int x)
{
    x++;
}
add1(a);
```
执行完之后a还是10。
```
int a=10;
void add2(int &x)
{
    x++;
}
add2(a);
```
执行完之后a=11。
**禁止在栈中实例化的类**
数据库类把析构函数设置为私有，只能使用new在自由储存区中创建其对象。如下代码：
```
class MonsterDB
{
private:
    ~MonsterDB( ) {};
public:
    static void DestroyInstance(MonsterDB* pInstance)
    {
        delete pInstance;
    }
    //……imagine a few other methods
};
int main()
{
    MonsterDB* pMyDatabase = new MonsterDB();
    MonsterDB :: DestroyInstance(pMyDatabase);
    return 0;
}
```
**this指针**
在类中，关键字this包含当前对象的地址，换句话说，其值为&object。
**结构不同于类的地方**
关键字struct来自于c语言，它与类极其相似。除非指定了，结构中的成员默认为公有，而类默认为私有；结构默认以公有方式继承，而类默认以私有方式继承。
**4、声明友元函数**
不能从外部访问类的私有数据成员和方法，但这条规则不适用于友元类和友元函数。要声明友元类和友元函数，可使用关键字**friend**，如下述所示。
```
#include<iostream>
#include<string>
using namespace std;
class Human
{
private:
        string Name;
        int Age;
        friend void DisplayAge(const Human& Person);
public:
        Human(string InputName, int InputAge)
        {
               Name = InputAge;
               Age = InputAge;
        }
};
void DisplayAge(const Human& person)
{
        cout << person.Age << endl;
}
int main()
{
        Human FirstMan("Adam", 25);
        cout << "Accessing private member age via:";
        DisplayAge(FirstMan);
        return 0;
}
```
输出为：
```
Accessing private member age via:25
```
与函数一样，也可以将外部类指定为可信任的朋友。
```
friend class Utility;
class Utility
{
    //code here;
}
```
**5、实现继承**
基类比如鸟，将定义鸟的基本属性，如有羽毛和翅膀等等；派生类为乌鸦、鹦鹉等等将继承这些属性并进行定制。有些派生类能继承两个基类，这种称为多继承。
**C++派生语法**
```
//declaring a super class
class Base
{
    //……base class members
};
//declaring a sub_class
class Derived: acess-specifier Base
{
    //……derived class members
};
```
其中acess-specifier可以是public（这是最常见的，表示派生类是一个基类）、private或protected（表示派生类有一个基类）。
示例代码：
```
#include<iostream>
#include<string>
using namespace std;
class Fish
{
public:
        bool FreshWaterFish;
        void Swim()
        {
               if (FreshWaterFish)
                       cout << "swims in lake" << endl;
               else
                       cout << "swims in sea" << endl;
        }
};
class Tuna :public Fish
{
public:
        Tuna()
        {
               FreshWaterFish = false;
        }
};
class Carp :public Fish
{
public:
        Carp()
        {
               FreshWaterFish = true;
        }
};
int main()
{
        Carp myLunch;
        Tuna myDinner;
        cout << "my food to swim" << endl;
        cout << "lunch:";
        myLunch.Swim();
        cout << "Dinner:";
        myDinner.Swim();
        return 0;
}
```
输出为：
```
my food to swim
lunch:swims in lake
Dinner:swims in sea
```
**访问限定符protected**
将属性声明为protected时，相当于允许派生类和友元类访问他，但禁止在继承层次结构外部（包括main（）访问他）。
```
class Fish
{
protected:
    bool FreshWaterFish;
    //code here
}
```
**基类初始化——向基类传递参数**。如下面代码所示：
```
class Base
{
public:
    Base(int SomeNumber)  //overload constructor
    {
        //Do Something with Somenumber
    }
};
class Derived : public Base
{
public:
Derived(): Base(25)  //instance class Base with argument 25
{
    //derived class constructor code
}
};
```
上述代码改一下：
```
//基类改一下
public :
    Fish(bool IsFreshWater) : FreshWaterFish(IsFreshWater) {}
//继承类Tuna改一下
public:
    Tuna() : Fish(false) {}
//继承类Carp改一下
public:
    Carp() : Fish(true) {}
```
**在派生类中覆盖基类的方法**
如果派生类实现了从基类继承的函数，且返回值和特征值相同，就相当于覆盖了基类的方法，即派生类有一个和基类一模一样的函数，则程序运行是输出了派生类的函数结果。
**调用基类中被覆盖的方法**
1)如果要在main（）中调用Fish::Swim(),需要使用作用域解析符（：：）把上述的代码改一下就可以。
```
//在Carp类中
void Swim()
{
    cout<<"Carp swims is too low"<<endl;
    Fish::Swim();
}
//在main（）函数中
cout<<"Dinner:";
myDinner.Fish::Swim();
```
2)在Carp类中，使用关键字using解除对Fish::Swim()的隐藏
```
//在Carp类中
class Carp
{
public:
    using Fish::Swim;  //去除基类隐藏的方法
}
```
3）在Carp类中，覆盖Fish::Swim()的所有重载版本。
**6、析构顺序和构造顺序**

继承时，构造函数和析构函数的调用顺序
1、先调用父类的*构造函数*，再初始化成员，最后调用自己的构造函数
2、先调用自己的*析构函数*，再析构成员，最后调用父类的析构函数
3、如果父类定义了有参数的构造函数，则自己也必须自定义带参数的构造函数
4、父类的构造函数必须是参数列表形式的
5、多继承时，class D: public Base2, Base1, Base的含义是：class D: public Base2, private Base1, private Base，而不是class D: publicBase2, public Base1, public Base6.多继承时，调用顺序取决于class D: public Base2, public Base1, public Base的顺序，也就是先调用Base2,再Base1，再Base。但是有虚继承的时候，虚继承的构造函数是最优先被调用的。
**私有继承：private**
即便是Base类的公有成员和方法，也只能被Derived类使用，而无法通过Derived实例来使用他们。
**保护继承：protected**
保护继承与私有继承的相似之处：
1）他也表示has-a关系
2）他也让派生类能够访问基类的所有公类和保护成员
3）在继承结构层次外面，也不能通过派生类实例访问基类的公有成员
随着继承结构的加深，保护继承与私有继承有些不同
```
class Derived2:protected Derived
{
    //can acess menber of Base
};
```
在保护继承层次结构中，子类的子类（Derived）能够访问Base类的公有成员。
将Base类作为子类的私有成员被称为组合或聚合，如下面代码所示：
```
class Car
{
private:
    Motor heartOfcar;
public:
void Move()
{
    heartOfca.SwitchInginition();
    //code here;
]
];
```
这能够轻松在Car类中添加Motor成员而无需改变继承结构层次
**7、切除问题**
```
Derived objectDerived;
Base objectBase = objectDerived;
```
或者：
```
void FunuseBase(Base Input)
……
Derived objectDerived;
FunuseBase(objectDerived);
```
上述代码都将Derived对象复制给Base对象，一个是显示复制，一个是通过传递函数。在这种情况下，编译器将只会复制objectDerived的Base部分，即不是整个对象，而是Base能容纳的部分。这种无意间裁剪数据，导致Derived变成Base的行为称为切除（slice）
**8、多继承**
C++允许继承多个基类
```
class Derived : acess-specifier Base1,cess-specifier Base2,……
{
    //class menbers
};
```
