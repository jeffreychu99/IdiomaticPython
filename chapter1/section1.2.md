# 1.2 for循环

### 1.2.1 在循环中使用enumerate函数而非创建index索引变量

其它编程语言的程序员经常显式声明一个索引变量，用于跟踪循环中的容器。譬如，在C++中：

```C++
for (int i=0; i < container.size(); ++i)
{
    // Do stuff
}
```

在python中，有内建的enumerate函数用于处理这种情况。

#### 1.2.1.1 不好的风格

```python
my_container = ['Larry', 'Moe', 'Curly']
index = 0
for element in my_container:
    print ('{} {}'.format(index, element))
    index += 1
```

#### 1.2.1.2 python的风格

```python
my_container = ['Larry', 'Moe', 'Curly']
for index, element in enumerate(my_container):
    print ('{} {}'.format(index, element))
```

### 1.2.2 使用in关键字迭代可迭代的对象

使用缺少for_each风格语言的程序员通过索引访问元素迭代容器。Python的in关键字很优雅地处理了这个情况。

#### 1.2.2.1 不好的风格

```python
my_list = ['Larry', 'Moe', 'Curly']
index = 0
while index < len(my_list):
    print (my_list[index])
    index += 1
```

#### 1.2.2.2 python的风格

```python
my_list = ['Larry', 'Moe', 'Curly']
for element in my_list:
    print (element)
```

### 1.2.3 for循环结束后使用else执行代码

很少有人知道for循环语句后面可以跟随一段else语句。当迭代器执行结束后，else语句才会被执行，除非循环因为break语句提前结束了。break语句允许在循环内检查某些条件，当条件满足时，结束循环，否则，继续执行直到循环结束(无论如何最终else语句还是需要执行的)。这样可以避免通过在循环内增加标志位来检查条件是否满足。

在下面的情况下，代码用于检测用户注册的邮件地址是否合法（一个用户可以注册多个地址）。python风格的代码比较简洁，得益于不用处理has_malformed_email_address标志。更何况，即使一个程序员不熟悉for .. else语法，他们也可以很容易地看懂代码。

#### 1.2.3.1 

```python
for user in get_all_users():
    has_malformed_email_address = False
    print ('Checking {}'.format(user))
    for email_address in user.get_all_email_addresses():
        if email_is_malformed(email_address):
            has_malformed_email_address = True
            print ('Has a malformed email address!')
            break
    if not has_malformed_email_address:
        print ('All email addresses are valid!')
``` 

#### 1.2.3.2

```python
for user in get_all_users():
    print ('Checking {}'.format(user))
    for email_address in user.get_all_email_addresses():
        if email_is_malformed(email_address):
            print ('Has a malformed email address!')
            break
    else:
        print ('All email addresses are valid!')
```
