## 第五章

**1、标准模版库介绍**

**STL容器**

**顺序容器**

顺序容器按顺序存储数据，如数组和列表。顺序容器具有插入速度快但查找操作相对较慢的特征。STL顺序容器包括：

*  std::vector——操作与动态数组一样，在最后插入数据；可将vector视为书架，您可在一端添加和拿走图书；
*  std::deque——与std::vector类似，但允许在开头插入或删除元素；
*   std::list——操作与双向链表一样。可将它视为链条，对象被连接在一起，您可在任何位置添加或删除对象；
*   std::forward_list——类似于std::list，但是单向链表，只能沿一个方向遍历。

**关联容器**

关联容器按指定的顺序存储数据，就像词典一样。这将降低插入数据的速度，但在查询方面有很大的优势。STL提供的关联容器包括：

*  std::set——存储各不相同的值，在插入时进行排序；容器的复杂度为对数；
*  std::unordered_set——存储各不相同的值，在插入时进行排序；容器的复杂度为常数。这种容器是C++11新增的；
*  std::map——存储键-值对，并根据唯一的键排序；容器的复杂度为对数；
*   std::unordered_map——存储键-值对，并根据唯一的键排序；容器的复杂度为对数。这种容器是C++11新增的；
*    std::multiset——与set类似，但允许存储多个值相同的项，即值不需要是唯一的；
*    std::unordered_multiset——与 unordered_set 类似，但允许存储多个值相同的项，即值不需要是唯一的。这种容器是C++11新增的；
*    std::multimap——与map类似，但不要求键是唯一的；
*    std::unordered_multimap——与unordered_map类似，但不要求键是唯一的。


**容器适配器**

容器适配器（Container Adapter）是顺序容器和关联容器的变种，其功能有限，用于满足特定的需求。主要的适配器类如下。

* std::stack：以 LIFO（后进先出）的方式存储元素，让您能够在栈顶插入（压入）和删除（弹出）元素。
* std::queue：以FIFO（先进先出）的方式存储元素，让您能够删除最先插入的元素。
* std::priority_queue：以特定顺序存储元素，因为优先级最高的元素总是位于队列开头。

**2、STL算法**

最常见的算法如下：

* std::find：在集合中查找值。
* std::find_if：根据用户指定的谓词在集合中查找值。
* std::reverse：反转集合中元素的排列顺序。
* std::remove_if：根据用户定义的谓词将元素从集合中删除。
* std::transform：使用用户定义的变换函数对容器中的元素进行变换
这些算法都是std命名空间中的模板函数，要使用它们，必须包含标准头文件<algorithm>。

典型代码如下：
```
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
int main()
{
        vector <int> vecIntegerArray;
        vecIntegerArray.push_back(50);
        vecIntegerArray.push_back(100);
        vecIntegerArray.push_back(4909);
        vecIntegerArray.push_back(1000);
        cout << "the contains of the vector are: " << endl;
        vector <int>::iterator iArrayWalker = vecIntegerArray.begin();
        while (iArrayWalker != vecIntegerArray.end())
        {
               cout << *iArrayWalker << endl;
               ++iArrayWalker;
        }
        vector <int>::iterator iElement = find(vecIntegerArray.begin(), 
vecIntegerArray.end(), 4909);
        if (iElement != vecIntegerArray.end())
        {
               int position = distance(vecIntegerArray.begin(), iElement);
               cout << "Value" << *iElement;
               cout << "find in the vector in position: " << position << endl;
               return 0;
        }
}
```
输出为：
```
the contains of the vector are:
50
100
4909
1000
Value4909find in the vector in position: 2
```
在上述代码中，有多个迭代器声明：
```
vector <int>::iterator iArrayWalker = vecIntegerArray.begin();
```
可将其简化为：
```
auto iArrayWalker = vecIntegerArray.begin();
```

**STL字符串类**

STL提供了一个专门为操纵字符串而设计的模板类：std::basic_string<T>，该模板类的两个常用具体化如下。

