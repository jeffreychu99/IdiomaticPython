# 2.5 集合

### 2.5.1 使用集合来消除可迭代容器中的重复项

字典或列表中有重复项很常见。大公司所有员工的姓氏中，同样的姓氏会不止一次地出现。如果用列表来处理唯一的姓氏，工作量将很大。集合三个方面的特性可以完美回答上述问题：

1. 集合仅包含唯一的元素
2. 集合中添加已存在的元素会被忽略
3. 可迭代容器中可以hash的元素都可以构建到集合中

继续上面的例子，有一个接受序列参数的display函数，可以以某一格式显示参数中的元素。从原始列表创建集合后，是否需要改变display函数？

答案是：否。假定我们的display函数实现是合理的，那么集合可以直接替换列表。这得益于集合事实上像列表一样，是可迭代的，可以在循环、列表推导中等使用。

2.5.1.1 不好的风格

```python
unique_surnames = []
for surname in employee_surnames:
    if surname not in unique_surnames:
        unique_surnames.append(surname)
def display(elements, output_format='html'):
    if output_format == 'std_out':
        for element in elements:
            print(element)
    elif output_format == 'html':
        as_html = '<ul>'
    for element in elements:
        as_html += '<li>{}</li>'.format(element)
        return as_html + '</ul>'
    else:
        raise RuntimeError('Unknown format {}'.format(output_format))
```

2.5.1.2 python的风格

```python
unique_surnames = set(employee_surnames)
def display(elements, output_format='html'):
    if output_format == 'std_out':
        for element in elements:
            print(element)
    elif output_format == 'html':
        as_html = '<ul>'
        for element in elements:
            as_html += '<li>{}</li>'.format(element)
            return as_html + '</ul>'
    else:
    raise RuntimeError('Unknown format {}'.format(output_format))
```

### 2.5.2 使用集合推导可以很简洁地生成集合

集合的推导是Python中相对比较新的概念，应此，常常被忽略。就像通过列表推导产生列表一样，集合也可以由集合推导产生。事实上，两种语法几乎是相同的，除了分界符。

#### 2.5.2.1 不好的风格

```python
users_first_names = set()
for user in users:
    users_first_names.add(user.first_name)
```

#### 2.5.2.2 python的风格

```python
users_first_names = {user.first_name for user in users}
```

### 2.5.3 理解和使用数学集合操作

集合是非常容易理解的数据结构。集合类似有索引没有值的字典，集合类实现了Iterable和Container接口。因此，集合可以在for循环和in语句中使用。

之前不了解集合数据类型的程序员，可能不能灵活运用。了解它们的关键是了解他们的数学起源。集合论是学习集合set的数学基础，了解基本的数学集合操作是发挥集合set能力的关键。

不用担心，无需深入理解和使用数学上的集合。仅需要记住简单的几个操作：

**并集**       集合A和B中的元素，或者说A和B中所有的元素（Python中写法：A | B）

**交集**       A和B中同时包含的元素（Python中写法：A & B）

**补集(差集)**  A中但是不再B中的元素（Python中写法：A - B）

注意：这里A和B之间的顺序是有影响的，A - B 不一定和 B - A相同。

**对称差集**   A或B中的元素，但是不能同时在A和B中（Python中写法：A ^ B）

当处理数据列表时，一个常见的任务就是找出同时出现在所有列表中的元素。任何时候，当需要基于序列之间的关系，从两个或多个序列中挑选元素时，请使用集合。

下面，探讨一些典型的例子：

#### 2.5.3.1 不好的风格

```python
def get_both_popular_and_active_users():
    # Assume the following two functions each return a
    # list of user names
    most_popular_users = get_list_of_most_popular_users()
    most_active_users = get_list_of_most_active_users()
    popular_and_active_users = []
    for user in most_active_users:
        if user in most_popular_users:
            popular_and_active_users.append(user)
    return popular_and_active_users
```

#### 2.5.3.2 python的风格

```python
def get_both_popular_and_active_users():
    # Assume the following two functions each return a
    # list of user names
    return(set(
        get_list_of_most_active_users()) & set(
            get_list_of_most_popular_users()))
```

