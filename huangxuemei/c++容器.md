## vector
连续的内存地址，如果插入的值大于当前最大内存，会寻找另外的更大的内存，将原来的内容复制过去，再删掉原来的部分，插入和删除元素是常数时间。
```c++
//初始化
vector<int> v(50);
//用v初始化v1
vector<int> v1(v);
vector<int> v1(v.begain(),v.end());
//初始化全为某个值的v2
vector<int> v2(10,4);
//初始化二维数组
vector<int> v3(4)
vector<int,vector<int>> v4(5,v3);

//常用方法
v.push_back(100);
v.pop_back();
int size = v.size();
bool empty = v.empty();
v.insert(v.end(),5,3);//在v的尾部插入五个为3的元素，只能在尾部插入
v.erase(v.begain(),v.begin()+2);//删除第一个和第二个元素，不包括v[2]
v.clear();
vector<int> :: iterator = v.begin();
vector<int> :: iterator = v.end();
```
## list
非连续的，链表实现，快速插入和删除，随机访问慢。
```c++
//初始化
list<int> lst1; 
list<int> lst2(3); 
list<int> lst3(3,2); 
list<int> lst4(lst2); //使用lst2初始化lst4
list<int> lst5(lst2.begin(),lst2.end());
 
//常用方法
lst1.assign(lst2.begin(),lst2.end()); //分配3个值为0的元素
lst1.push_back(10); 
lst1.pop_back();
lst1.clear(); //清空值
bool isEmpty1 = lst1.empty(); //判断为空
lst1.erase(lst1.begin(),lst1.end());  //删除元素
lst1.front(); //返回第一个元素的引用
lst1.back(); //返回最后一个元素的引用
lst1.insert(lst1.begin(), 3, 2);
lst1.remove(2); //相同的元素全部删除
//！！！重要
lst1.reverse(); //反转
lst1.size(); //含有元素个数
lst1.sort(); //排序
lst1.unique(); //删除相邻重复元素
```
## queue
队列，先进先出。
```c++
queue<int> q;
int i=q.back();返回最后一个元素
bool empty = q.empty();
int front=q.front()返回第一个元素
q.pop()删除第一个元素
q.push(2)在末尾加入一个元素
int size=q.size()返回队列中元素的个数
```

## priority_queue
## stack
栈，先进后出。
## map
键值对
```c++
map<int,int> m;
m.find(1)==m.end();//如果find的结果等于m的end()表示没找到该元素在m中
```
## set
在set中每个元素的值都唯一，而且系统能根据元素的值自动进行排序。
```c++
set<int> s;
s.insert(a);
for(set<int>::iteroter i=s.begin();i!=s.end();i++){
	cout<<*i<<endl;
}
```