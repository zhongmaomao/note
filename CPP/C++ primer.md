- [变量和基本类型](#变量和基本类型)
  - [复合类型声明](#复合类型声明)
  - [结构体](#结构体)
- [字符串、向量、数组](#字符串向量数组)
- [表达式](#表达式)
  - [左值(lvalue)，右值(rvalue)](#左值lvalue右值rvalue)
  - [求值顺序](#求值顺序)
  - [赋值运算](#赋值运算)
  - [sizeof运算符](#sizeof运算符)
  - [类型转换](#类型转换)
    - [隐式转换](#隐式转换)
    - [显式转换](#显式转换)
- [语句](#语句)
  - [条件语句](#条件语句)
    - [if else](#if-else)
    - [switch case](#switch-case)
  - [跳转语句](#跳转语句)
  - [try语句块与异常处理](#try语句块与异常处理)
- [函数](#函数)
  - [基础结构](#基础结构)
    - [局部对象](#局部对象)
    - [分离式编译](#分离式编译)
  - [参数传递](#参数传递)
    - [const形参和实参](#const形参和实参)
    - [数组形参](#数组形参)
    - [可变形参](#可变形参)
  - [return](#return)
  - [函数重载](#函数重载)
  - [函数特殊特性](#函数特殊特性)
    - [默认实参](#默认实参)
    - [内联函数与constexpr函数](#内联函数与constexpr函数)
    - [调试帮助](#调试帮助)
  - [函数匹配](#函数匹配)
  - [函数指针](#函数指针)
- [类](#类)
  - [抽象数据类型](#抽象数据类型)
  - [访问控制与封装](#访问控制与封装)
  - [类的作用域](#类的作用域)
  - [构造函数](#构造函数)
    - [基础](#基础)
    - [构造函数初始值列表](#构造函数初始值列表)
    - [委托构造函数](#委托构造函数)
    - [隐式的类类型转换](#隐式的类类型转换)
    - [特殊类](#特殊类)
  - [类的静态成员](#类的静态成员)
- [IO库](#io库)
  - [IO类](#io类)
  - [文件流](#文件流)
  - [string流](#string流)
- [顺序容器](#顺序容器)
  - [容器库通用方法](#容器库通用方法)
  - [顺序容器操作](#顺序容器操作)
    - [添加元素](#添加元素)
    - [访问元素、删除元素](#访问元素删除元素)
    - [forward\_list、string](#forward_liststring)
    - [迭代器失效](#迭代器失效)
  - [vector内存管理](#vector内存管理)
  - [容器适配器](#容器适配器)
- [泛型算法](#泛型算法)
  - [基础算法](#基础算法)
  - [lambda](#lambda)
    - [lambda捕获和返回](#lambda捕获和返回)
    - [参数绑定](#参数绑定)
  - [特殊迭代器](#特殊迭代器)
  - [泛型算法结构](#泛型算法结构)
- [关联容器](#关联容器)
- [动态内存](#动态内存)
  - [智能指针](#智能指针)
    - [shared\_ptr](#shared_ptr)
    - [异常](#异常)
    - [unique\_ptr](#unique_ptr)
    - [weak\_ptr](#weak_ptr)
  - [动态数组](#动态数组)
    - [new动态数组](#new动态数组)
    - [allocator类](#allocator类)
  - [标准库实战 P430](#标准库实战-p430)
- [拷贝控制](#拷贝控制)
  - [拷贝赋值销毁](#拷贝赋值销毁)
    - [三/五法则](#三五法则)
    - [阻止拷贝](#阻止拷贝)
  - [拷贝控制和资源管理](#拷贝控制和资源管理)
    - [类值类](#类值类)
    - [类指针类](#类指针类)
  - [交换操作](#交换操作)
  - [动态内存管理类及示例 P460、P464](#动态内存管理类及示例-p460p464)
  - [对象移动](#对象移动)
    - [右值引用](#右值引用)
- [重载运算符与类型转换](#重载运算符与类型转换)
  - [重载运算符](#重载运算符)
  - [可调用对象](#可调用对象)
  - [重载、类型转换、运算符](#重载类型转换运算符)
- [OOP](#oop)
  - [基类派生类定义](#基类派生类定义)
  - [虚函数](#虚函数)
  - [访问控制](#访问控制)
  - [作用域](#作用域)
  - [构造函数与拷贝控制](#构造函数与拷贝控制)
  - [容器与继承](#容器与继承)
- [模板与泛型](#模板与泛型)
  - [CRTP](#crtp)
- [拓展资料](#拓展资料)
  - [变量区分类与默认初始化](#变量区分类与默认初始化)
  - [直接初始化与拷贝初始化](#直接初始化与拷贝初始化)


# 变量和基本类型
## 复合类型声明
引用不是对象，指针是对象
```C++
int *p;
int *&r = p;
```
```C++
int i = 0;
const int ci = 0; //常量表达式在编译时会用字面值替代对应常量
constexpr int cei = 0; //声明 或 初始化
int *const p0 = &i;
const int *p1 = &ci;
const int *const p2 = p1;
//int *const p3 = p1;
p1 = p2;
//int &r0 = ci;
const int &r = i;
const int &r1 = 3.14; //const int temp = 3.14;const int &r1 = temp;
                      //r1被绑定到了一个临时量上，正确但是要注意这个临时对象的生命周期
constexpr const int *p4 = &cei;  //constexpr 只能赋值为 nullptr 或 0 或 指向固定地址对象
```
```C++
typedef char *pstring;
const pstring cstr = 0;
const pstring *ps;  //char *const *ps;
```
auto一般忽略顶层const，保留底层const
```C++
int i = 0;
const int ci = i, &cr = ci;
auto b = ci;
auto c = cr;
auto d = &i;
auto e = &ci;
const auto f = ci;
auto &g = ci;
//auto &h = 42;
const auto &j = 42;
```
## 结构体
```C++
struct item {int id = 0;};
item it, *ip;
```
# 字符串、向量、数组
```c++
string::size_type, std::size_t //size_t在cstddef（stddef.h）中  
//vector访问用size_type, 数组用size_t
vector<string>::difference_type, std::ptrdiff_t  
vector<string>::iterator, vector<string>::const_iterator
```
```c++
vector<string> text{"123","456","789"};
auto beg = text.begin(), end = text.end();
auto cbeg = text.cbegin(), cend = text.cend();
auto mid = beg + (end - beg) / 2;
```
```c++
int *ptrs[10];
//int &ptrs[10]; 引用不是对象
int (*Parrary)[10] = &arr;
int (&arrRef)[10] = arr;
int *(&arry)[10] = ptrs;
```
```c++
int nums[] = {1,2,3};
auto p = nums;  //auto p = &nums[0];
decltype(nums) q = {1,2,3}; //长度为3得int数组
int* e = &nums[3];  //数组指针与vector迭代器有相同方法，这里获取尾后指针
for (int *b = nums; b != e; ++b)    cout << *b << endl;
int k = e[-2];  //int k = *(e - 2);
                //注意：vector下标运算符为size_type无符号数
/*C++ 11*/
int *beg = begin(nums), *end = end(nums);
```
```c++
/*旧代码接口*/
string s("hello world!");
const char* str = s.c_str();
s += str;   //可以作为复合运算右侧对象

int int_arr[] = {0,1,2,3,4,5};
vector<int> ivec(begin(int_arr), end(int_arr));
vector<int> subVec(int_arr + 1, int_arr + 3);
```
```c++
int ia[3][4] = {0,1,2,3,4};
int (&row)[4] = ia[1];
int (*p)[4] = ia;
int *p[4] = {ia[0]};
```
# 表达式
## 左值(lvalue)，右值(rvalue)
左值使用的是对象的身份(在内存中的位置)，右值使用的是对象的值(内容)  
decltype处理左值得到引用类型：
```c++
int *p;
decltype(*p) --> int&
decltype(&p) --> int**
//++i求值结果是对象i运算后本身作为左值返回
//i++求值结果是对象i原始值的副本作为右值返回
```
## 求值顺序
```C++
/* 错误示范 */
int i = f1() * f2(); //无法确定求值顺序
int j = 0;
cout << j << " " << ++j << endl;  //未定义，无法确定j和++j求值顺序
*p = toupper(*p++); //未定义
/* 例外 */
*++iter //子表达式虽然改变了变量值但是作为了另一个子表达式的变量
&& || ?: ,  //4个运算符确定了求值顺序
            //&&和||短路求值，左侧表达式无法确定结果时才对右侧求值
```
## 赋值运算
1. 赋值运算左侧运算对象必须为可修改的左值
2. 赋值运算满足右结合律
```c++
//C++11可以用初始值列表作为赋值右侧运算对象
//int k = {3.14};窄化转换错误
int i, j;
i = j = 0;
```
## sizeof运算符
右结合律，所得值为size_t类型的常量表达式
```C++
int num[5];
int *p = end(num);
size_t a = sizeof *p; //优先级相同且都是右结合律，即使p为空指针也合法(不会对*p求值)
auto b = sizeof num;
constexpr size_t sz = sizeof(num) / sizeof(*num);
int arr[sz];  //常量表达式可以声明数组维度
```
## 类型转换
### 隐式转换
1. 非常量指针/引用可以转换为常量类型的(添加底层const，但是不能删除底层const)
2. 不允许同时执行多次类型转换如：item it("9999-99-9");先将字面值转换成string类型，再把string转换成item
### 显式转换
*cast-name<type>(expression)*
```C++
//static_cast dynamic_cast const_cast reinterpret_cast
//static_cast:只要不包含底层const
void *p = &d; //任何非常量对象都可存入void*
double *dp = static_cast<double*>(p); //找回存于void*的指针
//const_cast:只改变底层const属性
const char *cp1;
char *p1 = const_cast<char*>(cp1); //但是任何cp1的写操作都会引起未定义的结果
static_cast<string>(cp1); //正确，字符串字面值转换为string，但是下列为错
                          //const_cast<string>(cp1);
//旧的类型转换type(expr)和(type)expr和reinterpret_cast类似
```

# 语句

## 条件语句

### if else
else一般与最近的未匹配的if匹配

### switch case
1. case标签后仅能跟整型常量表达式
2. 某个case匹配后会执行其后(包括其他case后)的所有语句。此时要注意初始化变量的作用域
3. default标签
```c++
int ival = 42;
while(cin >> ch){
  switch (ch) {
    //case ival:ival不是整型常量表达式
    case 'a':
      //int a = 0;如果这样初始化声明a，其作用域包括case a及后面所有case，但初始
      //化可能被跳过
      {
        int a = 0;
      }
    case 'e':
        break;
    default:
      break;
  }
}
```
## 跳转语句
break，continue，goto，return
1. break作用于最近的while, for, do while, switch
2. continue作用于最近的while, for, do while

## try语句块与异常处理

1. throw
```C++
if (item1.isbn() != item2.isbn())
  throw runtime_error("Data must refer to same ISBN");
  //throw一个异常将终止当前函数并把控制权交给能处理该异常的代码
cout << item1 + item2 << endl;
```
2. try
```c++
try {
  program-statements
} catch (exception-declaration) {
  handler-statements
} // ···
```
3. 标准异常 P176

# 函数

## 基础结构
1. 形参、实参
2. 函数返回值不能是数组或函数，但可以是指向他们的指针

### 局部对象
1. 名字具有作用域、对象具有生命周期
2. 自动对象：没有显式初始值，执行默认初始化(未定义行为)
3. 局部静态变量：在第一次经过对象定义语句时初始化，若没有显式初始值，执行值初始化
```c++
size_t count_calls(){
  //size_t i;
  static size_t j;
  static size_t k = 0;  //两条语句等价
  return ++k;
}
```
### 分离式编译
编译和链接多个源文件：如Main.cc使用了定义在Fact.cc中的fact函数，而fact函数在头文件Fact.h中声明了  
Fact.h首先要包含在Fact.cc中，然后分别编译生成.obj(.o)文件，即包含对象代码的文件(object code)  
然后编译器把对象文件链接在一起形成可执行文件 P186

## 参数传递
引用传递(引用)、值传递(指针等)

### const形参和实参
1. 初始化操作一般忽略顶层const(作用于对象本身的const)
```c++
void fcn(const int i) { /* */ }
void fcn(int i) { /* */ } //错误：重复定义fcn
//C++可以定义多个有明显区别形参列表的同名函数
//但是由于会忽略顶层const，所以传入上述两个函数的实参可以完全一样，所以两个形参列表实际上没啥区别
```
2. 初始化操作一般不能删除底层const(作用于指针指向对象的const)
```c++
void reset(int *i) { /* */ };
void reset(int &i) { /* */ };
int i = 42;
const int ci = i;
string::size_type ctr = 0;
reset(&i); 
//reset(&ci);
reset(i);
//reset(ci); reset(42);
//reset(ctr);
```
3. 尽量使用常量引用，普通引用不能接受const对象、字面值或需要类型转换的对象
```c++
int count(const string &s){ return s.length(); }
//int count(string &s){ return s.length(); }无法接受需要转换为string类型的const char*类型或string字面值
count("Hello world!");
```

### 数组形参
1. 数组不允许拷贝(值传递)、数组一般被转换成指针使用
```c++
//等价：void print(const int*), void print(const int[]), void print(const int[10]);
//表示长度限制需要：
void print(const char *cp)  //'\0'结束标识符
void print(const int *beg, const int *end)
void print(const int *beg, size_t sz)
```
2. 数组引用形参,数组可以引用
```c++
void print(const int (&arr)[10])  //维度是类型的一部分，表示引用的是长度为10的数组类型
```
3. 多维数组，实际是传递数组的数组
```c++
void print(const int (*matrix)[10], int rowSize); //第二个10是指针指向的类型定义一部分，不能省
void print(const int matrix[][10], int rowSize);  //第二个10不能省
```
4. main，处理命令行选项 P196
```c++
int main(int argc, char *argv[]) { ... };
```
### 可变形参
省略符、C++11：initializer_list、可变参数模板 P618
1. initializer_list标准库类型
```c++
//initializer_list<T> lst;
void error_msg(ErrCode e, initializer_list<string> il){
  for(auto beg = il.begin(); beg != il.end(); ++beg)  
    cout << *beg << " ";
  cout << endl;
}
```

2. 省略符形参，用于C++访问使用了名为varargs得C标准库功能的C代码
```c++
void foo(parm_list, ...)
void foo(...)
```

## return
1. 返回值为引用则返回左值，返回引用要注意引用对象的生命周期(函数内定义的字面值转换成的临时量对象会被销毁)
2. C++11可以返回列表初始化返回的临时量，但是返回内置类型时最多含有一个值且所占空间不能大于目标类型空间
3. main可以在结束时隐式的插入return 0;表示成功，另外有两个*预处理量*(不在std空间)EXIT_FAILURE和EXIT_SUCCESS可以作为main函数返回值。
4. 数组不能被拷贝所以不能作为返回值，只能返回其指针或引用。类型别名可以简化语句
```c++
typedef int arrT[10]; //等价using arrT = int[10];
arrT* func(int i);
int arr[10] = {1,2,3};
decltype(arr) *func(int i); //decltype
int (*func(int i))[10]; //func(int i)表示func是形参列表为int i的函数
                        //*func(parameter_list)表示函数func的值可以进行解引用操作
                        //(*func(parameter_list))[10]表示函数func的值解引用后会得到长度为10的数组
                        //int (*func(parameter_list))[dimension]表示函数func的值解引用后数组存储的对象是int
auto func(int i) -> int(*)[10]; //C++11尾置返回类型
```

## 函数重载
1. 同一作用域名字相同但形参列表不同的函数为重载函数  
   形参列表不同：顶层const不影响，形参名称有没有不影响，typedef不影响
```c++
const string &shorter(const string &s1, const string &s2){
  return s1.size() < s2.size() ? s1 : s2;
}
//为了使其对非常量的输入返回非常量的引用，使用const_cast:
string &shorter(string &s1, string &s2){
  auto &r = shorter(const_cast<const string&>(s1), const_cast<const string&>(s2));
  return const_cast<string&>(r);
}
//对shorter函数重载，非常量引用优先匹配非常量版本，而最后对r的显示转换作用于原本非常量的对象上，是安全的
```
2. 作用域：编译器一旦在当前作用域找到了所需名字，就会忽略外层作用域的同名实体  
   一般也不建议在局部作用域内声明函数

## 函数特殊特性
默认实参、内联函数、constexpr函数
### 默认实参
1. 一旦某个形参被赋予了默认值，它后面的形参都必须有默认值，因为函数调用实参按其位置解析，默认实参只负责填补缺少的尾部实参，所以第一个需要覆盖的默认值前的形参都需手动覆盖
```c++
string screen(int ht = 24, int wid = 80, char backgrnd = ' ');
string window = screen('?');  //正确，但等价于string window = screen('?', 80, ' ');
```
2. 实参默认值只能声明一次而函数可以多次声明。通常把默认实参放在函数声明中，并置于合适头文件
```c++
string screen(int, int, char = ' ');
//string screen(int, int, char = '*');  重复声明
string screen(int = 24, int = 80, char);  //正确添加默认实参
```
3. 局部变量不能作为默认实参(默认实参是一个初始化器，而局部变量不能作为初始化器 [变量区分类与默认初始化](#变量区分类与默认初始化) )，只要表达式的类型能转换成形参所需的类型，该表达式就能作为默认实参  
   用作默认实参的表达式在函数声明所在作用域内解析，但求值发生在函数调用时
```c++
int b = 0, c = 0;
int a();
void func(int = a(), int = b, int = c);
void f2(){
  b = 1;  //改变实参默认值
  int c = 2;  //隐藏了外层作用域的c，无作用
  func(); //func(a(), 1, 0);
}
```
### 内联函数与constexpr函数
1. 内联函数  
   一次函数调用需要：保存寄存器，并在返回时恢复；可能要拷贝实参；程序重定位  
   内联函数(inline type function_name(parameter_list);)向编译器发出内联展开函数的请求，编译器会对规模较小、流程直接、频繁调用的函数进行优化。而对递归函数或规模较大的函数的内联请求会忽略。

2. constexpr函数：能用于常量表达式，返回类型和形参都必须是字面值类型且函数体有且仅有一条return语句。  
   被隐式指定为内联函数  
   但是也能用一个非常量表达式调用constexpr函数，也有可能返回一个非常量表达式。当把他用在需要常量表达式的上下文，编译器会负责检查其结果是否符合要求。
   内联函数和constexpr函数一般定义在头文件(保证多次定义相同)
  
### 调试帮助
预处理功能：assert和NDEBUG。assert(expr)是预处理宏，NDEBUG是对应的预处理变量，都由预处理器管理，名字需要唯一。P215
   
## 函数匹配
1. 函数名相符，声明在调用点可见 ---> 候选函数  
2. 形参实参数目相符(注意默认实参)，类型相同或可转换 ---> 可行函数  
3. 最佳匹配(可能产生二义性调用) P219

## 函数指针
1. 函数指针指向函数而非对象，指向某种特定类型。
```c++
int func(int i);
int func(float x);
int (*func1)(int i) = func;  //func1是个函数指针,func 和 &func 等价
//int (*func3)(double y) = func;错误，重载函数必须精准匹配
int *func2(int i);  //func2是个函数，返回一个数组指针
```
2. 函数指针形参
```c++
//下述等价，自动转换为函数指针
void func(int i, int pf(const string &));
void func(int i, int (*pf)(const string &));
func(i, pf);
```
```c++
//类型别名简化语句
typedef int Func(const string &);
typedef decltype(pf) Func2;

typedef int (*FuncP)(const string &);
typedef decltype(pf) *FuncP2; //decltype的值是函数类别，需要手动加*转化为函数指针

void func(int i, Func); //等价void func(int i, FuncP);
```
3. 返回函数指针：需要手动写作指针形式，编译器不会将函数制动转换为函数指针
```c++
FuncP f1(int); //返回函数指针
//Func f2(const string &); 错误，无法返回函数
Func *f3(int);
int (*f4(int))(const string &); //f4函数返回指针，指向形参为const string &的函数
auto f5(int) -> int (*)(const string &); //尾指返回类型
```
# 类
类：数据抽象(接口、实现分离) + 封装
## 抽象数据类型
1. 成员函数必须声明在类内部，定义位置不限;作为接口的非成员函数可以声明和定义在类外部  
   定义在类内部的函数是隐式的inline函数，在外部显式定义的inline成员函数应该和类在同一个头文件
```c++
struct Sales_data {
  std::string &isbn() const {return bookNo;}
  Sales_data &combine(const Sales_data &);
  std::string bookNo;
  unsigned uints_sold = 0;
  double revenue = 0.0;
}
Sales_data add(const Sales_data &, const Sales_data &);
std::istream read(std::istream &, const Sales_data &); //IO类不可拷贝，所以只能通过引用传递
std::ostream print(std::ostram &, Sales_data &);  //IO操作会改变流，所以不能用常量引用
```
2. this：  
   成员函数声明时有额外的隐式参数this，在调用时this默认初始化为指向请求该成员函数对象的指针，且不可更改this的值，即this是Sales_data *const类型的对象。  
   在声明时，给this添加底层const(否则无法被常量对象及其引用和指针调用)：
```c++
std::string &isbn() const {return bookNo;}
//用形参列表后的const修饰this，使得isbn()可以处理常量类型类型的Sales_data,被称为常量成员函数
//return bookNo; 理解为return this->bookNo;
```
3. 作用域：在类外定义成员函数时用作用域运算符::表面函数被声明在哪个作用域
4. this作为返回值，注意const成员函数返回的将是一个常量引用
```c++
//Sales_data::combine模仿内置运算符+=，对左侧表达式赋值并返回其左值，这里用this和引用实现
Sales_data &Sales_data::combine(const Sales_data &rhs){
  units_sold += rhs.units_sold;
  revenue += rhs.revenue;
  return *this;
}
//为满足同时对常量和非常量对象的接口进行函数重载，也可以类似 ## 函数重载 中使用const_cast转换
class Sales_data {
public:
  Sales_data &print(std::ostream &os) { do_print(os); return *this;}
  const Sales_data &print(std::ostream &os) const { do_print(os); return *this;}
  ...
private:
  void do_print(std::ostream &os) const { os << bookNo;}
  ...
}
```
5. 拷贝、赋值和销毁在未自定义时会由编译器自动合成，但是对于管理动态内存的类来说会失效，对于这种情况一般用vector或string对象作为成员来避免管理动态内存带来的复杂性，并使编译器合成的拷贝赋值销毁操作合法
6. 类型成员必须先定义，后使用(与其他成员不同)
7. 可变数据成员：永远不会是const，即使在const对象中->所以一个const成员函数可以改变一个mutable的可变成员
8. 类类型、不完全类型(前向声明、可以定义指向其得指针或引用，也可以声明(但不能定义)以其为返回值类型得函数)


## 访问控制与封装
1. public、private访问说明符  
   class、struct除了默认访问权限不同外等价
3. 优点：确保用户无意破坏封装对象状态、被封装的成员实现可以随时独立改变、当更新代码或查找错误时缩小检查范围
2. 友元friend：为了使非成员函数访问private成员，在类内添加含有friend关键字的函数声明即可  
   友元声明只是表明了访问权限，并非真正的函数声明，用户使用前需要额外声明  
   友元类的成员函数可以访问该类的private成员变量。  
   友元关系不存在传递关系
4. 注意友元声明和定义的顺序，在定义一个友元函数前必须先定义它访问的类并在其中中声明它为友元，且成员函数必须在声明后才能被其他类声明为友元(非成员函数和类不用)
5. 使用一个友元前它必须在当前作用域可见(即在当前作用域声明过)，友元声明只是表明了访问权限，并非真正的函数声明

## 类的作用域
1. 成员函数的外部定义时，遇到类名时后面的表达式就属于类作用域之内了
```c++
class Sales_item {
public:  
  using sz = std::string::size_type;
  sz length(std::string &) const;
private:
  std::string bookNo = " ";
  ...
}
Sales_item::sz Sales_item::length(std::string &) const {
  return bookNo.length();
}
```
2. 名字查找：对于定义在类内的成员函数，首先编译器编译所有成员的声明直到全部可见后才编译函数体(一般是前向查找，若没有继续前向查找外层作用域)  
  类成员声明中使用的名字则只能前向查找，包括返回类型和参数列表，如果未查找到，就会继续在定义该类的外层作用域继续查找。
  特别的，对于类型名，类中如果已经使用过外层定义的某类姓名(typedef double Money;)，则不能重新定义
3. 成员函数的参数名会隐藏同名的成员，因为参数列表在函数定义的作用域内，若函数定义的作用域内无该名字才会在类中查找
4. 表示全局变量如 "return width * ::height;"



## 构造函数
   
### 基础
1. 构造函数可以有多个(类似重载函数，参数列表要有区别)  
   不能被声明为const(在构造函数完成其初始化后才能赋予对象常量属性)  
2. 默认构造函数：  
   若存在类内初始值，则用其初始化成员，否则默认初始化该成员(块中的内置类型或复合类型值会是未定义的)  
   只有当未显式声明构造函数时编译器才自动生成合成的默认构造函数
   类中包含其他无默认构造函数的类的成员，则无法自动生成合成的默认构造函数
   使用默认构造函数：Sales_item obj;而非Sales_item obj();
3. **类内初始值必须用=或者{}的形式初始化。**
```c++
struct Sales_data {
  Sales_data() = default; //不接受实参，默认构造函数，default表示使用编译器生成的合成的默认构造函数
  Sales_data(const std::string &s, unsigned n, double p) : 
             bookNo(s), units_sold(n), revenue(n*p) { }
             //构造函数初始值列表,函数体中内容可选
  Sales_data(std::istream &); //声明构造函数，外部定义
  ...
}
Sales_data::Sales_data(std::istream &is){ //构造函数没有返回类型
  read(is, *this);  //this把对象作为整体访问
}
```
### 构造函数初始值列表
1. 构造函数体一开始执行，初始化就已经完成，如果成员是const，引用或未提供默认构造函数的类类型，则必须初始化
```c++
class ConstRef {
public:
  ConstRef(int i);
private:
  int n;
  const int c;
  int &r; //和int n交换位   置的话下列构造函数会出现未定义的错误
}
//构造函数初始值列表不决定初始顺序，而由成员在类中的声明顺序决定
ConstRef::ConstRef(int i) : n(i), c(i), r(n) { }
//下为错误写法，c无法被赋值，r未初始化
ConstRef::ConstRef(int i) { n=i; c=i; r=n;}
```
2. 若一个构造函数为其所有参数都提供了默认实参，则实际上提供了一个默认构造函数

### 委托构造函数

```c++
class Sales_item {
public:
  Sales_item(std::string s, unsigned cnt, double price) :
            bookNo(s), units_sold(cnt), revenue(cnt*price) { }
  Sales_item() : Sales_item("", 0, 0) { }
  Sales_item(std::string s) : Sales_item(s, 0, 0) { }
  Sales_item(std::istream &is) : Sales_item() { read(is, *this); }
  ...
}
```
### 隐式的类类型转换
1. 只含有一个实参的构造函数隐式地定义了从实参类型到类类型地转换规则，但是由于只允许一步转换所以在输入字面值等需要先转换为实参类型地表达式无效
```c++
Sales_item::Sales_item(const std::string &);
Sales_item::Sales_item(std::istream &is);
std::string null_book = "9-999-99999-9";
item.combine(null_book);  //null_book会先隐式转换为Sales_item类的临时值，而由于combine是参数常量
                          //引用，所以可以给他传递一个临时对象
//item.combine("9-999-999999-9");错误
item.combine(string("9-999-999999-9"));
item.combine(Sales_item("9-999-999999-9"));
item.combine(cin);
```
2. 抑制隐式转换:explicit关键字，只用于只含一个参数的构造函数，只用于类内构造函数声明时  
   explicit构造函数只能用于直接初始化(不能用于=代表的拷贝初始化)
   但是可以使用explicit构造函数显式转换
```c++
item.combine(Sales_item(null_book));
item.combine(static_cast<Sales_item>(cin)); //static_cast可以使用
```

### 特殊类
1. 聚合类：所有成员public, 没有定义构造函数, 没有任何类内初始值, 没有基类和virtual函数  
   用户需要使用花括号括起来的成员初始值列表按顺序显式初始化类内成员(与数组一样，不足则值初始化，不能过多)
2. 字面值常量类 P267  
   constexpr构造函数(唯一可执行语句是return；不能return)

## 类的静态成员
1. 类的静态成员与类关联而非与类的每个对象分别关联  
   类对象中不包含任何与静态数据成员有关数据。  
   静态成员函数也不与任何对象绑定，不包含this指针，所以不能被声明为const的，不能通过this隐式或显式地调用非静态成员
2. 使用作用域运算符直接访问静态成员；或用类的对象、引用、指针访问  
   成员函数不用通过作用域就能使用静态成员(隐式的this指针调用)
3. 静态成员函数可以在类外定义，但是static关键字只能出现在类内函数声明时
4. 静态成员不在创建类对象时被定义，不是由类构造函数初始化，且一般不能在类内部初始化静态成员。类似于全局变量，其定义在任何函数之外，且只能定义一次。
5. 静态成员类内初始化：静态成员是constexpr的字面常量型时可以用const整数型在类内初始化，初始值必须是常量表达式
6. 静态成员可以是不完全类型，可以是类本身类型的对象(而普通成员只能是其引用或指针)，可以作为默认实参

# IO库

## IO类
1. IO类对象不能拷贝或赋值，一般以引用方式传递，读写IO对象会改变其状态，所以传递和返回的引用不能是const的
2. 条件状态(strm::iostate机器相关类型):badbit, failbit, eofbit, goodbit是iostate类型的constexpr值  
   使用流作为条件等价于!fail()
```c++
//复位failbit和badbit
cin.clear(cin.rdstate() & ~cin.failbit & ~cin.badbit);  //条件状态被看作一个位集合，用位运算操作
auto old_state = cin.rdstate();
...
cin.setstate(old_state);
```
3. 输出缓冲区管理，缓冲区允许合成多个输出并只进行一次系统级写操作，提升性能  
   显式刷新缓冲区：endl,flush,ends  
   操纵符unitbuf,nounitbuf设置流内部状态对每次输出设置刷新策略 cout << unitbuf;
   程序异常终止时输出缓冲区不刷新(操作已执行但未打印)
   关联输入输出流 strm::tie

## 文件流
1. 通过构造一个文件流对象关联一个文件来进行读写操作
2. fstream继承自iostream，在使用基类对象的地方可以用继承类对象代替，即形参为iostram &的函数可以由fstream实参调用
```c++
for (auto p = argv + 1; p != argv + argc; ++p) {
  ifstream input(*p);
  if (input)
    process(input);
  else
    cerr << "couldn't open : " + string(*p);  //cerr会自动刷新缓冲区
}//每步循环结束后input会被销毁，close被自动调用
```
3. 文件模式:in,out,app,ate,trunc,binary  
   out模式默认截断文件，否则要加app模式或者in模式，模式组合也用位运算符

## string流


# 顺序容器

vector, deque, list, forward_list, array, string  
区别：随机访问/顺序访问，插入/删除元素，双端/单端
1. vector、string保存在连续内存空间：增删中间元素；扩大存储空间
2. list、forward_list不支持随机访问，额外内存开销大

## 容器库通用方法

1. 迭代器范围[左闭右开)，可以通过反复递增从begin到达end，且可对begin进行解引用操作
2. begin和end成员，如果不带c则是一个重载函数，根据容器中的类型返回iterator或const_iterator  
   所以要注意若容器内元素是const类型，使用vector<type>::iterator iter = begin();会试图删除底层const，所以一般使用C++11新标准的auto关键字
3. 容器初始化：[直接初始化与拷贝初始化](##直接初始化与拷贝初始化)  
   使用迭代器初始化只要保证拷贝的元素可转换就行，拷贝初始化要保证容器类型和元素类型都相同
4. swap和赋值，数组array不能使用一个花括号列表赋值  
   assign等赋值操作会导致指向左边容器内部的迭代器、引用和指针失效，所以assign操作符参数不能指向调用它的对象中的元素。
   swap不会对任何元素进行拷贝、删除或插入，可以在常数时间内完成 -> 意味着之前的迭代器、引用和指针都不会失效，且指向的元素也不会发生改变，但是所属的容器对象改变了(string、array除外)
```c++
list<string> names;
vector<const char *> oldstyle;
names.assign(oldstyle.cbegin(), oldstyle.cend());
```
## 顺序容器操作

### 添加元素
1. 向一个vector、string或deque插入元素会使指向容器的迭代器、引用和指针失效，有可能需要重新分配所有对象的存储空间(保证内存上顺序存储)，在尾部外添加元素会移动元素。
2. 和assign一样，insert范围插入时表示范围的迭代器不能是指向调用对象元素的迭代器。  
   insert返回值是第一个插入的元素(没有也是插入点后的第一个元素)
3. C++11引入emplace成员会直接在容器对应位置构造元素，即在对应内存位置直接初始化一个元素
```c++
item.emplace_back("978-0590353403", 25, 15.99);
item.push_back(Sales_item("978-0590353403", 25, 15.99));
//item.push_back("978-0590353403", 25, 15.99);没有接受三个参数版本的push_back成员
auto next = item.insert(iter, 10, Sales_item());
```
```c++
c.push_back(t), c.emplace_back(args); //forward_list不支持
c.push_front(t), c.emplace_front(args); //vector, string不支持
c.insert(p, t), c.emplace(p, args), c.insert(p, n, t), c.insert(p, b, e), c.insert(p, il);
```
### 访问元素、删除元素
1. 访问元素的成员函数返回的是引用(front,back,at和下标)，如果是const对象，则返回const引用
2. 删除deque中除首尾元素之外的任何元素会使所有迭代器、指针和引用失效；  
   删除vector、string中元素会使删除点后的所有迭代器、指针和引用失效；
```c++
c.back(), c.front(), c[n], c.at(n);
c.pop_back(), c.pop_front(), c.clear();
c.erase(p), c.erase(b, e);  //返回删除元素后的第一个元素迭代器或尾后迭代器
```
### forward_list、string
1. elem1 -> elem2 -> elem3 -> elem4;  
   elem1 -> elem2 ----------> elem4;  
   增删elem3其实修改的使elem2的后继，所以需要访问前驱，但单向链表不能直接访问前驱
2. 首前迭代器lst.before_begin();提供在链头增删元素的方法
```c++
lst.insert_after(p, n, t), ... ;
lst.emplace_after(p, args);
lst.erase_after(p), lst.erase_after(b, e);  //b保留
```
3. s.append(), s.assign(), s.replace();
4. string搜索函数返回std::string::size_type，无符号整型，并定义常量值npos = -1(即最大值)。
```c++
s.find(args), s.rfind(args);
s.find_first_of(args), s.find_last_of(args);
s.find_first_not_of(args), s.find_last_not_of(args);
```
5. 类型转换
```c++
to_string(val);
stoi(s,p,b), stol(s,p,b), stoul(s,p,b), ... ; //p : size_type*(0), 不为0则保存第一个非数值位
stof(s,p), stod(s,p), stold(s,p);
```

### 迭代器失效

## vector内存管理
```c++
c.shrink_to_fit();  //请求可能被忽略 C++11
c.capacity();
c.reserve(n), c.resize(n), c.resize(n, t);
```
1. 当需求大小小于容量空间时，不会退回内存空间，所以c.reserve(n)后分配的容量可能大于等于n。
2. 只有当需求大于容量空间时才会分配新的内存空间。

## 容器适配器
stack, queue, priority_queue
1. 默认：stack和queue基于deque实现，priority_queue基于vector实现
```c++
s.pop(t), s.push(t), s.emplace(args), s.top();
q.pop(t), q.push(t), q.emplace(args), q.front(), q.back(), q.top(); 
```

# 泛型算法
1. 迭代器令算法不依赖于容器，但依赖于元素类型的操作；算法永远不会执行容器的操作
## 基础算法
1. 只读算法：find(b,e,val), accumulate(b,e,val);注意accumulate的val指定了执行加法的元素类型， string("") 有+而 "" 没有+运算符
2. 只接受单一迭代器表示第二序列的算法假定来给你个序列长度相等，如equal
3. 写容器算法，对容器内元素赋值，不会执行容器操作fill, fill_n, copy, repalce, repalce_copy
4. 插入迭代器back_inerter:对插入迭代器解引用后赋值会执行push_back操作
```c++
vector<int> vec;
fill_n(back_inserter(vec), 10, 0);  //vec.insert(vec.begin(), 10, 0);
//fill_n(vec.begin(), 10, 0)错误
auto it = back_inserter(vec);
*it = 42;
```
5. 重排元素：sort, unique(双指针实现) [vector版本例子 vec_unique()](test.cpp)

## lambda
1. 谓词(可调用的表达式，返回结果可做条件)：一元谓词、二元谓词；sort, find_if, partition
2. lambda表达式:一个可调用的代码单元,是一个可调用对象(函数及其指针、lambda、重载符号类)
```c++
[capture list](parameter list) -> return type {function body}
//capture list:lambda所在函数中定义的局部变量列表
//lambda表达式必须用尾指返回类型
//参数列表和返回类可以忽略，如 auto f = [] {return 42;};，但若包含单一return外语句默认返回void
//lambda表达式不能有默认参数
sort_stable(words.begin(), words.end(), isShorter); //根据二元谓词isShorter排序
auto wc = find_if(words.begin(), words.end(), [sz](const string &a) -> bool{
  return a.size() >= sz;
}); //根据局部变量sz返回第一个长为sz的string迭代器, bool可省
for_each(wc, words.end(), [](const string &s){cout << s << " ";});//每个词后跟一个空格输出
cout << endl;
```
3. lambda定义时由操作系统生成一个对应的类类型，当使用lambda时创建一个对应对象

### lambda捕获和返回

1. lambda生成的类包含其所捕获的变量对应数据成员，创建对象时被默认初始化
2. 值拷贝：在定义lambda时将对应变量对应对象拷贝
3. 引用拷贝：确保使用lambda时引用的对象还存在，而引用的局部变量会在函数后销毁  
   对不能拷贝的对象如iostream, 函数, 数组等使用引用捕获  
   lambda作为函数返回值时，不能包含引用捕获，因为引用的局部变量会在函数返回后销毁
4. 隐式捕获：编译器推断。[&]表示引用捕获，[=]表示值捕获，都可以与显式捕获混用如[=, &os], [&, c]
5. 可变lambda：值捕获加上mutable关键字,只是由lambda创建的对象中数据成员值可变，并不影响该对象外的任何值，参考[mutable_lambda()](test.cpp),与引用捕获差异很大。

### 参数绑定

1. bind函数(拷贝参数对象)
```c++
auto newCallable = bind(callable, arg_list);
//arg_list为用逗号隔开的对应于callable的参数列表，含有std::placeholders命名空间中的占位符_n
auto new_check_size = bind(check_size, std::palceholders::_1, 6);
//new_check_size(s) => check_size(s, 6);
//使用using std::placeholders::_1;说明_1的命名空间或using namespace std::placeholders;
using namespace std::placeholders;
auto g = bind(f, a, b, _2, c, _1);
//重排参数顺序
```
2. 绑定引用参数：ref(), cref()
```c++
ostream &print(ostream &os, const string &s, char c){
  return os << s << c;
}
for_each(words.begin(), words.end(), bind(print, ref(os), _1, ' '));  
//bind返回一个可调用对象作为一元谓词
```

## 特殊迭代器
迭代器、插入迭代器(inserter)、流迭代器(iostream_iterator)、反向迭代器(reverse_iterator)、移动迭代器

1. 插入迭代器:迭代器适配器, 返回的插入迭代器经过算数运算不会改变指向的位置  
   back_inserter(c), front_inserter(c), inserter(c, p);  
   注意区别front_inserter(c), inserter(c, c.begin());
2. 流迭代器(可以对任何定义了输入>>或输出<<运算符的类型使用)  
   不支持递减操作
```c++
ifstream in("filename");
istream_iterator<string> str_it(in);  //文件流继承自IO
istream_iterator<int> in_iter(cin), eof;  //默认初始化为尾后迭代器
while(in_iter != eof) vec.push_back(*in_iter++);
//等价于
vector<int> vec(in_iter, eof);

ostream_iterator<int> out_iter(cout, " ");  //不允许表示尾后迭代器，参数必须是const char*
copy(vec.begin(), vec.end(), out_iter); cout << endl; //out_iter = val;表示向cout赋值
//*out_iter, ++out_iter对其无影响，返回out_iter
```
3. 反向迭代器 iterator = reverse_iterator.base()成员函数

## 泛型算法结构
1. 5类迭代器：输入迭代器、输出迭代器、前向迭代器、双向迭代器、随机访问迭代器  
   输入迭代器：(==, !=, ++, *, ->)，只用于顺序访问，单遍扫描（假定了空间够）  
   输出迭代器：(++, *)，只能写一次，单遍扫描
   前向迭代器：多次读写同一元素，多遍扫描，但只能单向扫描  
   双向迭代器：(--)
   随机访问迭代器：(<, +, -, -, [])，常量时间内实现随机访问，-有两种含义，位移运算和计算距离
2. 参数规范  
   alg(beg, end, dest, other args)  //dest目标类型对象，或容器(插入)迭代器  
   alg(beg, end, beg2, end2, other args)
3. 命名规范：使用重载实现接受谓词的算法
4. _if版本, _copy版本, _copy_if版本
```c++
remove(vec.begin(), vec.end(), val);
remove_if(vec.begin(), vec.end(), pred);
remove_copy_if(vec.begin(), vec.end(), back_inserter(new_vec), pred);
```
5. list和forward_list分别提供双向和前向迭代器，所以有其独有的成员函数版本算法  
   链表可以直接改变元素间链接而不用真的拷贝赋值交换位置，所以其特有版本效率更高  
   lst.merge(lst2, comp); list.remove(val); lst.reverse(); lst.sort(comp); lst.unique(pred);  
   lst.splice(args), lst.splice_after(args);  
   链表特有算法会改变容器，而通用版本不会。

# 关联容器
1. 关联容器有其multi版本和unordered版本,除了无序容器，其成员函数版find都比泛型算法快
2. map<T, V>::value_type 为 pair<const T, V>;  
   set迭代器iterator和const_iterator都是只读类型
3. map下标操作(若没有关键字对应元素)会先创建一个值初始化的对象，并作为左值返回  
   map下标与解引用操作返回类型不同，mapped_type、value_type
4. 无序容器通过哈希函数实现桶存储，创建一个无序容器需要(隐式)提供一个关键字类型的==运算符和hash<key_type>类型 P395相关桶操作
```c++
c.insert(p,v), c.emplace(p,args), c.insert(b,e), c.insert(il); //p可选
c.erase(k), c.erase(p), c.erase(b,e); //注意返回值，c.erase(k)返回删除个数size_type
c.find(k), c.count(k);
c.lowwer_bound(k), c.upper_bound(k), c.equal_bound(k);
size_t hasher(const Sales_item &sd){
  return hash<string>()(sd.isbn());
}
bool eqOp(const Sales_item &lhs, const Sales_item &rhs){
  return lhs.isbn() == rhs.isbn();
}
using SD_multiset = unordered_multiset<Sales_item, decltype(hasher)*, decltype(eqOp)*>;
//参数分别代表桶大小、hash函数指针、相等性判断函数指针，有默认值的情况下可省
SD_multiset bookstore(42, hasher, eqOp);  
```

# 动态内存

## 智能指针

### shared_ptr
1. shared_ptr类也是模板，shared_ptr<string> p = make_shared<string>(10, '9');
2. 递增：初始化其他值、作为参数传递给函数、作为函数返回值  
   递减：析构函数(若为正则递减，为零则释放内存) -> 被赋予新值、局部shared_ptr离开作用域
3. new对没有定义默认初始函数的类(依赖编译器合成的默认初始函数也算)值初始化会得到未定义值  
   delete释放非new分配的空间或非空指针会产生未定义结果，const值不可被改变但可被销毁  
   delete之后指针变成空悬指针，状态和未初始化指针类似，手动置为nullptr可以对该指针(其他指向被释放内存指针无效)提供有限保护
4. 接受指针参数的智能指针构造函数时explicit的，只能使用直接初始化shared_ptr<int> p(new int(42))，而不能使用shared_ptr<int> q = new int (1024)，因为他试图将int*隐式转化，函数返回值也一样
```c++
void process(shared_ptr<int> ptr) { };
shared_ptr<int> p = make_shared<int>(42); //正确做法
//错误做法：临时对象被销毁时，其指向的内存被释放了
int *x(new int(42));
process(shared_ptr<int>(x));  //合法但是内存被释放了
int i = *x; //未定义，x变成了空悬指针
//另外，将另一个智能指针绑定到get返回的内置指针上也会造成错误，且不能delete这个内置指针
if (!p.unique())
  p.reset(new int(*p)); //p = new int(*p);错误
*p += newVal; //改变对象的值而不影响其他用户
```

### 异常
1. 使用智能指针能保证在出现异常，程序块过早结束离开作用域时正确释放分配的自由空间
2. 哑类、析构函数
```c++
connection connect(destination*);
void disconnect(connnection);
void f(destination &d){
  connnection c = connect(destination);
  //如果没有disconnect且connection没有定义析构函数，则退出后永远无法关闭了
}
void end_connection(connection *p) {disconnect(*p);}
void f(destination &d){
  connnection c = connect(destination);
  shared_ptr<connection> p(&c, end_connection); 
  //智能指针一般假定指向了动态内存，并会自动用定义的删除器函数(替代delete)对shared_ptr中保存的指针完成释放操作,因为不是new分配的指针，所以也不能使用默认的delete
  //当f退出(即使由异常引起)时，正常关闭connection
}
```

### unique_ptr
1. unique_str需要使用new初始化，也只能直接初始化(explicit)
2. unique_str<T, D> u(d); p = u.release(); u = reset(q); release()不会释放内存且会失去对指针的控制权，如果不用另一个智能指针保存，就要用户自己负责空间释放
3. unique_ptr不能拷贝或赋值，但是即将销毁的除外，比如函数返回unique_ptr
4. 与shared_ptr不同，自定义删除器需要在定义时指定删除器类型，创建对象时指明对应删除器类型对象

### weak_ptr
1. weak_ptr不控制对象生存期，由于指向的对象可能不存在，调用lock()访问对象
```c++
if(shared_ptr<string> sp = wp.lock()) { ... } //sp不为nullptr则条件为真
```

## 动态数组

### new动态数组
1. int *pia = new int[get_size()];  //pia指向第一个int，get_size()不必是常量，分配的动态数组不是数组，所以不调用begin,end,因为他们使用了数组维度(数组类型的一部分)，所以也不能使用范围for
2. 默认使用默认初始化、使用圆括号值初始化、C++11可以使用元素初始化器的花括号列表
3. 动态分配空数组：char *cp = new char[0];cp不能解引用，可以进行尾后指针的操作(±0,减自身)且保证唯一
4. delete [] pia;pia为空或指向动态数组，元素按逆向销毁
5. 智能指针
```c++
unique_ptr<int[]> up(new int[10]);//<int[]>
up.reset(); //当up销毁它管理的指针时，自动调用delete []。指向数组的unique_ptr不支持成员访问运算符(数组不是单个对象)，但可以用下标运算符访问元素
shared_ptr<int> sp(new int[10], [](int *p) {delete [] p;});//<int>
sp.reset(); //shared_ptr必须提供自定义的删除器
```

### allocator类
1. 把内存的分配和对象的构造、对象的析构和内存的释放分离开来，allocator分配的内存是原始未构造的
2. 不能使用未构造对象的原始内存，只能对真正构造了的元素使用destory
```c++
allocator<T> a; //创建一个对象a负责类型T的动态内存管理
a.allocate(n), a.deallocate(p, n);
a.construct(p, args), a.destory(p);
uninitialized_copy(b, e, b2), uninitialized_copy_n(b, n, b2); //在给定目标位置拷贝
uninitialized_fill(b, e, t), uninitialized_fill_n(b, n, t); 
```

## 标准库实战 P430

# 拷贝控制
拷贝构造、拷贝赋值、移动构造、移动赋值、析构
## 拷贝赋值销毁
1. 拷贝构造函数，第一个参数为自身类型的引用(一般const)，且其他参数都有默认值，为了隐式使用一般非explicit
2. 拷贝初始化：用=定义变量、非引用类型的参数和返回值、花括号列表初始化[拷贝初始化](#直接初始化与拷贝初始化)
3. 拷贝初始化用于初始化非引用类类型参数，所以若拷贝构造函数是非引用参数，调用则会失败(无限循环)
4. 拷贝赋值运算符：左侧运算对象绑定到隐式的this指针，右侧对象显式表示，一般返回一个左侧运算对象引用
5. 析构函数：函数体(成员销毁步骤外的部分)+析构部分(隐式)，对类成员隐式使用对应析构函数、内置类型成员没有析构函数所以什么也不需要做。普通指针是复合类型而智能指针是类类型。
6. =default,类内定义则合成的函数被隐式声明为内联

### 三/五法则
1. 需要析构函数的类也需要拷贝和赋值操作(new delete例子，默认拷贝只拷贝指针而不新建内存空间)
2. 需要拷贝操作的类也需要赋值操作，反之亦然(唯一编号例子)
3. 析构 -> 拷贝构造、拷贝赋值 -> 移动构造、移动赋值(如果只定义了移动操作，拷贝操作会是删除的，反之亦然)

### 阻止拷贝
1. 可以对任意成员函数定义为删除的函数，阻止编译器使用或合成对应函数，但是若析构函数为删除的成员，则无法定义(拷贝构造函数为删除的)或释放该类对象(可以new，但不能delete)
2. 若类有成员不能默认构造、拷贝、复制或销毁，则对应的成员函数将被定义为删除的；若一个类有const成员，则不能使用合成的拷贝赋值运算符；若有引用成员，则合成的拷贝赋值运算符会被定义为删除的。
3. 新标准前也可以使用private进行拷贝控制(显式的出现链接错误)

## 拷贝控制和资源管理
类的行为：像值、像指针、无法拷贝赋值

### 类值类
1. 拷贝构造或析构时也要分配或释放资源
2. 拷贝赋值运算符销毁左侧运算对象的资源、并从右侧拷贝数据和资源
3. 拷贝赋值运算符要注意防范自赋值操作(销毁左侧运算对象前先拷贝右侧运算对象)

### 类指针类
1. 当最后一个类指针类的对象被析构时，要释放其管理的资源。最好使用shared_ptr来管理资源，也可以使用引用计数的方法来直接管理资源
2. 引用计数与shared_ptr管理use_count()值类似，构造函数(除了拷贝构造)需要创建引用计数并初始化1；拷贝构造函数递增共享成员的计数器；析构函数递减共享资源计数器，若变为0则释放状态；拷贝赋值函数递增右侧对象共享资源计数器而递减左侧运算对象共享资源计数器，若左侧计数器变为0则需销毁；->在动态内存存放计数器以共享计数器状态
3. 为了防止自赋值操作出现错误，应该先递增右侧计数器，再递减左侧运算器

## 交换操作
1. 类型特定版本swap和std::swap
```c++
class HasPtr{
  friend void swap (HasPtr &, HasPtr &);
  ...
}
inline void swap(HasPtr &lhs, HasPtr &rhs){
  using std::swap;  //并不会隐藏特定版本swap P706
  swap(lhs.ps, rhs.ps); //若HasPtr.ps有特定的swap优先使用，否则用std::swap
  ...
}
```
2. 赋值运算符中使用swap，拷贝并交换  
   天然处理了自赋值情况，且异常安全(无论是类值类还是类指针类)
```c++
HasPtr& HasPtr::operator= (HasPtr rhs){ //传值方式拷贝
  swap(*this, rhs);
  return *this; //析构rhs，即this原来指向的对象(运算符左侧对象)
}
```

## 动态内存管理类及示例 P460、P464
[动态内存管理例(与书上不同)](CPP_Primer_exercise/StrVec.cpp)
```c++
class StrVec {
public:
    friend void swap(StrVec &, StrVec &);

    StrVec() : first(nullptr), next(nullptr), cap(nullptr) { }
    StrVec(const StrVec &);
    ~StrVec();
    StrVec& operator= (StrVec);

    string *begin() const {return first;}
    string *end() const {return next;}
    size_t size() const {return next - first;}
    void push_back(const string &);

private:
    string *first, *next, *cap;
    static allocator<string> alloc;
    void free();
    void chk_space();
};

StrVec::StrVec(const StrVec &sv) {
    first = alloc.allocate(sv.end() - sv.begin());
    next = uninitialized_copy(sv.begin(), sv.end(), first);
    cap = next;
}

void StrVec::free() {
    if(first){
        for(auto last = next; last != first; ){
            alloc.destroy(--last);
        }
        alloc.deallocate(first, cap - first);
    }
}

void StrVec::chk_space(){
    if(next == cap){
        string *new_first = alloc.allocate(2 * (cap - first) + 1);
        string *new_cap = new_first + 2 * (cap - first) + 1;
        string *new_next = uninitialized_copy(first, next, new_first);
        free();
        first = new_first, next = new_next, cap = new_cap;
    }
}

void StrVec::push_back(const string &s){
    chk_space();
    alloc.construct(next ++, s);
}

StrVec::~StrVec(){
    free();
}

inline void swap(StrVec &lhs, StrVec &rhs){
    string *tmp_first = rhs.first, *tmp_next = rhs.next, *tmp_cap = rhs.cap;
    rhs.first = lhs.first, rhs.next = lhs.next, rhs.cap = lhs.cap;    
    lhs.first = tmp_first, lhs.next = tmp_next, lhs.cap = tmp_cap;
}

StrVec& StrVec::operator= (StrVec sv){
    swap(*this, sv);
    return *this;
}
```
## 对象移动

### 右值引用
1. 左值引用无法绑定到要求转换的表达式、字面常量或返回右值的表达式，右值引用可以绑定上述表达式但不能直接绑定到左值上：右值引用只能绑定到临时对象，所以可以自由接管其所引用资源  
   std::move使我们可以像对待一个右值一样处理左值。调用move后我们即不能对移后源对象的值做任何假设，只能对该对象赋值或销毁

```c++
int &&rr1 = 42;
int &&rr2 = rr1; //error! 右值引用类型作为一个变量是左值
int &&rr3 = std::move(rr1); //ok! move返回给定对象的右值引用
```
1. 移动构造函数和移动赋值运算符必须保证移后源对象可析构且有效(支持除取值外的操作)，即真正移交资源所有权而不保留
```c++
StrVec::StrVec (StrVec &&s) noexcept : first(s.first), next(s.next), cap(s.cap){
  s.first = s.next = s.cap = nullptr; //保证析构s不影响移动的资源
}
StrVec& StrVec::operator= (StrVec &&s) noexcept {
  if(this != &s) {  //保证自赋值无误
    free(); //释放原来的资源
    first = s.first, next = s.next, cap = s.cap;
    s.first = s.next = s.cap = nullptr;
  }
  return *this;
}
//拷贝并交换可以同时生成拷贝赋值运算符和移动赋值运算符
//根据实参是左值还是右值，匹配拷贝构造函数或移动构造函数进行拷贝初始化
StrVec& StrVec::operator= (StrVec s) {  
  swap(*this, s); //交换状态
  return *this;   //销毁s
}
```
2. 移动过程中窃取资源而不分配资源，一般不会发生异常，用except说明，防止标准库为了对异常发生时对自身行为提供保障而做额外工作。如：在插入元素扩大capacity重新分配内存空间时若发生异常，标准库需要保证原对象值不变，如果不说明noexcept标准库会使用拷贝构造函数而非移动构造函数。
3. 合成的移动构造函数/移动赋值运算符：只有当没有定义任何拷贝控制成员且类每个非static成员都可移动时才自动合成，当有不可移动成员且定义为=default将移动操作定义为删除的函数。
4. 移动迭代器：解引用获得右值引用(一般获得左值)。
```c++
auto next = uninitialized_copy(make_move_iterator(vec.begin()), make_move_iterator(vec.end(), dest));
```
5. 引用限定符：在成员函数声明和定义时都要加上限定符，只能作用于(非static)成员函数，引用限定符必须跟在const限定符后(const &)
```c++
Foo sorted() &&;      //用于右值排序，由于没有其他用户所以可以安全地改变对象
Foo sorted() const &; //用于任何类型Foo
Foo sorted() const;   //error! 如果一个成员函数有引用限定符，所有具有相同参数列表和函数名函数都要加上引用限定符
```

# 重载运算符与类型转换
## 重载运算符
1. 重载后的运算符优先律和结合律与对应内置版本保持一致，但是逻辑运算符与逗号运算符的求值顺序与最短路求值不保留，所以一般不进行重载，因为重载后的运算符应该和内置类型的含义相同
2. 成员或非成员的区别：
```c++
obj1 + obj2;  
//作为类型成员，隐含*this作为第一运算对象，obj2要能转换为obj1的类型 - 改变对象状态、成员访问等
obj1.operator+(Cls obj2); 
//不作为类型成员，只要两个实参能转换为对应形参即可 - 具有对称性的运算符可能转换为任意一段类型
operator+(Cls obj1, Cls obj2); 
```
3. 输入输出运算符：左侧形参为非常量iostream对象引用，并返回其引用，只能作为非成员函数  
   输入操作符应该对IO错误进行处理，保证对象处于可用状态
4. 算术和关系运算符：一般定义为非成员函数以允许左右对象转换，形参一般为常量引用(无需改变对象状态)  
   如果同时定义了算术运算符和复合运算符，通常用复合运算符来实现算术运算符  
5. 赋值运算符、复合赋值运算符：也可以接受initializer_list来支持花括号表示的列表
6. 下标运算符：通常定义两个版本，返回普通引用和返回常量引用
7. 递增递减运算符：区分前置后置，如果需要显式调用后置版本则传入一个值p.operator++(0)以告知编译器
8. 成员访问运算符：箭头运算符根据调用它的是指针还是对象调用内置版本或自定义版本，自定义版本则使用自定义操作返回值来获取成员即(obj.operator->())->mem;直到返回值为某对象指针时调用内置箭头运算符返回成员，即一定包含成员访问含义。
## 可调用对象
1. 函数调用运算符必须是成员函数，可以有多个重载版本，定义了函数调用运算符的类对象为函数对象
2. lambda表达式是函数对象,产生的类不含默认构造函数、赋值运算符和默认析构函数，根据捕获对象类型决定是否含默认拷贝/移动构造函数
```c++
find_if(words.begin(), words.end(), [sz](cosnt string &s){ return s.size() > sz; });
class SizeComp {
public:
  SizeComp(size_t n) : sz(n) {};
  bool operator() (const string &s) const {return a.size() > sz;}
private:
  size_t sz;
}
find_if(words.begin(), words.end(), SizeComp(sz)); //默认初始化一个ShorterString对象
```
3. 标准库函数对象 greater<Type>、greater_equal<Type>等
4. 可调用对象：函数、函数指针、lambda、bind创建的对象、重载了函数调用运算符的类
5. 可调用对象和调用形式存入调用表中，如map<string, int(*)(int,int)> binops;则可以将{"+",add}存入binops(add必须为规定调用形式的普通函数类型，lambda或函数对象不行，因为他们有自己独有的类型)
6. function模板:function<int(int,int)> f;f用来存储一个可调用对象这样就可以将函数表表示为：  
   map<string, function<int(int,int)>> binops;  
   重载函数不能直接存入function对象中，解决二义性可以使用函数指针或lambda(新建一个调用了对应函数重载版本的可调用对象)

## 重载、类型转换、运算符
用户定义的类型转换：转换构造函数 + 类型转换运算符
1. 类型转换运算符：隐式执行，可以先类类型经过定义的类型转换运算符转换为一个类型，再把得到的结果经过编译器的隐式类型转换为另一个类型。
```c++
//假设cin定义了向bool的类型转换运算符
int i = 42;
cin << i; //cin并没有<<运算符，所以cin被隐式转换为bool类型，再被提升为int，所以变成了一个左移运算，显然错误但编译器可以正常通过
```
2. C++11引入了显式类型转换运算符explicit，但当表达式被作为条件时显式转换会被隐式执行
```c++
class SmallInt {
  friend SmallInt operator+ (const SmallInt&, const SmallInt&);
public:
  SmallInt(int i=0) : val(i){if(i<0 || i>255) throw out_of_range("Bad Value!")}
  operator int() const {return val;}
  explicit operator bool() const {return val>0;}
  SmallInt operator+ (const SmallInt &si) const {return SmallInt(val + si.val);}
private:
  size_t val;
}
SmallInt si = 3.14;
si + 3.14;
static_cast<bool>(si) + 1;
si ? f() : g();
```
3. 类型转换二义性：相同的类型转换(A->B,B->A)、存在多种转换路径(构造函数，类型转换运算符：本质由于转换所需标准类型转换级别没有区别)  
   除了显式地向bool转换外，尽量避免定义类型转换函数并限制非显式构造函数
4. 重载函数接受需要类型转换的实参时，我们认为自定义的各类型转换级别相同，在此过程中不会考虑标准类型转换级别(即使有路径不转换)
5. 重载运算符的候选函数集包括左侧运算对象的成员函数和普通非成员函数,如果我们既定义了向算术类型的类型转换又定义了重载运算符，运算符会产生二义性问题
```c++
SmallInt s1, s2;
SmallInt s3 = s1 + s1;  //ok!
int i = s3 + 0; //error!二义性调用
```
# OOP
数据抽象(接口与实现分离)、继承(相似关系建模)和动态绑定(统一调用)
## 基类派生类定义
1. 每个类控制自己的成员初始化过程；先初始化基类成员，然后按声明顺序初始化派生类成员；final
2. 静态类型(编译)、动态类型(运行)；不存在对象间的类型转换，只有类型对象的引用或指针向下转换为运行时的动态类型
## 虚函数
1. 虚函数必须定义(因为运行时才知道是否使用)；虚函数覆盖必须保证返回类型和形参列表相同，否则会隐藏外层作用域同名函数(基类)
2. final、override
3. 使用作用域运算符回避虚函数调用机制(用于定义派生类覆盖的虚函数)
4. 抽象基类：纯虚函数 =0；负责定义接口，后续其他类可以覆盖；不能创建一个抽象基类对象

## 访问控制
1. public、protected、private；派生类的成员和友元只能通过派生类对象来访问基类的protected成员
2. public、protected、private也能用于继承关系说明，影响**派生类用户**对派生类对基类可访问成员(public、protected)的访问权限
3. 派生类向基类的转换：当基类的公有成员对派生类用户可访问时才可转换
4. 友元关系无法传递和继承，每个类负责控制自己成员的可访问性
5. using声明访问权限

## 作用域
1. 嵌套关系：派生类作用域嵌套于基类；派生类的成员会隐藏基类的同名成员(除了当与同名虚函数形参列表、返回值一致时覆盖override)
2. 静态类型决定了哪些成员可见：首先从静态类型对应类查找mem，并依次向上(向外、向直接基类)查找，知道找到或到达继承链顶，然后若mem是虚函数且通过引用或指针调用则依据动态类型运行具体版本
3. 当派生类存在与基类虚函数同名但形参列表、返回类型不同的成员函数，派生类继承了虚函数定义但是被同名函数隐藏了，即派生类的派生类也能继承此虚函数
```c++
struct Base {
  virtual int f1();
};
struct D1 : public Base {
  int f1(int);  //隐藏了虚函数Base::f1(),普通成员函数
  virtual void f2();
};
struct D2 : public D1 {
  int f1(); //覆盖了D1继承自Base的虚函数f1()
  void f2();  //覆盖f2()
};
Base bobj; D1 d1obj; D2 d2obj;
Base *bp = &d1obj; bp->f1();  //虚调用，调用Base::f1()
D1 *d1p = &d1obj; d1p->f1();  //no matching function for call to 'D1::f1()'
D2 *d2p = &d2obj; d2p->f1(42);  //静态绑定，调用D1::f1(int)
```
## 构造函数与拷贝控制
1. 虚析构函数：确保delete基类指针时动态绑定正确的析构函数版本；如果一个类定义了析构函数(即使=default)会阻止编译器合成移动操作(但可以定义一个=default版本，除非有排斥移动的成员)
2. 合成的拷贝控制函数还负责使用直接基类对应操作进行初始化、赋值或销毁(若可访问且不是删除的)(注意顺序构造与析构顺序相反)
3. 当定义了拷贝或移动操作，该操作要负责包括基类成员在内的整个对象(默认构造函数会默认使用基类默认构造函数初始化基类成员)
4. 在构造和析构过程中，应该把对象的类和函数所属的类看作同一个类型(构造过程中其派生类的成员还未初始化、析构过程中其派生类成员已被析构)
5. 默认、拷贝和移动构造函数不能继承，而是在派生类合成新的构造函数(如果应该)；  
   using声明语句作用于基类构造函数时会令编译器产生对应代码，即隐式的插入了一个构造函数定义derived(parms) : base(args) { };派生类部分将被默认初始化  
   与一般using不同，这里并不改变基类构造函数在派生类中的访问权限，且using不能指定explicit或constexpr(不改变其属性)  
   若基类构造函数含有默认实参，这些实参不会被继承，而是产生多个省略不同数目默认实参的重载构造函数(与函数匹配方式对应)  
   继承多个构造函数时若派生类中有定义对应的版本，将会替换之；默认、拷贝和移动构造函数也不会被继承，而是生成对应合成版本  
   继承的构造函数不会被当作用户定义的构造函数(若没有则合成)使用

## 容器与继承
1. 容器中应该存放基类的指针或智能指针，因为对象的类型不能转换，直接存基类对象会对派生类进行裁剪
2. 代码实战P560

# 模板与泛型
## CRTP



# 拓展资料

## 变量区分类与默认初始化
c++的变量区，可分为四大区域：栈区，堆区，静态区（全局区），常量区

栈区：由编译器分配和释放，主要用于存放局部变量，函数参数等。

堆区：由程序员分配和释放，主要用于指针的声明，如string *p=new("name")，new关键字会在堆区分配内存空间，并构造初始化相应类型变量，返回指针传递给p;

静态区（全局区）：用于存放静态变量和全局变量，其中未初始化的静态变量和全局变量存放在一起，程序结束后由系统自动释放。

常量区：用于存放全局常量，如const int age=24；常量age就存放于常量区，结束后由系统自动释放。

全局变量，存于静态区（全局区）；局部变量，存于栈区，局部变量不能默认初始化是因为它在栈上，全局变量区可以统一清零(所以默认初始化值都是size_t*8个0b)，但是栈上若加了清零操作，会使得函数调用等操作变得更加缓慢，因此编译器取消了此功能。  
[默认初始化相关csdn链接](https://blog.csdn.net/qq_44165157/article/details/109587428)  
[静态变量链接](https://www.yisu.com/zixun/609652.html#:~:text=%E5%B8%B8%E9%87%8F%E5%8C%BA%20c%2B%2B%20%E4%B8%AD%EF%BC%8C%E4%B8%80%E4%B8%AA%20const,%E4%B8%8D%E6%98%AF%E5%BF%85%E9%9C%80%E5%88%9B%E5%BB%BA%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4%EF%BC%8C%E8%80%8C%E5%9C%A8%20c%20%E4%B8%AD%EF%BC%8C%E4%B8%80%E4%B8%AA%20const%20%E6%80%BB%E6%98%AF%E9%9C%80%E8%A6%81%E4%B8%80%E5%9D%97%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4%E3%80%82)  
[常量链接](https://blog.csdn.net/qq_43152052/article/details/99306967)

## 直接初始化与拷贝初始化
若一个类没有拷贝构造函数，当遇到赋值操作符时会自动生成一个public的拷贝构造函数，拷贝初始化先用构造函数生成一个类对应的临时对象(若没有，且非explicit)，并用拷贝构造函数拷贝到要初始化的对象。直接初始化则直接调用构造函数生成对象(可能是拷贝构造函数)
由于编译器优化，=操作有可能优先选择对应参数的构造函数构造,但即使跳过了拷贝/移动构造函数，但在这个程序点上，拷贝移动构造函数也必须可访问
```c++
std::string str("1234");  //std::string::string(const char *);
std::string s0 = "1234";
std::string s1 = str;
std::string s2(str);  //std::string::string(const string &);使用拷贝构造函数直接初始化
ifstream file1("filename"):
ifstream file2 = "filename";//error:constructor is explicit
```
[编译器行为：直接初始化与复制初始化的区别](https://blog.csdn.net/ljianhui/article/details/9245661)  
[结论总结](https://www.jianshu.com/p/69377c778dc2)
