# 3.4 导入

### 3.4.1 相比相对导入，最好使用绝对导入

有两种导入模块的风格：绝对导入和相对导入。绝对导入依据sys.path指定导入模块位置（像\<package\>.\<module\>.\<submodule\>）。

相对模块导入的模块相对于当前模块在文件系统中的位置。假设有模块package.sub_package.module，想要导入package.other_module,可以使用.风格的相对导入语法：from ..other_module import foo。单一.表示当前模块包含的包。每一个额外的.意味着包的父亲一级，一级一个点。请注意，相对导入必须使用from ... import ...风格。import foo通常会被认为是绝对导入。

相应地，使用绝对导入应该这样写：import package.other_module（有可能使用子句给模块命名一个短的名字）。

那么，为什么更倾向于使用绝对导入呢？相对导入会搞混模块的命名空间。from foo import bar语句将bar绑定到自己模块的命名空间下。对于那些阅读你代码的读者而言，不太清楚bar来自哪里，尤其是当应用在复杂函数或大模块中。然而，boo.bar，很清晰地知道bar在哪里定义。Python编程常见问题甚至可以这样说：永远不要使用相对包导入。

#### 3.4.1.1 不好的风格

```python
# My location is package.sub_package.module
# and I want to import package.other_module.
# The following should be avoided:
from ...package import other_module
```

#### 3.4.1.2 Python的风格

```python
# My location is package.sub_package.another_sub_package.module
# and I want to import package.other_module.
# Either of the following are acceptable:
import package.other_module
import package.other_module as other
```

### 3.4.2 不要使用from foo import * 导入模块中的内容

考虑之前的习惯，该规则就会很明显。在导入的时候(from foo import \*)，使用\*号很容易混乱命名空间。如果自己定义和包中定义的名称出现冲突，可能会导致问题。

不过，如果想要从foo包中导入大量的名字怎么办？很简单。利用括号来组合导入语句。你没有必要从一个模块中写10行导入语句，命名空间仍然相对比较干净。

更好的是，只需简单使用绝对导入。如果包/模块名称很长，用as从句命名一个短点的变量名。

#### 3.4.2.1 不好的风格

```python
from foo import *
```

#### 3.4.2.2 Python的风格

```python
from foo import (bar, baz, qux, quux, quuux)
# or even better
import foo
```

#### 3.4.3 使用标准的顺序排列导入语句

随着工程规模的增长（尤其是那些WEB框架），导入语句的数量也会变大。让所有的导入语句在文件的开头，使用标准的顺序给import语句排序，并坚持下去。不过实际的顺序并不是太重要，以下是Python编程常见问题推荐的顺序：

1. 标准库模块
2. 安装在site-packages的第三方库
3. 本工程中的本地模块

许多人选择按照（大致）按字母顺序排列导入语句。其他人会认为这是可笑的。 实际上，这并不重要。 重要的是你选择一个标准的顺序，并遵守它。

#### 3.4.3.1 不好风格

```python
import os.path
# Some function and class definitions,
# one of which uses os.path
# ....
import concurrent.futures
from flask import render_template
# Stuff using futures and Flask's render_template
# ....
from flask import (Flask, request, session, g,
    redirect, url_for, abort,
    render_template, flash, _app_ctx_stack)
import requests
# Code using flask and requests
# ....
if __name__ == '__main__':
    # Imports when imported as a module are not so
    # costly that they need to be relegated to inside
    # an 'if __name__ == '__main__'' block...
    import this_project.utilities.sentient_network as skynet
    import this_project.widgets
    import sys
```
#### 3.4.3.2 Python的风格

```python
Easy to see exactly what my dependencies are and where to
# make changes if a module or package name changes
import concurrent.futures
import os.path
import sys
from flask import (Flask, request, session, g,
    redirect, url_for, abort,
    render_template, flash, _app_ctx_stack)
import requests
import this_project.utilities.sentient_network as skynet
import this_project.widgets
```