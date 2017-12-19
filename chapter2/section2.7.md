# 2.7 上下文管理

### 2.7.1 使用上下文管理器确保资源的合理管理

类似C++和D语言的RAII原则，上下文管理器(和with语句一起使用)可以让资源的管理更加安全和清晰。一个典型的例子就是文件IO操作。

看一下下面不好的代码。如果发生了异常会，怎么样？因为代码中没有捕获异常，异常会向上传递。代码中的退出点可能被跳过，导致没法关闭已经打开的文件。

标准库中有很多支持和使用上下文管理器的类。此外，通过定义__enter__和__exit__方法，用户自定义类也可以很容易地支持上下文管理器。函数可以通过contextlib模块，用上下文管理器来包装。

### 2.7.1.1 不好的风格

```python
file_handle = open(path_to_file, 'r')
for line in file_handle.readlines():
    if raise_exception(line):
        print('No! An Exception!')
```

#### 2.7.1.2 python的风格

```python
with open(path_to_file, 'r') as file_handle:
for line in file_handle:
    if raise_exception(line):
        print('No! An Exception!')
```