* std::string：基于char的std::basic_string具体化，用于操纵简单字符串。
* std::wstring：基于wchar_t的std::basic_string具体化，用于操纵宽字符串


**3、STL string类**

典型代码为：
```
#include<iostream>
#include<algorithm>
#include<string>
using namespace std;
int main()
{
        const char* String = "Hello,world!";
        cout << "String is: " << String << endl;
        string strfromconst(String);
        cout << "strfromconst is: " << strfromconst << endl;
        string str2("hello,string");
        string str2copy(str2);
        cout << "str2copy is: " << str2copy << endl;
        string strpartialcopy(String, 5);
        cout << "strpartialcopy is: " << strpartialcopy << endl;
        string strRepeatChars(10,"a");
        cout << "str3 is: " << strRepeatChars << endl;
        return 0;
}
```
输出为：
```
String is: Hello,world!
strfromconst is: Hello,world!
str2copy is: hello,string
strpartialcopy is: Hello
aaaaaaaaaa
```

**拼接字符串**

要拼接字符串，可使用运算符+=，也可使用成员函数append

**在string中查找字符或字符串**
STL string类提供了成员函数 find，该函数有多个重载版本，可在给定 string对象中查找字符或子字符串。

**截短STL string**

STL string类提供了 erase函数，可用于：


* 在给定偏移位置和字符数时删除指定数目的字符；
```
string strSample("hello string! wake up to beautiful day!");
strSample.erase(13,28);  //hello string!
```

* 在给定指向字符的迭代器时删除该字符；
```
strSample.erase(iCharS); //iterator points to a specific character
```

* 在给定由两个迭代器指定的范围时删除该范围内的字符。
```
strSample.erase(strSample.begin(), strSample.end());
```
**使用auto简化冗长的迭代器声明**

对于冗长的迭代器声明，C++11可帮助简化：

`string::iterator iCharS = find(strSample.begin(), strSample.end(),"S");`

可简化为：

`auto IcharS = find(strSample.begin(), strSample.end(),"S");`

编译器将根据std::find的返回类型自动推断变量iCharS的类型.

**字符串反转**

只需使用泛型算法 std::reverse：
```
string strSample("Hello String!");
reverse(strSample.begin(), strSample.end(),"S");
```

**字符串的大小写转换**

要对字符串进行大小写转换，可使用算法 std::transform，它对集合中的每个元素执行一个用户指定的函数。在这里，集合是string对象本身。
语法如下：
```
transform(strInput.begin(),strInput.end(),strInput..begin(),toupper); //转换为大写
transform(strInput.begin(),strInput.end(),strInput..begin(),tolower); //转换为小写
```
转换为大小写也可以直接用`toupper()`，`tolower()`函数。

**3、STL动态数组**

**实例化vector**
```
vector<int> vecDynamicArray;
```
要声明指向list中元素的迭代器，可以这样做：
```
std::list<int>::const_iterator iElementInSet;
```
 指定长度和初始化值的实例化：
 
```
vector<int> vecwithinitializedTenElements(10,90)
```
复制另一个vector的部分值
```
vector<int> vecsaomeElementCopy (vecwithinitializedTenElements.begin(), vecwithinitializedTenElements.begin()+5);
```

**使用push.back()插入数组**

**使用insert（）在指定位置插入元素**
```
vecIntegers.insert (vecIntegers.begin() , 25);
```
另一个版本让您能够指定插入位置、要插入的元素数以及这些元素的值（都相同）：
```
vecIntegers.insert (vecIntegers.end() , 2, 25);
```
还可将另一个vector的内容插入到指定位置：
```
vector<int> vecAnother (2, 30);
vecIntegers.insert (vecIntegers.ibegin()+1, vecAnother.begin(),vecAnother.end());
```
**使用数组语法访问vector中的元素**

使用[]访问vector的元素时，面临的风险与访问数组元素相同，即不能超出容器的边界。使用下标运算符（[ ]）访问vector的元素时，如果指定的位置超出了边界，结果将是不确定的（什么情况都可能发生，很可能是访问违规）。
更安全的方法是使用成员函数at()：
```
cout<<vecIntegersArray.at(2);
cout<<vecIntegersArray.at(Index);
```
**删除vector中元素**

