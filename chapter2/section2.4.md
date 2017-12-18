# 2.4 类

### 2.4.1 在函数名和变量名中使用下划线来帮助标记为私有数据

Python类中的所有属性，不论是数据还是函数，本质上都是公有的。用户可以在类定义完成后自动地添加属性。此外，如果这个类是为了继承而来的，子类可能在不知不觉中更改基类的属性。最后，告知用户关于类的某些部分逻辑上公开的（不会变得向后不兼容），而其它属性是纯粹的内部实现，不应该被使用本类的客户端代码直接使用，关于这一点是非常有用的。

一些被广泛遵循的约定已经出现，使得作者的意图更加明确，有助于避免无意的命名冲突。于这两种用法，虽然普遍认为是公共约定，但是事实上在使用中会使解释器也产生不同的行为。

首先，用单下划线开始命名的表明是受保护的属性，用户不应该直接访问。其次，用两个连续地下划线开头的属性，表明是私有的，即使子类都不应该访问。当然了，这些更多的是约定，并不能真正阻止用户访问到这些私有的属性，但这些约定在整个Python社区中被广泛使用，不太可能遇到那些故意不遵循的开发者。从某种角度上来说这也是Python里用一种办法完成一件事情的哲学体现。

之前，本文提到过单下划线和双下划线不仅仅是使用习惯问题，一些开发者意识到这种写法是有实际作用的。以单下划线开头的变量在import *时不会被导入。以双下划线开头的变量则会触发Python中name mangling，如果Foo是一个类，那么Foo中定义的\_\_bar()方法将会被展开成_classname__attributename.

#### 2.4.1.1 不好的风格

```python
class Foo():
    def __init__(self):
        self.id = 8
        self.value = self.get_value()
    def get_value(self):
        pass
    def should_destroy_earth(self):
        return self.id == 42

class Baz(Foo):
    def get_value(self, some_new_parameter):
        """Since 'get_value' is called from the base class's
        __init__ method and the base class definition doesn't
        take a parameter, trying to create a Baz instance will
        fail.
        """
        pass
class Qux(Foo):
    """We aren't aware of Foo's internals, and we innocently
    create an instance attribute named 'id' and set it to 42.
    This overwrites Foo's id attribute and we inadvertently
    blow up the earth.
    """
    def __init__(self):
        super(Qux, self).__init__()
        self.id = 42
        # No relation to Foo's id, purely coincidental
q = Qux()
b = Baz() # Raises 'TypeError'
q.should_destroy_earth() # returns True
q.id == 42 # returns True
```

#### 2.4.1.2 python的风格

```python
class Foo():
    def __init__(self):
        """Since 'id' is of vital importance to us, we don't
        want a derived class accidentally overwriting it. We'll
        prepend with double underscores to introduce name
        mangling.
        """
        self.__id = 8
        self.value = self.__get_value() # Our 'private copy'
    def get_value(self):
        pass

    def should_destroy_earth(self):
        return self.__id == 42
    # Here, we're storing an 'private copy' of get_value,
    # and assigning it to '__get_value'. Even if a derived
    # class overrides get_value is a way incompatible with
    # ours, we're fine
    __get_value = get_value
class Baz(Foo):
    def get_value(self, some_new_parameter):
            pass
class Qux(Foo):
    def __init__(self):
        """Now when we set 'id' to 42, it's not the same 'id'
        that 'should_destroy_earth' is concerned with. In fact,
        if you inspect a Qux object, you'll find it doesn't
        have an __id attribute. So we can't mistakenly change
        Foo's __id attribute even if we wanted to.
        """
        self.id = 42
        # No relation to Foo's id, purely coincidental
        super(Qux, self).__init__()
q = Qux()
b = Baz() # Works fine now
q.should_destroy_earth() # returns False
q.id == 42 # returns True
with pytest.raises(AttributeError):
    getattr(q, '__id')        

```

### 2.4.2 在类中定义__str__方法用于可读性显示

当定义可能被print()方法使用的类时，默认的Python呈现方法帮助不大。通过定义__str__方法，可以控制类实例打印呈现出来的内容。

#### 2.4.2.1 不好的风格

```python
class Point():
    def __init__(self, x, y):
        self.x = x
        self.y = y
p = Point(1, 2)
print (p)
# Prints '<__main__.Point object at 0x91ebd0>'

2.4.2.2 python的风格

```python
class Point():
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def __str__(self):
        return '{0}, {1}'.format(self.x, self.y)
p = Point(1, 2)
print (p)
# Prints '1, 2'
