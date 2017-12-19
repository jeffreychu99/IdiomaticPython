# 2.8 元组

### 2.8.1 使用元组解包(unpack)数据

在Python中，可使用解包数据实现多重赋值。这类似于LISP中的desctructuring bind。

### 2.8.1.1 不好的风格

```python
list_from_comma_separated_value_file = ['dog', 'Fido', 10]
animal = list_from_comma_separated_value_file[0]
name = list_from_comma_separated_value_file[1]
age = list_from_comma_separated_value_file[2]
output = ('{name} the {animal} is {age} years old'.format(
    animal=animal, name=name, age=age))
```

#### 2.8.1.2 python的风格

```python
list_from_comma_separated_value_file = ['dog', 'Fido', 10]
(animal, name, age) = list_from_comma_separated_value_file
output = ('{name} the {animal} is {age} years old'.format(
    animal=animal, name=name, age=age))
```

### 2.8.2 在元组中使用_占位符表示要忽略的值

当将元组中内容顺序赋值到一些变量中时，很多时候并不是所有的数据都是需要的。与其创建令人困惑的废弃变量，不如使用_占位符告诉读者，该数据是没用的。

###2.8.2.1 不好的风格

```python
(name, age, temp, temp2) = get_user_info(user)
if age > 21:
    output = '{name} can drink!'.format(name=name)
# "Wait, where are temp and temp2 being used?"
```

#### 2.8.2.2 python的风格

```python
(name, age, _, _) = get_user_info(user)
if age > 21:
    output = '{name} can drink!'.format(name=name)
# "Clearly, only name and age are interesting"
```

