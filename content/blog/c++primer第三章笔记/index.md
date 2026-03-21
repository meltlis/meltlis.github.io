---
title: 'c++primer第三章笔记'
date: '2025-01-20 19:59:38'
authors:
  - me
categories:
  - '学习'
  - 'C++'
toc: true
---

用于个人学习笔记使用,可能会存在许多错误与模糊的地方。
<!--more-->
# 命名空间的using声明
```
using namespace::name;
```
使用using声明之后就可以直接访问命名空间中的名字。例如:
```
#include<iostream>
using std::cin;//编译器应当从左侧名字所示的作用域中寻找右边的名字
using std::endl;
int main(){
    int i;
    cin >> i;
    std::cout << i << endl;//std还没有声明，需要使用完整的访问
    return 0;
}
```
每个名字都需要单独的声名。头文件不应当包含using声明。
# 标准库类型string
string作为标准库的一部分,定义在命名空间std中。
```
#include <string>
using std::string;
string s1, s2(s1), s3 = s1, s4("value"), s5 = "value", s6(n, 'c');
```
使用等号是拷贝初始化，不使用等号是直接初始化。


## string对象的操作
![图片](/image/string1.png "string对象的操作")

string的size函数返回的是一个string::size_type类型的值。其是一个无符号类型的值且足够存放下任何string对象的大小。因为string::size_type返回的是一个无符号类型的值,所以用其与有符号类型的值比较的时候要注意转换,比如s.size() < n,当n为小于0的int类型时,结果会为true,因为n会转换成一个较大的无符号类型数。

string在比较中遵循:1.长度不同，若较短的string对象每个字符与更长的对象上对应位置上的字符位置相同则较短对象小于较长对象。2.如果对象在对应位置上的字符不一致,则string对象比较的结果为第一个不同字符比较的结果。3.大小写敏感。

```
string a;
unsigned b = sizeof(a);//b(string对象的大小)恒定为32, 在不同编译器中也可能是40
```

![图片](/image/cctype1.png "cctype头文件中的函数")

# 基于范围的for语句
C++11新标准提供范围for语句:
```
for(declaration:expression)
//expression:对象,表示一个序列 
//declaration:负责定义一个变量用于访问序列中的基础元素,
//会初始化为expression部分的下一个元素值。
    statement
```
例如
```
string s("some string");
for(auto i : s)
    cout << i << std::endl;
```
使用范围for语句改变字符串中的字符
```
string s("some string");
for(auto &i : s)//i为引用
    i = toupper(i);//i为引用,相当于是元素的别名,因此相当于修改了原本的元素。
```
下标索引符[]

