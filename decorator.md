### 装饰器

##### 例一

###### level 1

```python
import datetime

def running_time():
    now = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    print(now)

def foo():
    print('this is a foo function')


if __name__ == '__main__':
    running_time()
    foo()
```

> 2019-07-03 16:56:02
> this is a foo function

###### level 2

```python
import datetime

def running_time(func):
    def wrapper():
        now = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        print(now)
        return func()
    return wrapper

def foo():
    print('this is a foo function')
    
if __name__ == '__main__':
    t = running_time(foo)
    t()
```

###### level 3

```python
import datetime

def running_time(func):
    def wrapper():
        now = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        print(now)
        return func()
    return wrapper

@running_time
def foo():
    print('this is a foo function')
    

if __name__ == '__main__':
    foo()
```

##### 例二

###### level 1

```python
import json

def appointment(func):
    def wrapper(response):
        r = json.loads(response)
        am_info = r['sch'].get('1597_4379').get('1597_4379_am')
        return func(**am_info)
    return wrapper

def parse(**kwargs):
    available_list = kwargs.keys()
    return available_list

if __name__ == '__main__':
    with open(r'XXX\r.json', 'r', encoding='utf-8') as f:
        content = f.read()
    t = appointment(parse)
    l = t(content)
    print(l)
```

###### level 2

```python
import json

def appointment(func):
    def wrapper(response):
        r = json.loads(response)
        am_info = r['sch'].get('1597_4379').get('1597_4379_am')
        return func(**am_info)
    return wrapper

@appointment
def parse(**kwargs):
    available_list = kwargs.keys()
    return available_list

if __name__ == '__main__':
    with open(r'XXX\r.json', 'r', encoding='utf-8') as f:
        content = f.read()
    l = parse(content)
    print(l)
```

> 关键点在于， 当 parse() 函数被 appointment 装饰之后， 调用的使用不是传入一个字典值了， 例子中传递进去的 content 是 str 类型。content 是传递给形参 response 的。wrapper() 函数处理完之后得到的值 am_info 再以参数的形式传递给 parse() 函数调用。
