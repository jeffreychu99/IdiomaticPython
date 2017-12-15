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

### 1.2.3 
