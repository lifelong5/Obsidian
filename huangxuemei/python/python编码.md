#### 1、基础

![[image-20220913103745091.png]]

**编码不同，占用的字节就不同，不同编码之间不能相互识别，编码与解码必须一样**

#### 2、Unicode编码

在计算机内存中使用的是Unicode编码格式，当数据需要保存在硬盘或者需要网络传输的时候，就转换为非Unicode的编码格式。

![[image-20220913104226428.png]]

#### 3、编码转换

- encode：编码
- decode：解码

**其它类型不能直接转化bytes类型，需要先转换成str字符串类型然后在进行转换**

```python
s='中国'
# 编码
b=s.encode('utf-8') # 将s进行utf-8的编码
print(b,type(b)) # b'\xe4\xbd\xa0\xe5\xa5\xbd' <class 'bytes'>
# 解码
b=b'\xe4\xb8\xad\xe5\x9b\xbd'
b=b.decode('utf-8')
print(b.type(b)) # 中国 <class 'str'>
# 如果是非字符串的内容需要先转变为str
s=['我的',123]
s=str(s)
```

#### 4、编码格式转换

##### gbk与utf-8的转换

gbk格式的编码-gbk解码-得到Unicode-utf-8编码-utf-8的编码

```python
# utf-8 --> gbk
s='中国'
b=s.encode('utf-8')#编码
print(b,type(b))#得到utf-8编码的中国
#b'\xe4\xb8\xad\xe5\x9b\xbd'
g=b.decode('utf-8')#得到解码Unocode的中国
print(g)#中国
g=g.encode('gbk')#制定gbk编码
print(g)#b'\xd6\xd0\xb9\xfa'


# gbk --> utf-8
s='中国'
b=s.encode('gbk')#编码
print(b)#b'\xd6\xd0\xb9\xfa'
g=b.decode('gbk')
print(g)#中国
g=g.encode('utf-8')
print(g)#b'\xe4\xb8\xad\xe5\x9b\xbd'
```