# 标准库类型vector
标准库类型vector表示对象的集合,其中所有对象的类型都相同。集合中的每个对象都有一个与之对应的索引,索引用于访问对象。vector也被称为容器。
```
#include<vector>
using std::vector;
vector<int> ivec = 1;           //vector保存int类型对象
vector<Sales_item> Sales_vec;   //vector保存Sales_item类型对象
vector<vector<string>> file;    //vector保存vector对象
vector<int> ivec1(ivec);        //初始化为ivec所有元素的副本
vector<int> ivec0(10, 1);       //初始化10个为1的对象(默认为0)
vector<string> svec{"a", "an", "the"};//列表初始化为3个元素
vector<int> ivec2(10, ivec);    //创建指定数量的元素
//向vector对象中添加元素,直到运行时知道元素的数量
//不能直接用下标索引符[]添加元素,只能对确知已执行的元素执行下标操作
int i = 0;
ivec.push_back(i);//
```
vector其它操作
![图片](/image/vector1.png "vector的其它操作")
其方法与访问string对象中字符的方法差不多。
```
//练习3.17
int main(){
    vector<string> s;
    string is;
    while(cin >> is){
        for(auto &i : is)
            i = toupper(i);
        s.push_back(is);
    }
    for(auto i : s)
        cout << i << std::endl;
    return 0;
}
```
```
//练习3.20
int main(){
    vector<int> vint;
    int i, n;
    while(cin >> i)vint.push_back(i);
    n = vint.size();
    //没有相邻的元素存在
    if(n <= 1){
        std::cerr << "Error data!";
        return -1;
    }
    //上半问
    cout << "t1:" << endl;
    for(i = 0; i < n - 1; ++i)
        cout << vint[i] + vint[i + 1] << endl;
    //下半问
    cout << "t2:" << endl;
    for(i = 0; i < n; ++i)
        cout << vint[i] + vint[n - 1 -i] << endl;
    return 0;
}
```
# 迭代器
string与vector都支持迭代器。
begin与end成员
```
auto b = v.begin(); //返回指向第一个元素的迭代器
auto e = v.end();   //返回尾元素的下一个位置的迭代器
//容器为空时,begin与end返回同一个迭代器   
```
当对象时常量时,begin和end返回const_iterator,否则返回iterator。如果只需要读操作,可以用cbegin和cend两个函数来强行返回const_iterator。
![图片](/image/ddq1.png "迭代器的运算符")
```
//迭代器实现二分
int main(){
    vector<int> vint;
    int a;
    int sought;
    cin >> sought;
    while(cin >> a)vint.push_back(a);
    auto beg = vint.begin(), end = vint.end();
    auto b = beg;
    auto mid = vint.begin() + (end - beg)/2;
    while(mid < end && *mid != sought){
        if(sought < *mid)           //判断是否在前半部分
            end = mid;              
        else                        //在后半部分
            beg = mid + 1;
        mid = beg + (end - beg)/2;
    }
    cout << mid - b;
    return 0;
}
```
# 数组
## 数组的声明
a[d],其中a是数组的名字,d是数组的维度。数组的维度应当大于0,且在编译的时候应该是已知的。
```
int a[10];  //10个整数的数组
int *pa[10];//10个整型指针的数组
char b0[] = {'c', '+', '+'};
char b1[] = {'c', '+', '+', '\0'};
char b2[] = 'c++';//字符串最后会有一个空字符
char b3[2] = 'c++';//错误,没有放最后一个空字符的空间

int *ptrs[10];//含有10个整形指针的数组
int (*parray)[10] = &arr;//指向一个含有10个整数的数组
int (&arrref)[10] = arr;//引用一个含有10个整数的数组
int *(&arry)[10] = ptrs;//arry引用,该数组有10个指针
//数组声明的含义从数组的名字由内向外的顺序阅读
```
## 访问数组元素
数组也可以使用范围for语句或下标运算符来访问。在使用数组下标的时候,通常将其定义为size_t类型。
在使用数组时应当要检查下标是否在合理范围之内。
## 指针与数组
使用数组时,编译器一般会把它转换成指针。
```
string nums[] = {"one", "two", "three"};
string *p = &nums[0];
string *p2 = nums;//数组名编译器会将其替换为首元素的指针,等价于p2 = &nums[0]
``` 
一些情况下数组的操作实际上是指针的操作。
比如
```
int ia[] = {0,1,2,3,4};
auto ia2(ia);
```
初始化会类似于
```
auto ia2(&ia[0]);
++ia2; //指向ia[1];
int *e = &ia[10];//获取尾后指针
```
### 指针也是迭代器
auto推断的类型为指针而非数组。此时指针也可以作为迭代器使用。也可以利用尾元素后不存在的那个元素获取尾后指针。尾后指针和迭代器一样不指向具体的元素,不能对尾后指针进行解引用或者递增的操作。
### begin与end函数
数组中有begin和end函数,可以更简单安全地获取数组的首尾指针。
### 指针的运算
给指针加上一个整数,得到的新指针依然指向同一数组的其他元素或者同一数组尾元素的下一位置(尾指针)。和迭代器相同,两个指针相减的结果是其距离,结果是一种名为ptrdiff_t的标准库类型(带符号)。
如果两个指针指向同一个数组的元素或尾元素的下一位置,就能运用关系运算符比较。两个指针指向的对象不相关则不能比较。
### 指针与下标
标准库类型string与vector也能执行下标运算,不过string与vector要求下标是无符号类型,内置的下标运算无此要求。结果地址必须指向原来指针所指(或尾指针)的同一数组的元素。
```
int *beg = begin(ia);
int *end = end(ia);
while(beg != end && beg >= 0)beg++;//寻找第一个负数
int *ip = ia2;//ip指向ia[1]
ip += 2;//ip指向ia[3];
int len = end(ia) - begin(ia);//为数组中的元素个数。
while(ia2 < ip){
    ia2++;
    cout << *ia2;//数组指针解引用
}//遍历ia2到ip
int a = *(ip - 3);//a为ip[0]
int k = ip[-2];//ip[-2]为ia[1]
```
### 指针与数组练习
3.36
```
int main(){
    int n1[] = {1, 2, 3};
    int n2[] = {1, 2, 3};
    int *p1 = n1, *p2 = n2;
    auto end1 = std::end(n1);
    auto end2 = std::end(n2);
    //判断元素数目是否相同
    if((end1 - p1) != (end2 - p2)){
        cout << "These two arrays are different." << std::endl;
    }
    //若元素数目相同再判断具体元素
    else{
        while(p1 < end1 && p2 < end2 && *p1 == *p2){
            ++p1;
            ++p2;
        }
        if(p1 != end1)cout << "These two arrays are different." << std::endl;
        else cout << "These two arrays are the same." << std::endl;
    }
    //vector迭代器类似
    vector<int> vint1 = {1, 0, 3}, vint2 = {1, 2, 3};
    auto vp1 = vint1.begin(), vp2 = vint2.begin();
    auto endv1 = vint1.end(), endv2 = vint2.end();
    if((endv1 - vp1) != (endv2 - vp2)){
        cout << "These two vectors are different." << std::endl;
    }
    else{
        while(vp1 < endv1 && vp2 < endv2 && *vp1 == *vp2){
            ++vp1;
            ++vp2;
        }
        if(vp1 != endv1)cout << "These two vectors are different." << std::endl;
        else cout << "These two vectors are the same." << std::endl;
    }
    return 0;
}


```
## C风格字符串
将字符串存放在字符数组中并以空字符('\0')结束。最好别用。
### C标准库String函数(定义于Cstring头文件中)
```
strlen(p); //返回p的长度
strcmp(p1, p2); //比较p1和p2是否相等, 相等返回0; 如果p1>p2, 返回一个正值, 如果p1小于p2, 返回负值
strcat(p1, p2); //将p2附加到p1之后, 返回p1
strcpy(p1, p2); //将p2拷贝给p1, 返回p1
```
实际上p就是char*类型。
C风格字符串比较的是const char*的值。
```
string s1 = "A string example";
string s2 = "A other string";
if(s1 < s2)//false;
//等效于
if(strcmp(c1, c2) < 0);

const char ca1[] = "A string example";
const char ca2[] = "A different string";
if(ca1 < ca2)//未定义, 比较两个无关的地址

//连接两个字符串
string largeStr = s1 + " " + s2;
//ca1 + ca2相当于将两个指针相加,无意义
strcpy(largeStr, ca1);  //cal拷贝给largeStr
strcat(largeStr, " ");  //在largeStr的末尾加一个空格
strcat(largeStr, ca2);  //把ca2连接到largeStr后面
//需要正确估计largeStr所需要的空间, 这种代码具有风险, 容易导致严重的安全泄露
```

