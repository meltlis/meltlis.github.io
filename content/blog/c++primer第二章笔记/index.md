---
title: 'c++primer第二章笔记'
date: '2025-01-15 15:18:02'
authors:
  - me
categories:
  - '学习'
  - 'C++'
math: true
toc: true
---

用于个人学习笔记使用,可能会存在许多错误与模糊的地方。
<!--more-->
# 二.基础变量和类型
## 基础变量
浮点数字面值
3.14159 或 3.14159E0,其中E0为科学计数法表示指数,E0表示指数为0。

C++规定的转义序列:
换行符\n    横向制表符\t    报警(响铃)符\a  反斜线\\    回车符\r    略

Latin-1字符集
\(3位八进制数或X+多个六进制数)

L'a'宽字符型字面值 wchar_t
3.14159L long double

对象是指一块能存储数据并具有某种类型的内存空间。

C++中初始化不等同于赋值。

int a{ld}列表初始化。
列表初始化存在丢失信息的风险时将会报错。

//page 67

## 引用和指针
### 引用
引用即别名,引用非对象。
引用的使用:
```
int &j;//error
int &j = 1;//error 
int &i = j;
i = 1;//j = 1;
```
### 指针
指针指向对象, 引用非对象,因此无法定义指向对象的指针。
```
int ival = 42;
int *p = &ival;//&为取地址符
cout << *p;//&为解引用符,输出p指针指向地址上的内容
```
C++11以后,空指针最好用nullptr而非NULL,尽量初始化所有的指针。
如果指针具有合法值,就能在条件表达式当中采用算术值为条件。
```
int val = 1024;
int *pi = 0;
int *pi2 = &val;
if(pi)//false
if(pi2)//true
```
void* 指针
```
int* a,b;
int *A, *B;
```
### 引用和指针的区别
一旦定义了引用,就无法令其再绑定到另外的对象,之后每次使用那个引用都是访问它最初绑定的对象。

引用本身不是一个对象,但是指向引用的指针是一个对象,因此可以引用指向引用的指针。如
```
int *ip, &i = ip;//i为ip指针的引用
```

## const限定符
const可以将一个变量的类型加以限定。
```
const int i = 1;
```
默认情况下,const仅文件内有效。对于const变脸的声明与定义添加extern关键字就可以只用定义一次:
```
extern const int bufsize = fcn();//定义并初始化
extern const int bufsize;//头文件
```
### 引用与const
const也可以进行引用
```
const int i = 10086;
const int &is = i;
```
常量引用绑定非常量对象是合法的:
```
double a = 3.14;
const int &a1 = a;
```
这个操作是合法的,然而不允许通过a1改变a的值。如果该变量可以转换成const对应的类型,则编译器会生成一个临时量来实现对象的绑定。其间编译器会将上述代码变为以下形式:
```
const int temp = a;
const int &a1 = temp;
``` 
### 指针与const
指向常量的指针(pointer to const)不能改变其所指向对象的值,但是可以指向常量。它会“自以为”指向了一个常量。普通变量指针不能指向常量:
```
const double pi = 3.14;
double pp = 3.14;
const double* p1 = &pi, p2 = &pp;//合法
double *p0 = &pi;//不合法
```
顶层const:指针本身是常量。底层const:指针所指对象为常量。

## constexpr和常量表达式
常量表达式指值不会改变且在编译过程就能得到计算结果的表达式。
```
constexpr int mf = 20;
constexpr int m = mf + 1;
constexpr int sz = size();//size定义为constexpr函数才是正确的声明语句
```
constex将变量认定为一个常量表达式,声明为constexpr的变量初始值一定是常量,且constexpr函数也应该足够简单,所以可以在编译环节就能获取到。constexpr指针初始值只能为nullptr或者0,或者是存储与某个固定地址中的对象。

## 处理类型

### 类型别名
```
typedef double wages;//传统方法
using SI =  Sales_items;//别名声明
```

### auto类型说明符
auto让编译器来分析表达式所属于的类型,auto初始化中所有变量的基本数据类型必须一样。
```
auto item = val1 + val2;//item会根据val1和val的类型来自动确定变量类型。
auto i = 0, *p = &i;//正确
auto j = 0, pi = 3.14;//错误 
```

