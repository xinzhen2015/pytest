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
以模块为单位执行：
```
pytest test_mod.py
```
以文件夹为单位执行：
```
pytest testing/
```
以关键字表示的方式执行：
```
pytest -k "MyClass and not method"
```
