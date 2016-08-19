# Python Enclosing作用域、闭包、装饰器话聊下篇


Python Enclosing作用域、闭包、装饰器的基础篇，请看[Python Enclosing作用域、闭包、装饰器话聊上篇](https://github.com/pylego/blog/blob/master/posts/2016-08-11-python-decorator-part-0.md)

Jaglawz: 我经常看到有人的装饰器是带参数的，这又是咋回事呢？

Pylego: 这个其实很简单的，你还记得上次我说:

```python
@deco
def foo():
    pass

# 相当于: foo = deco(foo)
# 那么
@new_deco(*args, **kwargs):
def bar():
    pass
    
# 相当于: bar = new_deco(*args, **kwargs)(bar)
```

Jaglawz: 也就是说，new_deco返回的是一个装饰器函数，然后再去装饰其他函数。那类装饰器又是怎么回事呢？

Pylego: 你知道Python的对象可以像函数一样调用吗？

```python
from hashlib import sha256


class HashCache(object):
    def __init__(self):
        self.cache = {}
        
    def __call__(self, string):
        if string not in self.cache:
            self.cache[string] = sha256(string).hexdigest()
        return self.cache[string]
            
            
hc = HashCahce()
hc('foo')  # 像函数一样调用hc对象
hc('foo')
hc('bar')
```

Jaglawz: 如果是这样的话我就明白了。

```python
class FibCache(object):
    def __init__(self, func):
        self.func = func
        self.cache = {}
    
    def __call__(self, n):
        if n not in self.cache:
            self.cache[n] = self.func(n)
        return self.cache[n]
        
        
@FibCache
def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)
        
# 相当于: fib = FibCache(fib)
# fib(10)相当于FibCache(fib)(10)
# 装饰后的fib是FibCache的一个对象而已

# 也就是说作为装饰器的类的构造方法要接收一个待装饰的函数，然后__call__函数的参数要和待装饰的函数的参数是一样的(除了self)，这样的类就可以用来装饰函数了
```


Pylego: 你这个装饰器厉害，还给fibnacci函数加了缓存，佩服！

Jaglawz: 我不会告诉你我是看了大神的杰作然后吓吓唬吓唬你的！