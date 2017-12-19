# 2.9 变量

### 2.9.1 在交互数值时避免使用临时变量

Python中交互数值时没有使用临时变量的理由。可以使用元组使得互换更加清晰。

#### 2.9.1.1 不好的风格

```python
foo = 'Foo'
bar = 'Bar'
temp = foo
foo = bar
bar = temp
```

#### 2.9.1.2 python的风格
```python
foo = 'Foo'
bar = 'Bar'
(foo, bar) = (bar, foo)
```
