# 3.2 格式化

### 3.2.1 使用大写字母来声明全局常量

为了区分从定义在模块级别中(或者单个文件中的全局变量)导入的常量名字，都使用大写字母。

#### 3.2.1.1 不好的风格

```python
seconds_in_a_day = 60 * 60 * 24
# ...
def display_uptime(uptime_in_seconds):
    percentage_run_time = (
        uptime_in_seconds/seconds_in_a_day) * 100
    # "Huh!? Where did seconds_in_a_day come from?"
    return 'The process was up {percent} percent of the day'.format(
        percent=int(percentage_run_time))
# ...
uptime_in_seconds = 60 * 60 * 24
display_uptime(uptime_in_seconds)
```

#### 3.2.1.2 Python的风格

```python
SECONDS_IN_A_DAY = 60 * 60 * 24
# ...
def display_uptime(uptime_in_seconds):
    percentage_run_time = (
        uptime_in_seconds/SECONDS_IN_A_DAY) * 100
    # "Clearly SECONDS_IN_A_DAY is a constant defined
    # elsewhere in this module."
    return 'The process was up {percent} percent of the day'.format(
        percent=int(percentage_run_time))
    # ...
uptime_in_seconds = 60 * 60 * 24
display_uptime(uptime_in_seconds)
```

### 3.2.2 避免在同一行上放置多条语句

尽管语言定义允许放置多条，没有理由地使用，使得代码很难读，无法较好表述语句。当同一行上出现像if, else 或 elif 多条语句时，情形变得更加困惑。

#### 3.2.2.1 不好的风格

```python
if this_is_bad_code: rewrite_code(); make_it_more_readable();
```

#### 3.2.2.2 Python的风格

```python
if this_is_bad_code:
    rewrite_code()
    make_it_more_readable()
```

### 3.2.3 依据PEP8规则调整代码风格

Python定义了一套代码风格规则，俗称PEP8。如果你浏览Python工程的commit信息时，你将会发现，里面散落对PEP8的引用。原因很简单：如果我们都同意通用的命名和格式约定，那么所有的Python代码对新手和经验丰富的开发者而言，可读性更好。PEP8也许是Python社区中习语最明显的例子。阅读PEP，需要为编辑器安装PEP8的样式检查插件（一般编辑器都有），用其它程序员都欣赏的风格编写代码。下面列举几个例子：

<table>
    <tr>
        <th>标识符类型</th>
        <th>格式</th>
        <th>示例</th>
    </tr>
    <tr>
        <td>类</td>
        <td>首字母大写的Camel风格</td>
        <td>class StringManipulator():</td>
    </tr>
    <tr>
        <td>变量</td>
        <td>单词之间使用_连接</td>
        <td>joined_by_underscore = True</td>
    </tr>
    <tr>
        <td>函数</td>
        <td>单词之间使用_连接</td>
        <td>def multi_word_name(words):</td>
    </tr>
    <tr>
        <td>常量</td>
        <td>所有字母大写</td>
        <td>SECRET_KEY = 42</td>
    </tr>
</table>

一般其它没有列出的遵循变量和函数命名习惯：单词之间使用下划线连接。



