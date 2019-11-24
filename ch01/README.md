
### 《21天学通C++》 第一章
1、**在C++中，可不显示地指定变量类型，使用关键字auto**
例如：auto flag=true。这里将指定变量flag的类型交给了编译器，编译器会自动确定变量应为什么类型。
***PS***：auto时必须将变量初始化，否则会出现编译错误。
**2**、使用enum来定义变量只有一组特定的取值
例如：
```
enum RainbowColors
{ 
    violet=0,
    Indigo,
    Blue,
    Green,
    Yellow,
    Orange,
    Red
};
```
**RainbowColors MyWorldsColor = blue** ; #声明了常量MyWorldColor,这个常量只能取RainbowColors的值，声明枚举常量时，编译器把枚举值（voilet等）转化为整数，每个枚举值都比前一个大1.可以自己指定初始值，没有指定的话初始值为0.
**3**、为减少内存的占用，可以用**std::vector**来定义动态数组。需要包含头文件#include<vector>
例如：简要代码如下
   
```
#include<iostream>
#include<vector>
using namespace std;
int main()
{
   vector<int> DynArrNums(3)  #数组初始长度为3
   int AnotherNum=0;
   cin>>AnotherNum;
   DynArrNums.push_back(AnotherNum)  #使用这个函数将这个数字压入到矢量中
}
```
**4、C++字符串的使用**
需要使用头文件#include<string>才能使用**string 变量名**定义字符串变量。
例如：
```
#include<iostream>
#include<string>
using namespace std;
int main()
{
        string Greetings("Hello,std:string!");
        cout << Greetings << endl;
        cout << "enter a line o text:" << endl;
        string FirstLine;
        getline(cin, FirstLine);
        cout << "enter another" << endl;
        string SecLine;
        getline(cin, SecLine);
        cout << "concatenation:" << endl;
        string Contact = FirstLine + " " + SecLine;
        cout << Contact << endl;
        return 0;
   }
```
**5、语句、运算符**
要将一条放到两行中，可在第一行末尾添加反斜杠（\），也可将字符串字面量分成两个，如下例：
```
cout << "hello \
       world" << endl;
 或是：
 cout << "Hello"
       "world" <<endl;
```
使用后缀运算时，先将值赋给左值，再将右值递增或递减，左值都为执行前的旧值；使用前缀运算就相反，先将值递增或递减，再将结果赋给左值。
>++变量名 一般优于 变量名++
```
#include<bitset>
cin>>InputNum;
bitset<8> InputBits (InputNum)  #转换为二进制
bitset<8> BitwiseNOT = (~InputNum) #按位运算符Not
bitset<8> BitwiseAND = ( ) #AND运算
BitwiseOR = ( )
BitwiseXOR=( ) #异或运算
```
**6、控制流程序**
字符初始化一般用：
`char userselection= '\0`;
死循环一般用来检测操作系统USB接口是否连接了设备，只要系统一直在运行，这种活动就不会停止。
一个函数可以包含**多条return语句**。
*7、函数重载*
名称和返回类型相同，参数不同的函数称为重载函数。在应用程序中，如果使用不同的参数调用具有特定名称和返回类型的函数，重载函数将很有用。
**按引用传递函数**
即不是以返回值的方式而是以引用参数的方式提供给函数，如下所示：
```
#include<iostream>
#include<string>
using namespace std;
const double Pi = 3.1416;
void Area(double radius, double &result)
{
               result = Pi * radius *radius;
}
int main()
{
        cout << "enter radius:";
        double radius = 0;
        cin >> radius;
        double AreaFet = 0;
        Area(radius, AreaFet);
        cout << "the area is:" << AreaFet << endl;
        return 0;
}
```
**8、内联函数**
当定义一个函数时，执行函数的开销有可能非常高，所以使用关键字**inline**可以节省内存空间,将函数的内容直接放到它调用的地方，以提高代码的执行速度。但是应尽量少用关键字inline。
```
inline long DoubleNum(int InputNum)
{
    description;
}
```
**9、lambda函数**
lambda函数语法如下：
`[optional parameters] (parameter list){statements ;}`
代码如下：
```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
void DisplayNums(vector<int>&DynArray)
{
        for_each(DynArray.begin(), DynArray.end(), \
               [ ](int Element) {cout << Element << " "; });
        cout << endl;
}
int main()
{
        vector<int>MyNumbers;
        MyNumbers.push_back(501);
        MyNumbers.push_back(-1)；
        MyNumbers.push_back(25);
        MyNumbers.push_back(-35);
        DisplayNums(MyNumbers);
        cout << "sorting them in descend orders" << endl;
        sort(MyNumbers.begin(), MyNumbers.end(), \
               [ ](int Num1, int Num2) {return (Num2 < Num1); });
        DisplayNums(MyNumbers);
        return 0;
}
```
当定义函数提供形式参数时，要将所有有默认参数值的参数放在列表末尾，要么给所有参数都指定默认值。
**10、指针**
指针是一种指向内存单元的特殊变量。声明指针如下：
`int *pInteger = NULL;`    #初始化指针
使用引用运算符（&）获取变量的地址
可以声明一个int指针来储存变量的地址：
`int* pInteger = &age` 
**可将不同的内存地址赋给同一个指针变量，让它指向不同的值，如下个程序：**
```
#include<iostream>
using namespace std;
int main()
{
        int age = 30;
        int* pInteger = &age;
        cout << "pInteger points to Age now" << endl;
        cout << "pInteger=0x" << hex << pInteger << endl;
        int dogsage = 9;
        pInteger = &dogsage;
        cout << "pInteger dogsage=0x" << hex << pInteger << endl;
        return 0;
}
```
使用解除引用运算符（星号）访问指向的数据，如：
`*pInteger`  #访问数据
将sizeof()用于指针时，结果与指针指向的变量类型无关，而是取决于使用的编译器和针对的操作系统。
**动态内存分配**
使用**new**来动态的分配新的内存块。如果成功，new将返回指向一个指针，指向分配的内存；需要指定要为哪种数据类型分配内存。
`Type* Pointer = new Type;`   #Type为类型
还可以指定为多少个元素分配内存
`Type* Pointer = new Type[NumElements]`
例如：`int* Pointer = new int[10];`
使用new分配的内存最终都需要使用对应的delete进行释放
`delete Pointer;`
`delete[] Pointer`
**PS: delete只能释放new创建的内存，而不是用于包含任何地址的内存。**
将指针递增或递减时，其包含的地址将增加或减少指向的数据类型的sizeof(并不一定是1字节)。这样，编译器将确保指针不会指向数据的中间或末尾，而只会指向数据的开头。如下：
`Type* pType = Address;`
则执行++PType后，PType将包含指向Address+sizeof（Type）
示例代码如下：
```
#include<iostream>
using namespace std;
int main()
{
        cout << "how many integers you wish to enter?";
        int InputNums = 0;
        cin >> InputNums;
        int*pNumbers = new int[InputNums];
        int*pCopy = pNumbers;   //保存了该地址的拷贝
        cout << "sucessfully allocated memory for" << InputNums << "integers" << endl；
        for (int Index = 0; Index < InputNums; ++Index)
        {
               cout << "enter number" << Index << ":";
               cin >> *(pNumbers + Index);
        }
        cout << "displaying all numbers input:" << endl;
        for (int Index = 0, int* pCopy = pNumbers; Index < InputNums; ++Index)
        {
               cout << *(pCopy++) << ":" << endl;
        }
        delete[] pNumbers;
        return 0;
}
```
**将关键字const用于指针**
const指针有如下三种：
①指针指向的数据为常量，不能修改，但可以修改指针的地址，即指针可以指向其它地方
```
int HoursInDay = 24;
const int* pInteger = & HoursInDay   
```
②指针包含的地址是常量，不能修改，但可以修改指针指向的数据
```
int  HoursInDay  =24;
int* const pInteger = & HoursInDay 
```
③指针包含的地址及它指向的值都是常量，不能修改
```
int  HoursInDay  = 24;
const int* const pInteger = & HoursInDay 
```
务必初始化指针变量，如果不能将指针初始化为new返回的有效地址或它有效变量，可将其初始化为NULL。
**检查使用new发出的分配请求是否得到满足**
C++提供了两种确保指针有效的方法，默认方法是使用异常，即如果内存分配失败，将引发std::bad_alloc异常。这将导致应用程序中断执行。异常处理有以下两种方法：
```
//第一种
#include<iostream>
using namespace std;
int main()
{
        try
        {
               int* pAge = new int[5368709111];
               delete[] pAge;
        }
        catch (bad_alloc)
        {
               cout << "memory failed.ending program" << endl;
        }
        return 0;
}
```
```
//第二种
#include<iostream>
using namespace std;
int main()
{
        int* pAge = new(nothrow) int[0x1fffffff];
        if (pAge)
        {
               delete[] pAge;
        }
        else
        cout << "memory allocation failed" << endl;
        return 0;
}
```