### decltype类型指示符
decltype允许变量通过操作数确定变量的类型,但是却不实际计算表达式的值。
```
decltype(f()) sum = x; //sum的类型为f()返回的类型
```
此时编译器只会调用f()的返回值类型,并不会完整调用f()函数。如果操作数的类型是一个常量的话,需要进行初始化。
当操作数为引用时,decltype()的结果也将会是引用类型。若想让结果类型为引用的所指类型,可以将引用作为表达式的一部分。例如:
```
int i = 42, *p = &i, &r = i;
decltype(r + 0) b;
decltype(*p) c;//错误, int&需要初始化。
```
当表达式内容是解引用操作时,decltype将会得到引用类型。
在decltype中,变量名不加括号将会得到变量的类型,如果加上一层或多层,就会被视作表达式,变量是一种可以作为赋值语句左值的特殊表达式,此时decltype得到引用类型。
```
decltype((i)) d;//int&,此时是引用类型,需要初始化
decltype(i) e;//int
```
双层括号的情况下,得到的结果永远是引用,单层括号只有在操作数本身是一个引用时才会是引用。
> 头文件：#include<typeinfo>
> 获取变量类型名称: typeid(变量).name()
> 返回值取决于编辑器

练习题
```
#include<iostream>
#include<typeinfo>
int main(){
    int a = 3, b = 4;
    decltype(a) c = a;
    decltype(a = b) d = a;
    std::cout << "a(" << typeid(a).name() << ")=" << a <<std::endl;
    std::cout << "b(" << typeid(b).name() << ")=" << b <<std::endl;
    std::cout << "c(" << typeid(c).name() << ")=" << c <<std::endl;
    std::cout << "d(" << typeid(d).name() << ")=" << d <<std::endl;
    return 0;
    //a = 4(int), b = 4(int), c = 4(int) d = 4(int);
    其中(a = b)返回的是对a的左值引用，因此d是引用类型。
}
```
### auto与decltype的区别
|特性|decltype|auto|
|:---|:---:|:---:|
|用法|推导表达式类型，**不进行初始化**|推导变量类型，**进行初始化**|
|推导依据|基于表达式或变量的静态类型|基于初始化表达式的实际类型|
|适用范围|用于任何表达式|仅可用于变量|
|引用与修饰符|可**保留引用与const修饰符**|通常**忽略顶层const和引用修饰符**|
```
int x = 42
decltype(auto) a = (x);//a为&int
auto b = (x);//b为int
```
## 自定义数据结构
### 简单类的初步定义
```
struct Sales_data{
    std:: string bookIbsn;
    unsigned sold_num = 0;
    double revenue = 0;
};//没有初始值的成员将会被默认初始化
Sakes_data accum, trans, *salesptr;//类与对象的定义也可以放一起写
```
### 使用类
```
#include<iostream>
#include<string>
#include"Sales_data.h"
int main(){
    Sales_data data1, data2;
    int price;
    std::cin >> data1.bookIbsn >> data1.sold_num >> price;
    data1.revenue = price * data1.sold_num;
    std::cin >> data2.bookIbsn >> data2.sold_num >> price;
    data2.revenue = price * data2.sold_num;//分别计算两个对象的总金额
    if(data1.bookIbsn == data2.bookIbsn){//判断是否为相同的IBSN
        unsigned totalcnt = data1.sold_num + data2.sold_num;
        double totalrevenue = data1.revenue + data2.revenue;
        if(totalcnt != 0)
            std::cout << "Sold " << totalcnt << std::endl <<"Earned a total of "<< totalrevenue << std::endl;
        else
           std::cout << "No sales" << std::endl;
        return 0;
    }
    else{
        std::cerr << "Data should refer to the same Ibsn" << std::endl;
        return -1;
    }//处理错误情况
}
```
### 编写头文件
头文件保护符依赖于预处理变量。当变量已定义#ifdef为真,变量未定义时#ifndef为真;再用#define将名称设置为预处理变量,检查结果为真将会执行后续操作直到#endif为止。这样可以避免重复包含的发生。
```
#ifndef SALES_DATA_H //未定义是为真
#define SALES_DATA_H //定义防止重复包含该头文件
#include <string>
struct Sales_data {
    std::string bookIbsn;
    unsigned sold_num = 0;
    double revenue = 0;
};
#endif //执行到#endif
```
