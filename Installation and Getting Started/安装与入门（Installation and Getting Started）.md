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
