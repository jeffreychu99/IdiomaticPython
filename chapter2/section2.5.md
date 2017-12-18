# 2.5 集合

### 2.5.1 使用集合来消除可迭代容器中的重复项

字典或列表中有重复项很常见。大公司所有员工的姓氏中，同样的姓氏会不止一次地出现。如果用列表来处理唯一的姓氏，工作量将很大。集合三个方面的特性可以完美回答上述问题：

1. 集合仅包含唯一的元素
2. 集合中添加已存在的元素会被忽略
3. 可迭代容器中可以hash的元素都可以构建到集合中

继续上面的例子，有一个接受序列参数的display函数，可以以某一格式显示参数中的元素。从原始列表创建集合后，是否需要改变display函数？

答案是：否。假定我们的display函数实现是合理的，那么集合可以直接替换列表。这得益于集合事实上像列表一样，是可迭代的，可以在循环、列表推导中等使用。

2.5.1.1 Harmful

```python
unique_surnames = []
for surname in employee_surnames:
    if surname not in unique_surnames:
        unique_surnames.append(surname)
def display(elements, output_format='html'):
    if output_format == 'std_out':
        for element in elements:
            print(element)
    elif output_format == 'html':
        as_html = '<ul>'
    for element in elements:
        as_html += '<li>{}</li>'.format(element)
        return as_html + '</ul>'
    else:
        raise RuntimeError('Unknown format {}'.format(output_format))
```

2.5.1.2 Idiomatic

```python
unique_surnames = set(employee_surnames)
def display(elements, output_format='html'):
    if output_format == 'std_out':
        for element in elements:
            print(element)
    elif output_format == 'html':
        as_html = '<ul>'
        for element in elements:
            as_html += '<li>{}</li>'.format(element)
            return as_html + '</ul>'
    else:
    raise RuntimeError('Unknown format {}'.format(output_format))
```
