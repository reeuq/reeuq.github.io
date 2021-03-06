---
title: python编码
date: 2018-11-19 14:37:28
tags: [python,encode]
categories: python疑难杂症
---
计算机最早是只有英文ASCII码，但世界上除了英语，还有很多其他语言，因此只有ASCII码编码显然不适合这种情况。

于是后来在中国、日本等其他国家都有了自己的一套编码，但是这样就出现问题了，不同国家之间数据传输，就会出现乱码。

为了让全世界都可以使用计算机，于是有了Unicode编码方式，俗称万国码，可以存储好几万个字符。（unicode专题TODO）

**因此计算机内存中，对字符串的编码使用的都是Unicode，作为中间者。**

<!-- more -->

但是又有新的问题出现了，**Unicode是定长编码，不适合存储**，非常浪费存储空间，这里又有了一种新的编码方式：utf-8。**utf-8采用的是不定长编码**，大大节约了存储空间。在数据存储和传输方面非常方便。

当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码(或者其他编码)。

{% asset_img 1.png %}

{% asset_img 2.png %}

![](./python编码/1.png)

***

![](./python编码/2.png)



#### pyhton2 中的编码过程



```python
# _*_ coding:utf8 _*_
s = "中文"
print(type(s)) # <type 'str'>

s2=s.encode('utf8') # unicode解码错误，无法以ASCII解码字节 0xe4，超出了range（128）：UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position 0: ordinal not in range(128)

s2 = u'中文'  # <type 'unicode'>
s3=s2.encode('utf8')
print(type(s3)) # <type 'str'>
```

1. **在运行前**，python解释器默认以ASCII码解码文件，因此如果文件中有中文就会报错：`Non-ASCII character '\xe9' ` ，表示出现了超出ASCII码（0-127）以外的字节，因此需要在文件第一行添加：`# _*_ coding:utf8 _*_` ，让编译器以指定的字符集进行解码然后编译。

2. **在运行时**，Python2 中字符串有2种类型：`<type 'str'>` 和`<type 'unicode'>`，默认是`str`类型 。变量`s` 的类型是`<type 'str'>` ，可以理解为保存了“中文”以utf8编码的二进制数据。`s.encode('utf8')`命令实际上过程是这样的：

   ```python
   s.decode(defaultencoding).encode('utf8')
   
   # 类型的转变过程
   <type 'str'>  ——以defaultencoding解码——>   <type 'unicode'>	——以encode方法指定的字符集编码——>   <type 'str'>
   ```

   ```python
   import sys
   print(sys.getdefaultencoding()) # ascii
   ```

   `defaultencoding`在python2中默认是ASCII，而`s`是以utf8编码的，因此在str—>unicode的时候就会出现`UnicodeDecodeError`

3. **解决办法**有2种：

   1. 显式进行解码`s.decode('utf8').encode('utf8')`

   2. 修改系统默认字符集

      ```python
      import sys
      reload(sys)# Python2.5初始化后会删除sys.setdefaultencoding这个方法，因此需要重新载入
      print(sys.getdefaultencoding()) # ascii
      sys.setdefaultencoding('utf-8')
      print(sys.getdefaultencoding()) # utf-8
      ```

##### u前缀

在python2里面，`u`表示unicode string，类型是unicode, 没有`u`表示byte string，类型是 str。

```python
s1 = '中文'	   # '\xe4\xb8\xad\xe6\x96\x87'
print(type(s1)) # <type 'str'>
print(len(s1))  # 6

s2 = u'中文'     # u'\u4e2d\u6587'
print(type(s2)) # <type 'unicode'>
print(len(s2))  # 2

print(s1 == s2) # False (报错)
print(s1 is s2) # False

s3=s.encode('utf8')
print(type(s3))
```

##### 字符串长度问题

