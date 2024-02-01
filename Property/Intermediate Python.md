## 生成器(Generators)

- 可迭代对象（Iterable）
> 可迭代对象是能够（通过`__iter__`或`__getitem__`方法）提供迭代器的对象
- 迭代器（Iterator）
> 迭代器是定义了`__next__`方法的对象
- 迭代（Iteration）
> 迭代是从可迭代对象中取出一个元素的过程

生成器是只能迭代一次的迭代器

迭代操作的局部性

`next()`的定义域是迭代器

`iter()`的定义域是可迭代对象，返回一个迭代器

生成器多以函数对象出现，比如参考中的
```python
# generator version
def fibon(n):
	a = b = 1
	for i in range(n):
		yield a
		a, b = b, a + b
```
使用方式为
```python
for x in fibon(10):  
    print(x)
```
其中
如果
```python
<generator object fibon at 0x000001D18C5D0F40>
```
说明`fibon(10)`是生成器，

参考: [《Python进阶》](https://eastlakeside.gitbook.io/interpy-zh/)
## 装饰器(Decorators)

### 在函数中定义和返回函数，将函数作为参数
分清作为映射和作为元素，能够理解在函数中定义函数

### 我的第一个装饰器
我想把参考里面这的代码写成右平移算子的形式，大概是这样
```python
def F(f):
    def tau_alpha(x):
        print("Hi! I'm rightTranslation")
        return f(x) + alpha
    return tau_alpha

def f(x):
    return y
```

```python
@F
def f(x):
    return y

f(x)
#outputs: I am doing some boring work before executing a_func()
#         I am the function which needs some decoration to remove my foul smell
#         I am doing some boring work after executing a_func()

#the @F is just a short way of saying:
g = F(f) 
```

$g(x) = F(f)(x) = \tau_\alpha(f)(x) = f(x + \alpha) = x + \alpha$

之后再使用f(x)其实是g(x) = F(f)(x)（这样写可能更接近数学g = F[f]）

这时我也可以说：一切都是映射

装饰(decorate)与包裹(wrap)

```python
def f(x):
    # say, y = f(x) = x
    return y
```

关于__name__

参考https://www.runoob.com/note/39287

- python中的一个内置类属性
- 自己的 __name__ 在自己用时就是 main，当自己作为模块被调用时就是自己的名字

然后提到了一个问题
```python
print(f.__name__)
# Output: g
```

就是f被装饰了之后就是g了，所以输出的是g的__name__而不是f的(f = g = F(f))

这时候需要用 functools.wraps
```python
from functools import wraps

def F(f):
    @wraps(f)
    def tau_alpha(x):
        print("Hi! I'm rightTranslation")
        return f(x) + alpha
    return tau_alpha

def f(x):
    return y
```

看到这里自然会有一个问题，就是alpha是什么，在右平移算子的例子中，alpha是算子的参数，如果算子翻译过来是装饰器，那么alpha就是装饰器的参数，

```python
from functools import wraps

def F_with_parameter(alpha):
    def F(f):
        @wraps(f)
        def tau_alpha(x):
            print("Hi! I'm rightTranslation")
            return f(x) + alpha
        return tau_alpha
    return F

def f(x):
    return y
```

or

```python
from functools import wraps

def F(f, alpha):
    @wraps(f)
    def tau_alpha(x):
        print("Hi! I'm rightTranslation")
        return f(x) + alpha
    return tau_alpha
return F

def f(x):
    return y
```

https://blog.csdn.net/sinat_38682860/article/details/114695466

关于装饰器大概就是这样

本文主要介绍`Python`中的容器(collection)
