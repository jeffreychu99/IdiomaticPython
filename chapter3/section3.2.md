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
