---
title: 流畅的Python
tags: Python
abbrlink: 32ffb807
date: 2024-03-06 09:02:17
---

《流畅的Python》读书笔记

<!-- more -->

### 第一章 Python数据模型

```python
import collections
from random import choice
Card = collections.namedtuple('Card' , ['rank' , 'suit'])
class FrenchDeck:
    ranks = [str(n) for n in range(2,11)] + list('JQKA')
    suits  = 'spades diamonds clubs hearts'.split()
    
    def __init__(self):
        self._cards = [Card(rank , suit) for rank in self.ranks 
                                         for suit in self.suits]
    def __len__(self):
        return len(self._cards)
    def __getitem__(self,position):
        return self._cards[position]
    
if __name__ == '__main__':
    deck = FrenchDeck()
    print(len(deck))
    print(deck[-1])
    print(deck[:3])
    for i in range(10):
        print(choice(deck))
```

- `namedtuple`

> In Python, NamedTuple is present inside the collections module. It provides a way to create simple, lightweight data structures similar to a class, but without the overhead of defining a full class. Like dictionaries, they contain keys that are hashed to a particular value. On the contrary, it supports both access from key-value and iteration, the functionality that dictionaries lack.

[Namedtuple in Python - GeeksforGeeks](https://www.geeksforgeeks.org/namedtuple-in-python/)

划重点:①一个轻量的数据结构类；②相比于字典，还有下标索引的功能。

**序列协议:**

- 在 Python 中，序列是一种通用的数据结构，可以包含多个元素，并且支持**迭代**、**索引**和**切片**操作。
- 序列协议定义了一组**特殊方法**，例如 `__len__` 和 `__getitem__`，用于使对象表现得像序列一样。
- 如果一个类实现了这些方法，它的**实例**就可以像**列表**、**字符串**或**元组**一样进行操作。

`FrenchDeck`类中实现了`__len__`方法后，对实例化类`deck`执行`len(deck)`时，实际上调用的是`deck.__len__(self)` ; 同理，对`deck`执行切片、下标索引时实际调用的是`__getitem__`方法。

**sorted**:

```python
for card in sorted(deck , key=card_value):
        print(card)
```

其中`card_value`不需要传入参数`card` , `sorted`会自动将`deck`中每一个`card`传入`key`中,不需要显式传参。



```python
from math import sqrt
class Vector:
    def __init__(self , x=0 , y=0):
        self.x = x
        self.y = y
    def __repr__(self) -> str:
        return 'Vector(%r,%r)'%(self.x , self.y)
    def __abs__(self):
        return sqrt(self.x**2 + self.y**2)
    def __bool__(self):
        return bool(self.x or self.y)
    def __add__(self , other):
        return Vector(self.x+other.x , self.y+other.y)
    def __mul__(self , scalar):
        return Vector(self.x*scalar , self.y*scalar) 
if __name__ == '__main__':
    v1 = Vector(3,4)
    v2 = Vector(2,2)
    v3 = Vector(0,0)
    print(v1+v2)
    print(v1*2)
    print(abs(v1))
    print(bool(v3))
```

### 第二章 数据结构

**列表推导与生成器表达式**

```python
if __name__ == '__main__':
    colors = ['black', 'white']
    sizes = ['S', 'M', 'L']
    tshirt = [(color,size) for color in colors
                            for size in sizes]
    print(tshirt)
    symbols = '$¢£¥€¤'
    a = list(ord(symbol) for symbol in symbols if ord(symbol) > 127)
    b = [ ord(symbol) for symbol in symbols if ord(symbol) > 127 ]
    print(a , b)
```

> 虽然也可以用列表推导来初始化元组、数组或其他序列类型，但是生成
> 器表达式是更好的选择。这是因为生成器表达式背后遵守了迭代器协
> 议，可以逐个地产出元素，而不是先建立一个完整的列表，然后再把这
> 个列表传递到某个构造函数里。前面那种方式显然能够节省内存。

**元组**

```python
if __name__ == '__main__':
    lax_coordinates = (33.9425, -118.408056)
    city, year, pop, chg, area = ('Tokyo', 2003, 32450, 0.66, 8014)
    traveler_ids = [('USA', '31195855'), ('BRA', 'CE342567'),('ESP', 'XDA205856')]
    for passport in sorted(traveler_ids):
        print('%s/%s' % passport)
```

> 我们把元组 ('Tokyo', 2003, 32450, 0.66, 8014) 里
> 的元素分别赋值给变量 city、year、pop、chg 和 area，而这所有的
> 赋值我们只用一行声明就写完了。同样，在后面一行中，一个 % 运算符
> 就把 passport 元组里的元素对应到了 print 函数的格式字符串空档
> 中。这两个都是对元组拆包的应用。

不使用中间变量交换两个变量的值

```python
a , b = b , a
```

实际此处涉及到元组的解包:python首先解析右值，组成元组(b,a),再解包赋值给a,b

***运算符**

```python
def devmode(a , b):
    return (a//b , a%b)
t = (12,7)
print(devmode(*t))
a , b , *_ = range(5)
print(a,b,_)
```

**嵌套元组**

```python
if __name__ == '__main__':
    metro_areas = [
    ('Tokyo','JP',36.933,(35.689722,139.691667)), 
    ('Delhi NCR', 'IN', 21.935, (28.613889, 77.208889)),
    ('Mexico City', 'MX', 20.142, (19.433333, -99.133333)),
    ('New York-Newark', 'US', 20.104, (40.808611, -74.020386)),
    ('Sao Paulo', 'BR', 19.649, (-23.547778, -46.635833)),
    ]
    print('{:15} | {:^9} | {:^9}'.format('', 'lat.', 'long.'))
    fmt = '{:15} | {:9.4f} | {:9.4f}'
    for name, cc, pop, (latitude, longitude) in metro_areas:
        if longitude <= 0:
            print(fmt.format(name, latitude, longitude))
```

`{:15}`表示占15格 ， `{:^9}`表示占9格，字符串居中 ， `{:>9}`右对齐占9格，`{:9.4f}`占9格，保留四位小数的浮点数



**具名元组collections.namedtuple**

```python
from collections import namedtuple
if __name__ == '__main__':
    city = namedtuple('city' , ['name' , 'country' , 'pop' , 'coordinates'])
    tokyo = city('tokyo' , 'Japan' , '36.933' , (35,139))
    print(tokyo.pop , tokyo.coordinates)

    print(city._fields)
    delhi_data = ('Delhi NCR', 'IN', 21.935, (28.613889, 77.208889))
    delhi = city._make(delhi_data)
    print(delhi._asdict)
```

`._fields`返回具名元素所有的字段名称

`._make`支持以**元组(或列表)**创建具名元组

`._asdict`把具名元组以 collections.OrderedDict 的形式返回，我们可以利用它来把元组里的信息友好地呈现出来。



**切片**

```python
s = 'bicycle'
print(s[::3] , s[::-1] , s[::-2])
print(s[1:5:2] , s[5:1:-2])
```

`start:stop:step`    `step`为负值表示反向切片

**对序列使用***

```python
if __name__ == '__main__':
    l = [1,2,3]
    print(l * 3 , 3 * 'abc')
    board = [['_']*3 for i in range(3)]
    print(board)
    board[1][2] = 'X'
    print(board)
    board_ = [['_']*3]*3
    board_[1][2] = 'X'
    print(board_)


```

```shell
$ python chapter2.py 
[1, 2, 3, 1, 2, 3, 1, 2, 3] abcabcabc
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
[['_', '_', '_'], ['_', '_', 'X'], ['_', '_', '_']]
[['_', '_', 'X'], ['_', '_', 'X'], ['_', '_', 'X']]

```

`board_ = [['_']*3]*3`  这种方法创建了一个包含三个引用的列表，修改其中一个列表，其他两个列表都会变化。



### 第三章 字典和集合

**字典初始化**

```python
if __name__ == '__main__':
    a = dict(one=1, two=2, three=3)
    b = {'one': 1, 'two': 2, 'three': 3}
    c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
    d = dict([('two', 2), ('one', 1), ('three', 3)])
    e = dict({'three': 3, 'one': 1, 'two': 2})
    print(a == b == c == d == e)#以上五种字典初始化方式等价

```

**字典推导**

```python
DIAL_CODES = [(86, 'China'),(91, 'India'),(1, 'United States'),(62, 'Indonesia'),(55, 'Brazil'),(92, 'Pakistan'),(880, 'Bangladesh'),(234, 'Nigeria'),(7, 'Russia'),(81, 'Japan')]
country_code = {country : code for code,country in DIAL_CODES if code > 66}
print(country_code)

```

**集合**

```python
if __name__ == '__main__':
    l = [1,2,3,4,5,6,7,1,2,3]
    s = set(l) 
    print(s)    

    s2 = {4,6,9}
    print(s&s2)
    print(s|s2)
    print(s-s2)

    s3 = set()
    s3.add(1)
    # s3.pop()
    print(s3)

```

`s = set(l)`会将列表`l`中元素去重

`s&s2`求交集，`s|s2`求并集，`s-s2`求补集