# 2.1 列表

### 2.1.1 使用列表推导创建基于现有列表的新列表

机智地使用列表推导，可以使得基于现有数据构建列表的代码很清晰。尤其当进行一些条件检测和转换时。

使用列表推导（或者使用生成器表达式）通常还会带来性能上的提升，这是因为cPython的解释器的优化。

####2.1.1.1 不好的风格

```python
some_other_list = range(10)
some_list = list()
for element in some_other_list:
    if is_prime(element):
        some_list.append(element + 5)
```

#### 2.1.1.2 python的风格

```python
some_other_list = range(10)
some_list = [element + 5
                for element in some_other_list
                if is_prime(element)]
```

#### 2.1.2 使用*操作符来表示列表的其余部分

通常情况下，尤其是在处理函数参数时，这是很有用的，可以提取列表头部（或尾部）的一些元素，然后剩下的后面使用。Python2没有简单的方法完成这一点，可以通过切片来模拟。Python3允许在左边使用*运算符用来表示列表的其余部分。

#### 2.1.2.1 不好的风格

```python
some_list = ['a', 'b', 'c', 'd', 'e']
(first, second, rest) = some_list[0], some_list[1], some_list[2:]
print(rest)
(first, middle, last) = some_list[0], some_list[1:-1], some_list[-1]
print(middle)
(head, penultimate, last) = some_list[:-2], some_list[-2], some_list[-1]
print(head)
```

#### 2.1.2.2 python的风格

```python
some_list = ['a', 'b', 'c', 'd', 'e']
(first, second, *rest) = some_list
print(rest)
(first, *middle, last) = some_list
print(middle)
(*head, penultimate, last) = some_list
print(head)
```

