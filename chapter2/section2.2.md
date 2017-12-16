# 2.2 字典

### 2.2.1 使用dict.get方法的默认参数来提供默认值

在dict.get的定义中经常被忽略的是默认参数。没有使用默认（或collections.defaultdict类）值，代码将会被if语句搞晕。记住，要力求清晰。

#### 2.2.1.1 不好的风格

```python
log_severity = None
if 'severity' in configuration:
    log_severity = configuration['severity']
else:
    log_severity = 'Info'
```

#### 2.2.1.2 python的风格

```python
log_severity = configuration.get('severity', 'Info')
```

#### 2.2.2 使用字典推导来更清晰和有效地构建字典

列表推导是python知名的构造方式，鲜为人知的是字典推导。不过，目的都是一样的：使用容易理解的推导语法构造字典。

#### 2.2.2.1 不好的风格

```python
user_email = {}
for user in users_list:
    if user.email:
        user_email[user.name] = user.email
```

####2.2.2.2 python的风格

```python
user_email = {user.name: user.email
                for user in users_list if user.email}
```