vector支持使用pop_back函数将末尾的元素删除。使用pop_back将元素从vector中删除所需的时间是固定的，即不随vector存储的元素个数而异。
```
vecIntegers.pop_back()    //删除数组最后一个元素
```
vector的大小指的是实际存储的元素数，而 vector 的容量指的是在重新分配内存以存储更多元素前vector能够存储的元素数。因此，vector的大小小于或等于容量。

要查询vector当前存储的元素数，可调用size()：

`cout<<"Size: "<<vecIntegers.size();`

要查询vector的容量，可调用capacity()：

`cout<<"Capacity: "<<vecIntegers.capacity()<<endl;`

如果vector需要频繁地给其内部动态数组重新分配内存，将对性能造成一定的影响。在很大程度上说，这种问题可以通过使用成员函数**reserve (number)** 来解决。reserve函数的功能基本上是增加分配给内部数组的内存，以免频繁地重新分配内存。通过减少重新分配内存的次数，还可减少复制对象的时间，从而提高性能.

**STL deque类**

deque是一个STL动态数组类，与vector非常类似，但支持在数组开头和末尾插入或删除元素。要实例化一个整型deque，可以像下面这样做：
```
deque<int> dqIntegers;
```
要使用std::deque，需要包含头文件#include<deque>：

deque 与 vector 极其相似，也支持使用方法 push_back()和 pop_back()在末尾插入和删除元素。与vector一样，deque也使用运算符[]以数组语法访问其元素。deque与vector的不同之处在于，它还允许您使用push_front和pop_front在开头插入和删除元素。

**4、STL list和forward_list**

标准模板库（STL）以模板类std::list的方式向程序员提供了一个双向链表。双向链表的主要优点是，插入和删除元素的速度快，且时间是固定的。
要使用std::list类，需要包含头文件#include<list>

**基本的list操作**
```
list<int> listIntegers;   //实例化list
```
要声明一个指向list中元素的迭代器，可以像下面这样做：
```
list<int>::const_iterator iElementInSet;
```
迭代器让容器的实现彼此独立，其通用功能让您能够使用 vector中的值实例化 list，如下面代码所示：
```
vector<int> vecIntegers(10,2011);
list<int> listcontainsvec(vecIntegers.begin(),vecIntegers.end());
```

**在list开头或结尾插入元素**

与deque类似，要在list开头插入元素，可使用其成员方法push_front。要在末尾插入，可使用成员方法push_back。

**在list中间插入元素**

std::list的特点之一是，在其中间插入元素所需的时间是固定的，这项工作是由成员函数insert完成的。
1、`iterator insert(iterator pos,conat T& x);` 
insert函数接受的第1个参数是插入位置，第2个参数是要插入的值。该函数返回一个迭代器，它指向刚插入到list中的元素。
2、`void insert(iterator pos, size_type n, const T& x);`
该函数的第1个参数是插入位置，最后一个参数是要插入的值，而第2个参数是要插入的元素个数。
3、
```
template <class InputIterator>
void insert(iterator pos,InputIterator f, InputIterator l);
```
该重载版本是一个模板函数，除一个位置参数外，它还接受两个输入迭代器，指定要将集合中相应范围内的元素插入到list中。注意，输入类型InputIterator是一种模板参数化类型，因此可指定任何集合（数组、vector或另一个list）的边界。

**删除list中的元素**

list的成员函数erase有两种重载版本：一个接受一个迭代器参数并删除迭代器指向的元素，另一个接受两个迭代器参数并删除指定范围内的所有元素。
```
listIntegers.erase(listIntegers.begin(),2);
```

**对list中的元素进行反转和排序**