```python
# python2.7
import sys
reload(sys)
sys.setdefaultencoding('utf-8')

msg='中文'   # '\xe4\xb8\xad\xe6\x96\x87'此时msg被编码为utf-8,而不是ascii ,
type(msg)   # <type 'str'>
len(msg) 	# 6 因为utf8每个汉字占3个字节

umsg=unicode(msg) # u'\u4e2d\u6587'
type(umsg)  # <type 'unicode'>
len(umsg)   # 2
```



#### Python3 中的编码过程

```python
s1 = '中文'	   # '中文'
print(type(s1)) # <class 'str'>
print(len(s1))  # 3

s2 = u'中文'     # '中文' 与s1没有区别
print(type(s2)) # <class 'str'>
print(len(s2))  # 3

print(s1 == s2) # True
print(s1 is s2) # True

s3=s1.encode("utf8")
print(type(s3))  #<class 'bytes'>
print(s3) # b'\xe4\xb8\xad\xe6\x96\x87'

s4=s3.decode("utf8") 
print(type(s4)) # <class 'str'>
print(s4) # 中文
```

python3的改进：

1. Python3在编译时，文件默认就是以`utf-8`进行解码然后编译

2. python3中**所有字符串都是以unicode格式(\uXXXX)保存在内存中**，**`u`前缀没有特殊含义了**。只有`<class str>` 类型，对应的就是python2中的 `<type 'unicode'>` 。而`<class byte>`  类似于python2的`<type 'str'>` 。

3. `encode`函数根据参数指定的编码方式，把str类型的字符串转换为bytes类型。而在python3中字符串没有`decode`函数，`byte`类型才有。

4. 在python3中，我们将不能直接看到unicode字节串，它会被显示为中文的“中文”；因为python3默认使用unicode编码，**unicode字节串将被直接处理为中文显示出来。**


#### 获取字符的unicode

- 根据unicode获取字符

```python
# chr参数支持10进制、16进制
chr(0x4e2d)  # '中' 
chr(20013)   # '中'
```

- 根据字符获取unicode码

```python
# 返回值：10进制的unicode码
ord("中")  # 20013
```



#### Python2中的print过程

Python2.7中调用print打印`var`变量时，操作系统会对`var`做一定的字符处理：

- 如果`var`是str类型的变量，则直接将`var`变量交付给终端进行显示；
- 如果`var`变量是unicode类型，则操作系统首先将var编码成str类型的对象（编码格式取决于stdout的编码格式`print(sys.stdout.encoding)`），然后再交由终端进行显示。
- 在终端显示时，如果str类型的**变量的编码方式**和**终端设置的编码方式**不一致，很可能会出现乱码问题。



#### Python2 与3 读取文件的编码问题

`codecs`会按照指定的字符集解码文件，然后将字符串转为`<type 'unicode'>` 

`open`读取文件后的字符串是`<type 'str'>` 类型，而且a.txt是以utf8编码保存的，与‘分’是同一种编码（`# _*_ coding:utf8 _*_`），因此可以直接split

a.txt中的内容是：中文分国家

```python
# _*_ coding:utf8 _*_
import codecs

with codecs.open('a.txt','r','utf-8') as f:
    line=f.read()
    print(type(line)) # <type 'unicode'>
    ss=line.split('分'.decode('utf8'))
    # 相当于下面2行
    # c='分'  # <type 'str'>  
    # ss=line.split(c.decode('utf8')) #因为line是unicode，而c是str，因此必须进行decode
    for s in ss:
        print(s)

with open('a.txt','r') as f:
    line2=f.readline()
    print(type(line2)) #<type 'str'>
    ss=line2.split('分')
    for s in ss:
        print(s)
```

python3中都是unicode存储字符串，因此没有上面的问题

```python
# python3
import codecs

with codecs.open('a.txt','r','utf-8') as f:
    line=f.read()
    print(type(line)) # <class 'str'>
    ss=line.split('分')
    for s in ss:
        print(s)

with open('a.txt','r',encoding='utf-8') as f:
    line2=f.readline()
    print(type(line2)) # <class 'str'>
    ss=line2.split('分')
    for s in ss:
        print(s)
```