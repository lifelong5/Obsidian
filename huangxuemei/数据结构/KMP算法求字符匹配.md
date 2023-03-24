```c++
#include <iostream>
#include <string>
#include <vector>
//首先是求next数组，next[j]=k，表示当T[i]!=P[j]的时候，j应该跳转到k位置继续进行比较，如果对于P字符串来说，j位置前面有相等或者重复的两部分，则j不用跳转到0位置，而是跳转到第二个重复部分的下一个位置继续进行比较，反之则需要跳转到0位置
//对于next[0]不用再进行跳转所以为-1
using namespace std;
vector<int> getNext(string str){
	vector<int> next;
	next.push_back(-1);
	//k的初始值为-1，因为j是从0开始的
	int k=-1;
	int j=0;
	while(j<str.length()-1){
		if(k==-1 || str[j]==str[k]){
			j++;
			k++;
			if(find(next.begin(),next.end(),j)!=str.end()){
				next[j]=k;
			}else{
				next.push_back(k);
			}
		}else{
			k=next[k];
		}
	}
	return next;
}
int main(){
	string t="ABCABCDH";
	string p="DH";
	int i=0;
	int j=0;
	//如果j是为-1表示在p的0位置，以及当两者匹配的时候，同时后移
	//如果两者到了不匹配的位置，j跳转到next[j]存储的位置
	while(i<t.length() && (j+1)<(p.length()+1)){
		if(j==-1 || t[i]==p[j]){
			i++;
			j++;
		}else{
			j=next[j];
		}
	}
	if(j==p.length()){
		return i-j;
	}else{
		return -1;
	}
}
```
[KMP算法详解-彻底清楚了(转载+部分原创) - sofu6 - 博客园](https://www.cnblogs.com/dusf/p/kmp.html)