list 的一个独特之处是，指向元素的迭代器在 list 的元素重新排列或插入元素后仍有效。为实现这种特点，list提供了成员方法sort和reverse，虽然STL也提供了这两种算法，且这些算法也可用于list类。
list提供了成员函数reverse()，该函数没有参数，它反转list中元素的排列顺序：`listIntegers.reverse();`
list的成员函数sort()有两个版本，其中一个没有参数：`listIntegers.sort();`
另一个接受一个二元谓词函数作为参数，让您能够指定排序标准：
```
//二元谓词
bool SortPredicate_Descending(const int& lsh,const int& rsh)
{
    //定义排序的标准，返回排序后的数组
    return (lsh > rsh)
}
//use perdicate to sort a list
listIntegers.sort(SortPredicate_Descending);
```

**对包含对象的list进行排序以及删除其中的元素**

如果list的元素类型为类，而不是int等简单内置类型，如何对其进行排序呢？假设有一个包含地址簿条目的list，其中每个元素都是一个对象，包含姓名、地址等内容，如何确保按姓名对其进行排序呢？
答案是采取下面两种方式之一：

* 在list包含的对象所属的类中，实现运算符<。
* 提供一个排序二元谓词——一个这样的函数，即接受两个输入值，并返回一个布尔值，指出第一个值是否比第二个值小。
 典型代码：
```
#include<iostream>
#include<algorithm>
#include<string>
#include<vector>
#include<list>
using namespace std;
template <typename T>
void DisplayContents(const T& Inputs)
{
        for (auto iElements = Inputs.cbegin(); iElements != Inputs.cend(); ++iElements)
               cout << *iElements << endl;;
        cout << endl;
}struct ContactItem
{
        string strname;
        string strphone;
        string strdisplayrep;
        ContactItem(const string& strName, const string& strPhone)
        {
               strname = strName;
               strphone = strPhone;
               strdisplayrep = (strName + ": "+strPhone);
        }
        bool operator == (const ContactItem& itemToCompare) const
        {
               return (itemToCompare.strname == this->strname);
        }
        bool operator < (const ContactItem& itemToCompare) const
        {
               return (this->strname < itemToCompare.strname);
        }
        operator const char*() const
        {
               return strdisplayrep.c_str();
        }
};
bool SortOnPhoneNumber(const ContactItem& item1, const ContactItem& item2)
{
        return (item1.strphone < item2.strphone);
}
int main()
{
        list<ContactItem> Contacts;
        Contacts.push_back(ContactItem("xiewenrui,", "+15797639728"));
        Contacts.push_back(ContactItem("chenchuanxi", "+1234567"));
        Contacts.push_back(ContactItem("hezhiqiang", "+1345678"));
        cout << "list in initial idea: " << endl;
        DisplayContents(Contacts);
        Contacts.sort();
        cout << "order: " <<endl;
        DisplayContents(Contacts);
        Contacts.sort(SortOnPhoneNumber);
        cout << "after sort in phone number: " <<endl;
        DisplayContents(Contacts);
}
```
输出为：
```
list in initial idea:
xiewenrui,: +15797639728
chenchuanxi: +1234567
hezhiqiang: +1345678

order:
chenchuanxi: +1234567
hezhiqiang: +1345678
xiewenrui,: +15797639728

after sort in phone number:
chenchuanxi: +1234567
hezhiqiang: +1345678
xiewenrui,: +15797639728
```

**C++11**

从C++11起，您可以使用forward_list，而不是双向链表std::list。std::forward_list是一种单向链表，即只允许沿一个方向遍历。要使用std::forward_list，需要包含头文件#include<forward_list>
forward_list 的用法与 list 很像，但只能沿一个方向移动迭代器，且插入元素时只能使用函数push_front()，而不能使用push_back()。当然，总是可以使用insert()及其重载版本在指定位置插入元素。
forward_list的优点在于，它是一种单向链表，占用的内存比list稍少，因为只需指向下一个元素，而无需指向前一个元素。


**5、STL集合类**

容器 set和 multiset让程序员能够在容器中快速查找键，键是存储在一维容器中的值。set和multiset之间的区别在于，后者可存储重复的值，而前者只能存储唯一的值。

*要使用std::set或set::multiset类，需要包含头文件<set>：*

位于set中特定位置的元素不能替换为值不同的新元素，这是因为set将把新元素同二叉树中的其他元素进行比较，进而将其放在其他位置。

