## 1.3 函数

### 1.3.1 避免使用'', []和{}作为函数参数的默认值

虽然在python教程中明确提到了这一点，很多人表示诧异，即使有经验的开发人员。简而言之，函数参数的默认值首选None，而非[]。下面是Python教程对这个问题的处理：

#### 1.3.1.1 不好的风格

```python
# The default value [of a function] is evaluated only once.
# This makes a difference when the default is a mutable object
# such as a list, dictionary, or instances of most classes. For
# example, the following function accumulates the arguments
# passed to it on subsequent calls.
def f(a, L=[]):
    L.append(a)
    return L
print(f(1))
print(f(2))
print(f(3))
# This will print
#
# [1]
# [1, 2]
# [1, 2, 3]
```

#### 1.3.1.2 python的风格

```python
# If you don't want the default to be shared between subsequent
# calls, you can write the function like this instead:
def f(a, L=None):
    if L is None:
        L = []
    L.append(a)
    return L
print(f(1))
print(f(2))
print(f(3))
# This will print
# [1]
# [2]
# [3]
```

### 1.3.2 使用*args和**kwargs接受任意多的参数

通常情况下，函数需要接受任意的位置参数列表或关键字参数，使用它们中的一部分，并将其余的部分传递给其他函数。 使用*args和**kwargs作为参数允许函数接受任意的位置和关键字参数列表。在维护API的向后兼容性时，这个习性也很有用。如果函数接受任意参数，那么可以在新版本中自由地添加新的参数而不会破会现有使用较少参数的代码。只要一切文档记录正确，函数的实际参数是什么并没有多少影响。

####1.3.2.1 不好的风格

```python
def make_api_call(foo, bar, baz):
    if baz in ('Unicorn', 'Oven', 'New York'):
        return foo(bar)
    else:
        return bar(foo)
# I need to add another parameter to `make_api_call`
# without breaking everyone's existing code.
# I have two options...
def so_many_options():
    # I can tack on new parameters, but only if I make
    # all of them optional...
    def make_api_call(foo, bar, baz, qux=None, foo_polarity=None,
                baz_coefficient=None, quux_capacitor=None,
                bar_has_hopped=None, true=None, false=None,
                file_not_found=None):
        # ... and so on ad infinitum
        return file_not_found
def version_graveyard():
    # ... or I can create a new function each time the signature
    # changes.
    def make_api_call_v2(foo, bar, baz, qux):
        return make_api_call(foo, bar, baz) - qux
    def make_api_call_v3(foo, bar, baz, qux, foo_polarity):
        if foo_polarity != 'reversed':
            return make_api_call_v2(foo, bar, baz, qux)
        return None

def make_api_call_v4(
        foo, bar, baz, qux, foo_polarity, baz_coefficient):
    return make_api_call_v3(
        foo, bar, baz, qux, foo_polarity) * baz_coefficient
def make_api_call_v5(
        foo, bar, baz, qux, foo_polarity,
        baz_coefficient, quux_capacitor):
    # I don't need 'foo', 'bar', or 'baz' anymore, but I have to
    # keep supporting them...
    return baz_coefficient * quux_capacitor
def make_api_call_v6(
        foo, bar, baz, qux, foo_polarity, baz_coefficient,
        quux_capacitor, bar_has_hopped):
    if bar_has_hopped:
        baz_coefficient *= -1
    return make_api_call_v5(foo, bar, baz, qux,
                    foo_polarity, baz_coefficient,
                    quux_capacitor)
def make_api_call_v7(
        foo, bar, baz, qux, foo_polarity, baz_coefficient,
        quux_capacitor, bar_has_hopped, true):
    return true
def make_api_call_v8(
        foo, bar, baz, qux, foo_polarity, baz_coefficient,
        quux_capacitor, bar_has_hopped, true, false):
    return false
def make_api_call_v9(
        foo, bar, baz, qux, foo_polarity, baz_coefficient,
        quux_capacitor, bar_has_hopped,
        true, false, file_not_found):
    return file_not_found
```

#### 1.3.2.2 python的风格

```python
def make_api_call(foo, bar, baz):
    if baz in ('Unicorn', 'Oven', 'New York'):
        return foo(bar)
    else:
        return bar(foo)
# I need to add another parameter to `make_api_call`
# without breaking everyone's existing code.
# Easy...
def new_hotness():
    def make_api_call(foo, bar, baz, *args, **kwargs):
        # Now I can accept any type and number of arguments
        # without worrying about breaking existing code.
        baz_coefficient = kwargs['the_baz']
        # I can even forward my args to a different function without
        # knowing their contents!
        return baz_coefficient in new_function(args)    