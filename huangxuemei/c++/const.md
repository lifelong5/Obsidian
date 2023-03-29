### const常量
const定义的常量有类型检查，而define只是字符串的替换
```c++
const int a = 100;
#define a 100
```
### const禁止修改
用const修饰的变量不允许进行修改
```cpp
void f(const int i){
    i++; //error!
}
```
### 指针常量和常量指针
#### 指针常量
指针常量，不能修改指针指向的地址，必须要进行初始化
```c++
char * const a=&p; 
```
#### 常量指针
常量指针，不能修改指针指向的内存的内容，但是可以改变指针的指向
```c++
const char * a=&p;
char const * a=&p;
```