**STL set和multiset的基本操作**
```
//实例化
std::set<int> setIntegers;
std::multiset<int>  msetIntegers;
```
要声明一个指向set或multiset的迭代器
```
std::set<int>::const_iterator iElementInSet;
std::multiset<int>::const_iterator iElementInMultiset;
```
如果需要一个可用于修改值或调用非const函数的迭代器，应将const_iterator替换为iterator。
鉴于set和multiset都是在插入时对元素进行排序的容器，如果您没有指定排序标准，它们将使用默认谓词std::less，确保包含的元素按升序排列。
要创建二元排序谓词，可在类中定义一个operator()，让它接受两个参数（其类型与集合存储的数据类型相同），并根据排序标准返回true。下面是一个二元谓词，他按降序排序：
```
template <typename T>
struct SortDescending
{
    bool operator() (const T& lhs,const T& rhs) const
    {
        return (lhs > rhs);
    }
};
```
然后实例化set或multiset时指定该谓词：
```
set<int, ortDescending<int>> setIntegers;
multiset<int,ortDescending<int>> msetIntegers;
```

**在set和multiset插入元素**

setIntegers.insert();
msetIntegers.insert()
`multiset::count(value)`它返回multiset中有多少个元素存储了指定的值。

**在set和multiset中查找元素**

诸如set、multiset、map和multimap等关联容器都提供了成员函数find()，它让您能够根据给定的键来查找值：
```
auto iElementsFound = setIntegers.find(-1);
if (iElementsFound !=setIntegers.end())
    cout<<"Element"<<*iElementsFound <<""found"<<endl;
else
    cout<<"Elemenr not found in set!"<<endl;
```

**删除set和multiset中的元素**

诸如set、multiset、map和multimap等关联容器都提供了成员函数erase()，它让您能够根据键删除值：`setObeject.erase(key)`;
erase函数的另一个版本接受一个迭代器作为参数，并删除该迭代器指向的元素：`setObeject.erase(iElement);`
通过使用迭代器指定的边界，可将指定范围内的所有元素都从set或multiset中删除：`setObeject.erase(iLowerBound,iUpperBound);`

**C++11**

STL散列集合实现std::unordered_set和std::unordered_multiset。STL提供的容器类std::unordered_set就是**基于散列的set**。
要使用STL容器std::unordered_set或std::unordered_multiset，需要包含头文件<unordered_set>：`#include<unordered_set>`
相比于std::set，这个类的用法差别不大。然而，unordered_set的一个重要特征是，有一个负责确定排列顺序的散列函数：
```
unordered_set<int>::hasher HFn = usetInt.hash_function();
```
典型代码如下：
```
#include<iostream>
#include<algorithm>
#include<string>
#include<vector>
#include<unordered_set>
using namespace std;
template <typename T>
void DisplayContents(const T& Inputs)
{
        cout << "number of elements,size()=" << Inputs.size()<<endl;
        //最大桶数
        cout << "Max bucket count=" << Inputs.max_bucket_count() << endl;
        //负载系数
        cout << "Load factor= " << Inputs.load_factor() << endl;
        //最大负载系数
        cout << "mx_load_factor= " << Inputs.max_load_factor() << endl;
        cout << "unordered set contains:" << endl;
        for (auto iElements = Inputs.cbegin(); iElements != Inputs.cend(); ++iElements)
               cout << *iElements << endl;;
        cout << endl;
}
int main()
{
        unordered_set<int> usetInt;
        usetInt.insert(1000);
        usetInt.insert(-3);
        usetInt.insert(2011);
        usetInt.insert(100);
        DisplayContents(usetInt);
        usetInt.insert(999);
        DisplayContents(usetInt);
        return 0;
}
```
输出为：
```
number of elements,size()=4
Max bucket count=536870911
Load factor= 0.5
mx_load_factor= 1
unordered set contains:
1000
-3
2011
100

number of elements,size()=5
Max bucket count=536870911
Load factor= 0.625
mx_load_factor= 1
unordered set contains:
1000
-3
999
2011
100
```
