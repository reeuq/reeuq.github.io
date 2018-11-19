计算机最早是只有英文ASCII码，但世界上除了英语，还有很多其他语言，因此只有ASCII码编码显然不适合这种情况。

于是后来在中国、日本等其他国家都有了自己的一套编码，但是这样就出现问题了，不同国家之间数据传输，就会出现乱码。

为了让全世界都可以使用计算机，于是有了Unicode编码方式，俗称万国码，可以存储好几万个字符。（unicode专题TODO）

**因此计算机内存中，对字符串的编码使用的都是Unicode，作为中间者。**

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
print(type(s2))
```



1. **在编译时**，python文件默认以ASCII码进行解码，因此如果文件中有中文就会报错：`Non-ASCII character '\xe9' ` ，表示出现了超出ASCII码（0-127）以外的字节，因此需要在文件第一行添加：`# _*_ coding:utf8 _*_` ，让编译器以指定的编码进行解码然后编译。

2. **在运行时**，变量`s` 的类型是`<type 'str'>` ，可以理解为保存了“中文”以utf8编码的二进制数据。`s.encode('utf8')`命令实际上过程是这样的：

   ```
   s.decode(defaultencoding).encode('utf8')
   
   <type 'str'>  ——以defaultencoding解码——>   <type 'unicode'>	——以encode方法指定的字符集编码——>   <type 'str'>
   ```

   defaultencoding在python2中默认是ASCII，而s是以utf8编码的，因此在str—>unicode的时候就会出现`UnicodeDecodeError`

3. **解决办法**有2种：

   1. 显式进行解码`s.decode('utf8').encode('utf8')`

   2. 修改系统默认字符集   

      ```python
      import sys
      reload(sys)
      sys.setdefaultencoding('utf8')
      ```


#### Python3 中的编码过程

```python
s = "中文"
print(type(s))
print(s)
s2=s.encode("utf8")
print(s2)
# print(type(s2))
# s3=s2.encode('utf8')
# print(type(s3))
# print(s2)
# print(s2.decode('utf8'))

```

python3的改进：

1. Python3在编译时，文件默认就是以utf8进行编码然后编译的

2. python3中字符串只有`<class str>` ，对应的就是unicode

《class byte》 type str





#### python中的unicode

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

- 编码为指定字符集

encode()函数根据括号内的编码方式，把str类型的字符串转换为bytes字符串，字符对应的若干十六进制数，根据编码方式决定。



```python
# python2.7
import sys
# Python2.5初始化后会删除sys.setdefaultencoding这个方法，因此需要重新载入
reload(sys)
sys.setdefaultencoding('utf-8')

msg='中文'   # 此时msg被编码为utf-8,而不是ascii
type(msg)   # <type 'str'>
len(msg) 	# 6

umsg=unicode(msg) # u'\u4e2d\u6587'
type(umsg)  # <type 'unicode'>
len(umsg)   # 2
```



#### python2与3的区别

在python2里面，u表示unicode string，类型是unicode, 没有u表示byte string，类型是 str。

```python
s1 = '我爱你'
print(type(s1)) # <type 'str'>
print(len(s1))  # 9

s2 = u'我爱你'
print(type(s2)) # <type 'unicode'>
print(len(s2))  # 3

print(s1 == s2) # False (报错)
print(s1 is s2) # False
```

在python3里面，所有字符串都是unicode string, u前缀没有特殊含义了。**去除了`<type 'unicode'>` ，全部都变成了`<class 'str'>`**

```python
s1 = '我爱你'
print(type(s1)) # <class 'str'>
print(len(s1))  # 3

s2 = u'我爱你'
print(type(s2)) # <class 'str'>
print(len(s2))  # 3

print(s1 == s2) # True
print(s1 is s2) # True
```

