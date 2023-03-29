```c++
#include <iostream>//头文件让程序知道该类的定义
#include <string>//让程序知道string类的定义
using namespace std;
int main(){
	//语句是最小独立单元
	string user_name;//声明语句
	cin>>user_name;
	cout<<'\n'
		<<"hello, "
		<<user_name;
	return 0;//返回0表示程序正常运行，然会1表示程序出错（一般情况下）
}
```
### 字符类型：
- 可打印字符：'a'等
- 不可打印字符：'\n'等，必须以两个字符组成的字符序列表示

### 命名空间namespace：
```c++
using namespace std;
```
将程序库名称封装起来的方法，避免和应用程序发生[命名冲突](命名冲突)的问题。

### 初始化
初始化：用来表示该对象尚未具有真正意义的值
#### 初始化方法
- 以’=‘运算符进行初始化：沿袭c语言中的初始化方法，适用于内建类型对象或者单一值对象初始化
```c++
int a=0;
```
- 构造函数初始化：多个值的对象的初始化只能使用这个
```c++
//复数，一个实部，一个虚部
complex<double> purei(0,7);
```
*complex后面的尖括号表示这是一个模板类，模板类可以允许我们在不必指明data member的类型的情况下对class进行定义，在使用complex的时候才决定真正的类型*

### 转义序列
![[Pasted image 20230326183333.png]]

### const
用const定义过的变量，在获得初值之后，不会再发生改变，如果对其进行改变，编译会出错

## 表达式
### 复合赋值运算符
*+=、/=等*
#### 递增运算符和递减运算符
- 前置：++n，前置形式会对原值进行递减或者递增之后，再被拿给表达式使用。
- 后置：n++，后置形式会先将原址给表达式进行计算，再进行递增或者递减。

```c++
#include <iostream>
using namespace std;
int main()
{
    int n = 0;
    //前置，先对n进行递增，再输出
    cout << ++n << endl;
    //后置，先输出n，再进行递增
    cout << n++ << endl;
}
```
### 运算符的优先级：
*括号可以改变优先级*
从上到小，从左到右优先级递减
![[Pasted image 20230326190055.png]]
### 条件语句和循环语句
#### 条件语句：
##### if
##### switch
n是核定值，核定值只能是整数类型。
*break保证每个case之后的语句执行之后，switch语句的结束，而不是接着运行下面的case语句，以至于default语句*
```c++
switch(n){
	case 1:
		break;
	case 2:
		break;
	default:
		break;
}
```

#### 循环语句：
##### while
##### for
*continue：结束这一次循环
return：结束本层循环*
## Arrays和Vectors
### Arrays
*array的尺度必须是一个常量表达式，也就是在运行时候不需要进行计算的值*
```c++
const int seq_size=18;
int pell_seq[seq_size];
//初始化
int pell_seq[seq_size]={1,2,3,...};
int pell_seq[]={1,2,3};
```
### Vectors
*vector的尺度不一定是一个常量表达式*
```c++
#include <vector>
vector<int> pell_seq(18);
//初始化
int pell_seq[]={1,2,3};
vector<int> pell_seq{pell_seq,pell_seq+seq_size};
```
## 指针

```c++
int* pi;
pi=&ival;
//存取指针
*pi = 1;
```
**如果对一个没有指向任何对象的指针进行提领，会报错，所以要保证在提领一个指针之前指针要有准确的指向**
*一个没有指向任何对象的指针，他的内含地址为0，我们也称其为null指针*
## [随机数](随机数)
*头文件：cstdlib*
- seed种子：作为生成随机数的种子
- rand（）：随机数生成器
- srand（）：随机数生成器

