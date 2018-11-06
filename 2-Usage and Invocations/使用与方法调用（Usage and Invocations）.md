## 2.1 通过 pytest -m pytest 调用pytest 
***
在版本2.0更新。  
你可以通过以下命令在Python解释器里调用pytest：
```
python -m pytest [...]
```
上面的命令几乎等价于在命令行里直接执行脚本 pytest[...]，除了一点，通过python调用pytest也会把当前目录加入到sys.path。
## 2.2 可能退出的code码
***
运行pytest可能返回6种不同的退出code码：
```
退出码 0：当前目录下的测试用例都被收集起来并且都成功通过。
退出码 1：当前目录下的测试用例都被收集起来，但是有一些测试是失败的。
退出码 2：测试被用户中断。
退出码 3：执行测试的时候，刚好pytest内核出现错误。
退出码 4：命令用法错误。
退出码 5：没有收集到测试用例。
```
## 2.3 获取版本、名字选项，环境变量
```
pytest --version   # shows where pytest was imported from
pytest --fixtures  # show available builtin function arguments
pytest -h | --help # show help on command line and config file options
```
## 2.4 在第几个失败时停止

在出现第一个失败或者第N个失败时停止测试：
```
pytest -x            # stop after first failure
pytest --maxfail=2    # stop after two failures
```
## 2.5 指定测试、选择测试

pytest支持以下几种方式去执行和选择测试:  

2.5.1 以模块为单位执行：
```
pytest test_mod.py
```
2.5.2 以文件夹为单位执行：
```
pytest testing/
```
2.5.3 以关键字表述的方式执行：
```
pytest -k "MyClass and not method"
```
这种方式会去匹配给定的字符表述去执行测试，字符表述包括使用文件名、类名、函数名作为变量的Python操作符。上面命令行中的例子，pytest会执行TestMyClass.test_something，而不会执行TestMyClass.test_method_simple。

2.5.4 通过NodeID运行测试

每一个被收集起来的测试用例都会被指定一个唯一的NodeID，NodeID组成由模块全名加说明符（::），以及后面跟随着类名、函数名、参数化的参数。

执行一个模块内的特定测试：
```
pytest test_mod.py::test_func
```
另外一个例子：有class的情况下执行特定的测试
```
pytest test_mod.py::TestClass::test_method
```
2.5.5 通过标记表达式运行测试
```
pytest -m slow
```
上面的命令会执行被装饰器@pytest.mark.slow装饰过的所有测试。

更多信息请参看[标记表达](https://docs.pytest.org/en/latest/mark.html#mark)

2.5.6 从包里运行测试
```
pytest --pyargs pkg.testing
```

上面命令将会导入pkg.testing，使用它的文件系统所在的位置寻找并运行测试。

## 2.6 改在Python打印记录

列子： 
```
pytest --showlocals # show local variables in tracebacks
pytest -l           # show local variables (shortcut)

pytest --tb=auto    # (default) 'long' tracebacks for the first and last
                     # entry, but 'short' style for the other entries
pytest --tb=long    # exhaustive, informative traceback formatting
pytest --tb=short   # shorter traceback format
pytest --tb=line    # only one line per failure
pytest --tb=native  # Python standard library formatting
pytest --tb=no      # no traceback at all
```
