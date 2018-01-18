# 4.2 再提模块

### 4.2.1 学习itertools模块中的内容

如果经常访问像StackOverflow这些网站，你会注意到诸如“为什么Python没有以下明显有用的库函数”这类问题的答案时，几乎总是引用itertools模块。itertools提供的稳定性程序功能应该被视为基础建筑模块。更重要的是，itertools的文档有一个“食谱”提供了部分常用功能编码的优雅实现，所有这些都是使用itertools模块创建的。因为某些原因，很少有Python开发者知道“食谱”部分，实际上，itertools确实这么做的（Python文档中常常有这类宝典）。编写优雅代码的一部分内容是你要熟悉什么时候重零开始编写代码。

### 4.2.2 使用os.path模块处理目录路径

当编写一个简单的命令行脚本时，Python新手经常操作字符串处理文件路径。Python有完整的模块专门用于处理路径名称：os.path。使用os.path减少通用错误的风险，让代码可移植，更易懂。

#### 4.2.2.1 不好的风格

```python
from datetime import date
import os
filename_to_archive = 'test.txt'
new_filename = 'test.bak'
target_directory = './archives'
today = date.today()
os.mkdir('./archives/' + str(today))
os.rename(filename_to_archive, target_directory + '/' + str(today) + '/' + new_filename))
```

#### 4.2.2.2 优雅的风格

```python
from datetime import date
import os
current_directory = os.getcwd()
filename_to_archive = 'test.txt'
new_filename = os.path.splitext(filename_to_archive)[0] + '.bak'
target_directory = os.path.join(current_directory, 'archives')
today = date.today()
new_path = os.path.join(target_directory, str(today))
    if (os.path.isdir(target_directory)):
        if not os.path.exists(new_path):
            os.mkdir(new_path)
    os.rename(
        os.path.join(current_directory, filename_to_archive),
        os.path.join(new_path, new_filename))
```
