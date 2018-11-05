## 1.1 安装 pytest
***

1、在命令行里运行以下命令：

```
pip install -U pytest
```

2、检查是否安装的正确的版本：

```
$ pytest --version
This is pytest version 3.x.y, imported from $PYTHON_PREFIX/lib/python3.6/site-packages/pytest.py
```

## 1.2 创建你的第一个测试用例
***

创建一个简单的由4行代码构成的测试函数：
```python
# content of test_sample.py
def func(x):
    return x + 1

def test_answer():
    assert func(3) == 5
```

就是这样，你现在可以执行一下这个函数：

```
$ pytest
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-3.x.y, py-1.x.y, pluggy-0.x.y
rootdir: $REGENDOC_TMPDIR, inifile:
collected 1 item

test_sample.py F                                                     [100%]

================================= FAILURES =================================
_______________________________ test_answer ________________________________

    def test_answer():
>       assert func(3) == 5
E       assert 4 == 5
E        +  where 4 = func(3)

test_sample.py:5: AssertionError
========================= 1 failed in 0.12 seconds =========================
```

测试返回一个失败，是因为func(3)不等于5。

```
注意： 

你可以用断言语句去验证预期结果。pytest[高级的自省式断言]将会智能的报告断言语句的中间值，这样有效的避免了单元测试遗留的方法中会有很多名字。
```
## 1.3 运行多个测试
***
pytest会运行当前目录及其子目录中命名为test_*.py 或者 *_test.py的所有文件。一般来讲，pytest遵循[标准的测试规则](https://docs.pytest.org/en/latest/goodpractices.html#test-discovery)。
## 1.4 断言引发某个异常
***
运用引发助手去断言某些代码会引发异常：
```python
# content of test_sysexit.py
import pytest
def f():
    raise SystemExit(1)

def test_mytest():
    with pytest.raises(SystemExit):
        f()
```
用简洁模式执行测试函数：
```
$ pytest -q test_sysexit.py
.                                                                    [100%]
1 passed in 0.12 seconds
```
## 1.5 在类里面执行多个测试
***
当你写了多个测试用例后，你希望用一个类把他们组合起来。pytest可以非常简单的创建一个包含很多测试的类。
```python
# content of test_class.py
class TestClass(object):
    def test_one(self):
        x = "this"
        assert 'h' in x

    def test_two(self):
        x = "hello"
        assert hasattr(x, 'check')
```
pytest将会依据python测试发觉的约定找到所有测试，所以ta找到了两个test_为前缀的函数。我们可以很简单的通过传递文件名去执行模块。
```
$ pytest -q test_class.py
.F                                                                   [100%]
================================= FAILURES =================================
____________________________ TestClass.test_two ____________________________

self = <test_class.TestClass object at 0xdeadbeef>

    def test_two(self):
        x = "hello"
>       assert hasattr(x, 'check')
E       AssertionError: assert False
E        +  where False = hasattr('hello', 'check')

test_class.py:8: AssertionError
1 failed, 1 passed in 0.12 seconds
```
