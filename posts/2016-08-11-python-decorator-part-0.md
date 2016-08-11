# Python Enclosing作用域、装饰器话聊


Jaglo: 听讲Python一切都是对象，是吗？

Pylego: 是的，像函数也是对象。

Jaglo: 那么函数也可以有自己的属性了？

Pylego： 当然，像下面这样写是可以的：

```python
def foo():
    print('I am foo')
    
def bar():
    print('I am bar')
    
foo.func = bar
foo.bar()
```

Jaglo: 这都行，那是不是函数也可以像普通对象一样当作参数传递也可以当作对象来返回？

Pylego: 是的，比如下面的用法:

```python
def deco(func):
    string = 'I am deco'
    def wrapper():
        print(string)
        func()
    return wrapper
    

def foo():
    print('I am foo')
    
    
foo = deco(foo)
foo()  
"""
输出:
I am deco
Iam foo
"""
```

Jaglo: 好吧，当作参数传递和返回我理解了，但是我对wrapper函数的**print(string)**这个string的查找感到迷惑。

Pylego: 不错嘛！这都被你看出来了，那你知道Python作用域的LEGB原则吗？

Jaglo: 我知道是知道可以我就是对那个E（Enclosing）作用域不是很理解。

Pylego: 那就对了，你可以在刚才代码的基础上运行下面的代码:

```python
print(foo.__closure__)
# 输出:(<cell at 0x7fc50f45afd8: function object at 0x7fc50f4168c0>, <cell at 0x7fc50f45aec0: str object at 0x7fc50c065fc0>)
```

Jaglo: 咦，这两个内存地址是啥家伙？

Pyelgo: 这就是wrapper函数引用的外层函数（就是deco函数啦）的两个变量：string和func啊！

Jaglo: 也就是说内层函数（在本例中就是wrapper啦）会把外层函数（在本例用就是deco啦）作用域里面的对象放到__closure__属性里，以供自己查找？

Pylego: 是的，但是不是所有外层函数作用域的对象都会放到内层函数的__closure__属性里，仅限自己用到的，这个__closure__就是enclosing作用域啦！

Jaglo: 原来enclosing作用域是这样的，明白了。

Pyelgo: 如果内部函数引用到外层函数作用域的对象，这个内部函数就称为闭包。

Jaglo: 原来闭包就是这家伙，很简单嘛！

Jaglo: 咦，我想到内部函数有一个妙用，你看看是不是这样啊，比如说我想输出一个函数的运行时间又不想去破坏这个函数的代码，是不是可以这样写:

```python
import time


def time_machine(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        func(*args, **kwargs)
        print(u'共耗时: %s秒' % (time.time()-start_time))
    return wrapper
    
    
def foo():
    time.sleep(3)
    
    
foo = time_machine(foo)
foo()
```

Pylego: 你这智商要冲出宇宙的节奏啊！但是Python的开发者早就想到每次**foo = time_machine(foo)**很麻烦，特地为你准备了语法糖，来接糖:

```python
import time


def time_machine(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        func(*args, **kwargs)
        print(u'共耗时: %s秒' % (time.time()-start_time))
    return wrapper
    

@time_machine
def foo():
    time.sleep(3)

"""
也就是说:
@time_machine
def foo():
    pass
相当于: foo = time_machine(foo)，这里是重点，这里是重点，这里是重点，以后的谈话能不能理解就看你对这个语句可理解！
"""

foo()
```

Pylego: time_machine就是装饰器(decorator)，名字起得多形象啊，装饰函数嘛！

Jaglo: 你这么一说，我就觉得装饰器咋这么简单呢！问题是我看到很多人的装饰器还带参数，还有人用类当装饰器，这又是咋回事呢？

Pylego: 你问题咋恁些？我先吃个饭，下次有空再聊哈！

未完，待续。。。