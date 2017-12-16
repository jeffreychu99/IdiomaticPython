# 2.3 字符串

### 2.3.1 倾向使用format函数构造字符串

有三种方式格式化字符串（就是创建一个由硬编码字符串和字符串变量混合而成的字符串）。最容易不过不好的方式是通过+运算符连接静态字符串和变量。使用“老式”的字符串格式化方法相对好一点。它就其它语言的printf一样，使用格式字符串和%运算符来填充值。格式化字符串的最清晰和最习惯的方式是使用format函数。就像老式的格式化一样，它利用了格式字符串并用值来替换其中的占位符。两者相似之处仅此而言。通过format函数，代码可以使用具名占位符、访问其中的属性、控制填充空白和字符串宽度，以及其它内容等。format函数使得字符串格式化更加清晰和简洁。

#### 2.3.1.1 不好的风格

```python
def get_formatted_user_info_worst(user):
    # Tedious to type and prone to conversion errors
    return 'Name: ' + user.name + ', Age: ' + \
            str(user.age) + ', Sex: ' + user.sex
def get_formatted_user_info_slightly_better(user):
    # No visible connection between the format string placeholders
    # and values to use. Also, why do I have to know the type?
    # Don't these types all have __str__ functions?
    return 'Name: %s, Age: %i, Sex: %c' % (
            user.name, user.age, user.sex)
```

#### 2.3.1.2 python的风格

```python
def get_formatted_user_info(user):
    # Clear and concise. At a glance I can tell exactly what
    # the output should be. Note: this string could be returned
    # directly, but the string itself is too long to fit on the
    # page.
    output = 'Name: {user.name}, Age: {user.age}'
        ', Sex: {user.sex}'.format(user=user)
    return output
```

### 2.3.2 使用.join函数将列表元素转换为单一字符串

这种方式更快，占用更少内存，很多地方都在使用。请注意，两个引号代表想要连接成字符串的列表元素之间的分隔符。''表示连接的列表元素之间没有任何字符内容。

#### 2.3.2.1 不好的风格

```python
result_list = ['True', 'False', 'File not found']
result_string = ''
for result in result_list:
    result_string += result
```
    
#### 2.3.2.2 python的风格

```python
result_list = ['True', 'False', 'File not found']
result_string = ''.join(result_list)
```

### 2.3.3 使用链式的字符串函数是一些列的字符串转换更加清晰

当在一些数据上应用一些简单的数据变换时，单一表达式的链式调用相比那些通过临时变量进行一步步转换的方式更加清晰。不过，太多的链接可能使得代码难以掌控。一个比较好的规则是函数之间的连接不超过三个。

#### 2.3.3.1 不好的风格

```python
book_info = ' The Three Musketeers: Alexandre Dumas'
formatted_book_info = book_info.strip()
formatted_book_info = formatted_book_info.upper()
formatted_book_info = formatted_book_info.replace(':', ' by')
```

#### 2.3.3.2 python的风格

```python
book_info = ' The Three Musketeers: Alexandre Dumas'
formatted_book_info = book_info.strip().upper().replace(':', ' by')
```

