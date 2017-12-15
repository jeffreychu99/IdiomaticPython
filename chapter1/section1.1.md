## 1.1 if语句

### 1.1.1 避免直接和True、False或者None进行比较

对于任意对象，内建还是用户定义的，本身都有真假的判断。当判断条件是否为真时，主要依赖于对象在条件语句中的真假性。真假性判断是非常清晰的。所有的下列条件都判断为假：

* None
* False
* 数值类型的0
* 空的序列
* 空的字典
* 当调用对象的__len__或__nonzero__方法时，返回值是0或False

其他的所有情况都判断为真(大部分情况是隐式为真)。最后一个通过检查__len__或__nonzero__方法返回值的判断条件，允许自定义的类如何决定对象的真假。

应该像Python中的if语句一样，在判断语句中隐式地利用真假。譬如下面判断变量foo是否为真的语句

```python
if foo == True: 
```

可以简单地写成

```python
if foo:
```

写成下面的原因有很多。最明显的就是当代码发生变化时，譬如foo从True或False变成int时，if语句仍然可以工作。不过深层次而言，判断真假是基于对象间的equality和identity。使用==判断两个对象是否包含同样的值(具体由_eq属性定义)时。使用is判断两个对象是否同一对象(应用相同)。

注意：虽然有些情况下条件成立，就像比较对象的之间的相等性一样，这些是特殊情况，不要依赖。

因此，避免直接和False、Node以及像[], {}, ()这样的空序列比较真假。如果my_list列表类型为空，直接调用 

```python
if my_list:
```

 结果就是假的。

有些时候，直接和None比较是必须的，虽然不是推荐的方式。函数中的某一个形参默认值为None时，而实际实参传递非空时，必须要将形参和None进行比较：

```python
def insert_value(value, position=None):
    """Inserts a value into my container, optionally at thespecified position"""
    if position is not None:
        ...
```

如果写成

```python
if position:
```

问题出在哪里？如果想要在位置0插值，函数没有任何赋值操作，因为当position为0时，判断为假。注意和None比较时，应该总是使用is或is not，而不是==（参考PEP8）.

#### 1.1.1.1 不好的风格

```python
def number_of_evil_robots_attacking():
    return 10
def should_raise_shields():
    # "We only raise Shields when one or more giant robots attack,
    # so I can just return that value..."
    return number_of_evil_robots_attacking()
if should_raise_shields() == True:
    raise_shields()
    print('Shields raised')
else:
    print('Safe! No giant robots attacking')
```

#### 1.1.1.2 python的风格

```python
def number_of_evil_robots_attacking():
    return 10
def should_raise_shields():
    # "We only raise Shields when one or more giant robots attack,
    # so I can just return that value..."
    return number_of_evil_robots_attacking()
if should_raise_shields():
    raise_shields()
    print('Shields raised')
else:
    print('Safe! No giant robots attacking')
```

### 1.1.2 避免在复合条件语句中重复变量名称

当检测变量是否对应一些列值时，重复使用变量进行数值比较是不必要的。使用迭代的方式可以让代码更加清晰，并且可读性更好。

#### 1.1.2.1 不好的风格

```python
is_generic_name = False
name = 'Tom'
if name == 'Tom' or name == 'Dick' or name == 'Harry':
    is_generic_name = True
```

#### 1.1.2.2 python的风格

```python
name = 'Tom'
is_generic_name = name in ('Tom', 'Dick', 'Harry')
```

### 1.1.3 避免把条件分支代码和分号放到同一行

使用缩进来表示范围(python就是这么做的)可以更容易知道代码是否是条件语句的一部分。if, elif和else语句应单独写成一行，冒号后面不应再写代码。

#### 1.1.3.1 不好的风格

```python
name = 'Jeff'
address = 'New York, NY'
if name: print(name)
print(address)
```

#### 1.1.3.2 python的风格

```python
name = 'Jeff'
address = 'New York, NY'
if name:
    print(name)
print(address)
```