# 2.6 生成器

#2.6.1 使用生成器惰性加载无穷序列

通常能够提供一种可以迭代无穷序列的方式是很有用的，否则，需要提供一个异常昂贵开销的接口来实现，并且用户还需花时间来等待列表的构建。

面临这些情况，使用生成器就很有帮助。生成器是协程的一种特殊类型，它返回iterable类型。生成器的状态会被保存，当再次调用生成器时，可以继续上次离开时的状态。下面的例子，介绍了如何生用生成器处理上面提到的情况。

#### 2.6.1.1 不好的风格

```python
def get_twitter_stream_for_keyword(keyword):
    """Get's the 'live stream', but only at the moment
    the function is initially called. To get more entries,
    the client code needs to keep calling
    'get_twitter_livestream_for_user'. Not ideal.
    """
    imaginary_twitter_api = ImaginaryTwitterAPI()
    if imaginary_twitter_api.can_get_stream_data(keyword):
        return imaginary_twitter_api.get_stream(keyword)
current_stream = get_twitter_stream_for_keyword('#jeffknupp')
for tweet in current_stream:
    process_tweet(tweet)
# Uh, I want to keep showing tweets until the program is quit.
# What do I do now? Just keep calling
# get_twitter_stream_for_keyword? That seems stupid.
def get_list_of_incredibly_complex_calculation_results(data):
    return [first_incredibly_long_calculation(data),
            second_incredibly_long_calculation(data),
            third_incredibly_long_calculation(data),
            ]
```

#### 2.6.1.2 python的风格

```python
def get_twitter_stream_for_keyword(keyword):
    """Now, 'get_twitter_stream_for_keyword' is a generator
    and will continue to generate Iterable pieces of data
    one at a time until 'can_get_stream_data(user)' is
    False (which may be never).
    """
    imaginary_twitter_api = ImaginaryTwitterAPI()
    while imaginary_twitter_api.can_get_stream_data(keyword):
        yield imaginary_twitter_api.get_stream(keyword)
# Because it's a generator, I can sit in this loop until
# the client wants to break out
for tweet in get_twitter_stream_for_keyword('#jeffknupp'):
    if got_stop_signal:
        break
    process_tweet(tweet)
def get_list_of_incredibly_complex_calculation_results(data):
    """A simple example to be sure, but now when the client
    code iterates over the call to
    'get_list_of_incredibly_complex_calculation_results',
    we only do as much work as necessary to generate the
    current item.
    """
    yield first_incredibly_long_calculation(data)
    yield second_incredibly_long_calculation(data)
    yield third_incredibly_long_calculation(data)
```


### 2.6.2 倾向使用生成器表达式到列表推导中用于简单的迭代

在处理序列时，通常需要在轻微修改的序列版本上迭代一次。譬如，你可以需要以首字母大写的方式打印所有用户的姓。

第一本能可能是就地构建并迭代序列。列表推导可能是理想的，不过Python有个内建的更好的处理方式：生成器表达式。

有什么不同？列表推导构建列表对象并立即填充所有的元素。对于大型的列表，可能非常耗资源。生成器返回生成器表达式，换句话说，按需产生每一个元素。你想要大写字母转换的列表？问题不大。但是，如果你想要写出国会图书馆中已知的每本书的标题时，可能在列表推导的过程中就会耗尽系统内存，而生成器表达式可以泰然处之。生成器表达式工作的逻辑扩展方式使得我们可以把它们应用到无穷序列中。

#### 2.6.2.1 不好的风格

```python
for uppercase_name in [name.upper() for name in get_all_usernames()]:
    process_normalized_username(uppercase_name)
```

#### 2.6.2.2 python的风格

```python
for uppercase_name in (name.upper() for name in get_all_usernames()):
    process_normalized_username(uppercase_name)
```



