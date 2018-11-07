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
***
```
pytest --version   # shows where pytest was imported from
pytest --fixtures  # show available builtin function arguments
pytest -h | --help # show help on command line and config file options
```
## 2.4 在第几个失败时停止
***

在出现第一个失败或者第N个失败时停止测试：
```
pytest -x            # stop after first failure
pytest --maxfail=2    # stop after two failures
```
## 2.5 指定测试、选择测试
***

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

## 2.6 改造Python打印记录
***

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
使用参数 --full-trace，当出现错误时会产生大量log（比参数--tb=long产生的log还要多）。这个参数也确保了在KeyboardInterrupt（Ctrl + C）模块中打印堆栈记录。当测试运行时间比较长的时候，可以使用 Ctrl + C 打断测试，并找到测试在哪挂掉了，这样来说，使用参数还是很有用的。默认情况下键盘打断测试执行后将没有输出展示（因为键盘打断被pytest捕获到了。）当使用这些选项的时候，堆栈记录确保会显示。

## 2.7 详细的总结报告
***
版本2.9更新  

当执行完测试的时候，-r 参数可以展示测试的概要结果，这样一来，当执行大型测试套件的时候，失败的用例，跳过的用例，xfails的用例等等都一目了然。  

举个例子：  
```
$ pytest -ra
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-3.x.y, py-1.x.y, pluggy-0.x.y
rootdir: $REGENDOC_TMPDIR, inifile:
collected 0 items

======================= no tests ran in 0.12 seconds =======================
````

参数 -r 后面可以跟一些字符，例如 -ra, a表示报告除了通过的测试之外的失败的测试结果。

下面是可用的字符：

```
f - failed
E - error
s - skipped
x - xfailed
X - xpassed
p - passed
P - passed with output
a - all except pP
```

字符可以混合使用，例如，只看失败和跳过的测试：
```
$ pytest -rfs
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-3.x.y, py-1.x.y, pluggy-0.x.y
rootdir: $REGENDOC_TMPDIR, inifile:
collected 0 items

======================= no tests ran in 0.12 seconds =======================
```

## 2.8 测试失败时切换到Python调试器（PDB）
***

Python内置了一个调试器叫[PDB](https://docs.python.org/3/library/pdb.html)。pytest允许通过命令行模式进入PDB。
```
pytest --pdb
```  

一旦使用了这个参数，当用例失败或者键盘打断的时候将会调用PDB。通常来说，你可能只会想要在第一次出现错误的时候用到这个参数，以便定位到错误的原因。

```
pytest -x --pdb   # drop to PDB on first failure, then end test session
pytest --pdb --maxfail=3  # drop to PDB for first three failures   前三个失败进入PDB
```

值得注意的是任何测试失败的异常信息都会存储在sys.last_value, sys.last_type，sys.last_traceback，在Python的命令行交互模式中，允许使用任何调试
工具进行后期调试。也可以手动的取出异常信息。举个例子：
```pyhon
>>> import sys
>>> sys.last_traceback.tb_lineno
42
>>> sys.last_value
AssertionError('assert result == "ok"',)
```

## 2.9 在测试开始的时候就进入PDB（Python调试器）
***
pytest在每次测试开始的时候立即进入PDB模式：
```
pytest --trace
```
上面的参数会在每个测试用例开始的时候调用Python调试器。

## 2.10 设置断点
***
在本地脚本里import pdb，然后设置断点，pdb.set_trace()在代码里可以被调用，pytest会自动禁用该测试的输出捕获：

※ 其他测试中的输出捕获不受影响。
※ 任何已经被捕获的测试输出以及将要被这样处理。
※ 在同一个测试中生成的任何后续输出都不会被捕获，而是直接发送给sys.stdout。需要注意的是，即使在退出交互式PDB模式后，继续执行的常规测试的输出也会发送给sys.stdout。
## 2.11 内置断点的使用 
***
Python在版本3.7引入内置的breakpoint()功能。Pytest支持在以下行为中使用breakpoint()：  
※ 当调用breakpoint()并且把PYTHONBREAKPOINT设置为默认值的时候，pytest将会使用定制的PDB记录UI，而不是系统默认的PDB。  
※ 当测试完成后，系统将会默认的回到系统之前的PDB记录UI。  
※ 当参数 --pdb 被使用执行的时候，自定义的PDB将会用在breakpoint()以及失败、未处理的测试用例上。  
※ 当使用参数 --pdbcls 的时候，一旦测试失败则调试类就会被执行。但是，当breakpoint()在测试用例内部调用的时候，定制的调试类将会被实例化。  

## 2.12 分析测试执行时长
***
获取执行时间最长的10个测试用例：
```
pytest --durations=10
```
Pytest默认不展示执行时间太短（<0.01s）的测试用例，除非加上参数 -vv。

## 2.13 创建JUnitXML格式的文件
***

2.13.1 生成的结果文件可以被Jenkins或者其他的持续集成工具读取，用法：
```
pytest --junitxml=path
```
在指定路径上生成xml文件。  

版本3.1更新。

当想要设置测试套件生成的xml文件名时，你可以在配置文件里使用junit_suite_name选项：
```
[pytest]
junit_suite_name = my_suite
```
2.13.2 属性记录  

版本2.8更新。

在版本3.5里有更改，夹具名从record_xml_property改为record_property，并且作为用户属性所有的报告都可以访问。record_xml_property已经被弃用了。  
如果你想获得测试额外的log信息，你可以使用record_property夹具：
```python
def test_function(record_property):
    record_property("example_key", 1)
    assert True
