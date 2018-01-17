# 3.3 可执行脚本

### 3.3.1 在脚本中使用sys.exit函数返回恰当的错误码

Python脚本应该是一个好的shell公民。在if \_\_name\_\_ == '\_\_main\_\_'后面执行一段代码，不返回任何结果，看起来挺神奇的。要避免这种做法。

在包含代码的运行脚本中创建一个主函数。如果有错误，在主函数中使用sys.exit返回错误码，否则，返回0。if \_\_name\_\_ =='\_\_main\_\_'下的唯一代码语句应该调用sys.exit作为主函数的返回值参数。

通过这样做，允许脚本在Unix管道中被使用，进行监控，失败的时候不需要自定义规则，可以被其他程序安全地调用。

#### 3.3.1.1 不好的风格

```python
if __name__ == '__main__':
    import sys
    # What happens if no argument is passed on the
    # command line?
    if len(sys.argv) > 1:
        argument = sys.argv[1]
        result = do_stuff(argument)
        # Again, what if this is False? How would other
        # programs know?
        if result:
            do_stuff_with_result(result)
```

#### 3.3.1.2 python的风格

```python
def main():
    import sys
    if len(sys.argv) < 2:
        # Calling sys.exit with a string automatically
        # prints the string to stderr and exits with
        # a value of '1' (error)
        sys.exit('You forgot to pass an argument')
    argument = sys.argv[1]
    result = do_stuff(argument)
    if not result:
        sys.exit(1)
        # We can also exit with just the return code
    do_stuff_with_result(result)
    # Optional, since the return value without this return
    # statment would default to None, which sys.exit treats
    # as 'exit with 0'
    return 0
# The three lines below are the canonical script entry
# point lines. You'll see them often in other Python scripts
if __name__ == '__main__':
    sys.exit(main())
```

### 3.3.2 在文件中使用if __name__ == '__main__'使得文件即可以被导入也可以直接运行

不像某些语言中的main()函数，Python没有内建的主入口点。相反，Python依赖加载的文件来执行语句。如果想要文件既可以作为模块导入，也可以作为一个独立的脚本，请使用 if \_\_name\_\_== '\_\_main\_\_'。

#### 3.3.2.1 不好的风格

```python
import sys
import os
FIRST_NUMBER = float(sys.argv[1])
SECOND_NUMBER = float(sys.argv[2])
def divide(a, b):
    return a/b
# I can't import this file (for the super
# useful 'divide' function) without the following
# code being executed.
if SECOND_NUMBER != 0:
    print(divide(FIRST_NUMBER, SECOND_NUMBER))
```

#### 3.3.2.2 Python的风格

```python
import sys
import os
def divide(a, b):
return a/b
# 仅当脚本直接运行的时候执行，作为模块导入的时候不执行
if __name__ == '__main__':
    first_number = float(sys.argv[1])
    second_number = float(sys.argv[2])
    if second_number != 0:
        print(divide(first_number, second_number))    
```