## 与旧代码的接口
### string对象与C风格字符串
任何出现字符串字面值的地方都可以用以空字符结束的字符数组替代。

反过来不成立, string无法直接替代C风格字符串, 但可以用c_str成员函数来实现string转换C风格字符串。
```
char *str = s;//错误
const char *str = s.c_str();
```
### 使用数组初始化vector对象
数组可以用来初始化vector对象,但是vector对象不能用来初始化数组。
```
int int_arr[] = {0, 1, 2, 3, 4, 5};
vector<int> ivec(begin(int_arr), end(int_arr));//用数组来初始化vector对象
vector<int> subvec(int_arr + 1, int_arr + 4);//初始化部分对象(int_arr[1], int_arr[2], int_arr[3])
```

## 多维数组
严格来说, C++语言中没有多维数组,通常说的多维数组实际上是数组的数组。当一个数组元素仍然是数组时, 一个维度表示数组本身大小, 另一个维度表示其元素(也是数组)的大小。
```
int ia[3][4];//大小为3的数组,每个元素是含有4个整数的数组
int arr[10][20][30] = {0};//将所有元素初始化为0
```
可以使用花括号括起来一组值初始化多维数组。
```
int ia[3][4] = {
    {0, 1, 2, 3},
    {4, 5, 6, 7},
    {8, 9, 10, 11},
};
```
其中花括号非必须。
```
int ia[3][4] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};
```
只初始化部分元素:
```
int ia[3][4] = {{0}, {4}, {8}};
//显示的初始化每行的首元素。
int ia[3][4] = {0, 3, 6, 9};//剩余元素执行默认初始化。
```
### 使用范围for语句处理多维数组
遍历ia
```
for(auto &row : ia){
    for(auto &col : row)
        cout << col << endl;
}
```
在没有写操作的时候, 最好也选用引用类型作为循环控制变量。在上面的遍历中, 如果不使用引用程序将无法通过编译。因为外层循环时, 编译器会像其它数组一样, 将row视作是指向数组首元素的指针,也就是int*类型, 因此在内层循环时将会变成遍历一个指针, 这显然是非法的。因此外层循环必须是引用类型。
### 指针和多维数组
程序使用多维数组的名字时也会将其自动转换成指向数组首元素的指针。多维数组名转换得来的指针是指向第一个内层数组的指针。
```
int ia[3][4];
int (*p)[4] = ia;//指向的是第一个维度为4的数组
int *ip[4];//整形指针的数组
p = &ia[2];//指向ia的尾元素
```
使用auto 或者decltype可以尽可能避免在数组前添加指针类型。
```
for(auto p = ia; p < end(ia); ++p){
    for(auto q = *p; q < end(*p); q++)
        cout << *q << " ";
    cout << endl;
}
```
### 类型别名简化多维数组的指针
将四个整数组成的数组类型命名为int_array。
```
using int_array = int[4];//C11的类型别名声明
typedef int int_array[4];//等价的typedef声明
for(int_array *p = ia; p! = ia + 3; ++p){
    for (int *q = *p; q!= *p + 4; ++q)
        cout << *q << ' ';
        cout << endl;
}
```