```
这将会在测试用例tag里增加一条额外的属性，例如example_key="1"：
```
<testcase classname="test_function" file="test_function.py" line="0" name="test_function" time="0.0009">
  <properties>
    <property name="example_key" value="1" />
  </properties>
</testcase>
```
或者，你也可以将此功能与自定义标记集成:
```python
# content of conftest.py

def pytest_collection_modifyitems(session, config, items):
    for item in items:
        for marker in item.iter_markers(name="test_id"):
            test_id = marker.args[0]
            item.user_properties.append(("test_id", test_id))
```
如下：  
```python
# content of test_function.py
import pytest


@pytest.mark.test_id(1501)
def test_function():
    assert True
```
将会导致：
```
<testcase classname="test_function" file="test_function.py" line="0" name="test_function" time="0.0009">
  <properties>
    <property name="test_id" value="1501" />
  </properties>
</testcase>
```
警告：
```
record_property是一个实验性的特性，将来可能会更改。  
另外请注意，使用此特性将打断任何模式的测试。当与一些CI服务一起使用时，会是个麻烦的问题。
```
2.13.3 record_xml_attribute  

版本3.4更新  

要向testcase元素添加额外的xml属性，可以使用record_xml_attribute夹具。  

也可以用来覆盖现有的值:  
```python
def test_function(record_xml_attribute):
    record_xml_attribute("assertions", "REQ-1234")
    record_xml_attribute("classname", "custom_classname")
    print("hello world")
    assert True
```
与record_property不同的是，使用record_xml_attribute不会增加子元素，而是在生成的测试用例tag里增加一个属性assertions="REQ-1234"，覆盖默认的classname数据：
```
<testcase classname="custom_classname" file="test_function.py" line="0" name="test_function" time="0.003" assertions="REQ-1234">
    <system-out>
        hello world
    </system-out>
</testcase>
```
警告：
```
record_xml_attribute是一个实验性的特性，在未来的版本中，它的接口可能会被更强大更通用的东西所取代。然而，功能本身将被保留。  

在使用ci工具解析xml报告时，使用这个上面的record_xml_property会有所帮助。然而，有些解析器对于允许的元素和属性非常严格。许多工具使用xsd模式(如下面的示例)来验证传入的xml。  

确保使用的解析器可以识别你使用的属性名。
```
一下的方案是使用Jenkins验证XML报告：
```
<xs:element name="testcase">
    <xs:complexType>
        <xs:sequence>
            <xs:element ref="skipped" minOccurs="0" maxOccurs="1"/>
            <xs:element ref="error" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element ref="failure" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element ref="system-out" minOccurs="0" maxOccurs="unbounded"/>
            <xs:element ref="system-err" minOccurs="0" maxOccurs="unbounded"/>
        </xs:sequence>
        <xs:attribute name="name" type="xs:string" use="required"/>
        <xs:attribute name="assertions" type="xs:string" use="optional"/>
        <xs:attribute name="time" type="xs:string" use="optional"/>
        <xs:attribute name="classname" type="xs:string" use="optional"/>
        <xs:attribute name="status" type="xs:string" use="optional"/>
    </xs:complexType>
</xs:element>

```
2.13.4 LogXML: add_global_property

版本3.0更新。

如果您想在testsuite级别添加一个属性节点，它可能包含与所有测试用例相关的属性，那么可以使用LogXML.add_global_properties。
```python
import pytest


@pytest.fixture(scope="session")
def log_global_env_facts(f):

    if pytest.config.pluginmanager.hasplugin("junitxml"):
        my_junit = getattr(pytest.config, "_xml", None)

    my_junit.add_global_property("ARCH", "PPC")
    my_junit.add_global_property("STORAGE_TYPE", "CEPH")


@pytest.mark.usefixtures(log_global_env_facts.__name__)
def start_and_prepare_env():
    pass


class TestMe(object):
    def test_foo(self):
        assert True
```
这将向生成的xml中添加testsuite节点下的子节点:
```
<testsuite errors="0" failures="0" name="pytest" skips="0" tests="1" time="0.006">
  <properties>
    <property name="ARCH" value="PPC"/>
    <property name="STORAGE_TYPE" value="CEPH"/>
  </properties>
  <testcase classname="test_me.TestMe" file="test_me.py" line="16" name="test_foo" time="0.000243663787842"/>
</testsuite>
```
警告：
```
add_global_property是一个实验性的特性，在未来的版本中，它的接口可能会被更强大更通用的东西所取代。然而，功能本身将被保留。  
```
## 2.14 生产结果文件  
***

从版本3.0开始弃用，--resultlog 很少被使用，安排在4.0中移除。  

对于仍然需要类似功能的用户，另一种选择是使用[pytest-tap](https://pypi.org/project/pytest-tap/)插件，它提供测试数据流。  

如果你有任何顾虑，请不要犹豫，立即提issue。

生成一个机器可读的纯文本文件：
```
pytest --resultlog=path
```
并查看路径位置的内容。例如，经过几个版本的修订，[PyPy-test](http://buildbot.pypy.org/summary) 网页使用这些文件来显示测试结果。
## 2.15 向在线的pastebin服务器发送测试报告
***
2.15.1 为每个失败的测试用例创建URL：
```
pytest --pastebin=failed
```
这将向远程粘贴服务提交测试运行信息，并为每次失败提供一个URL。如果您只想发送一个特定的失败，您可以像往常一样选择测试或者添加-x。

2.15.2 为全部测试用例的log创建URL

```
pytest --pastebin=all
```
目前只在http://bpaste.net 的服务上实现了粘贴。
