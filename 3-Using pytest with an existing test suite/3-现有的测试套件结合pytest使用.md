Pytest可以执行大部分现有的测试用例，但是它的使用方法和[nose](https://docs.pytest.org/en/latest/nose.html#noseintegration)以及Python的默认测试框架unittest不同。

开始这一章节前，你需要先安装pytest。

### 使用pytest执行现有的测试

假如你想对某个地方现有的存储库做一些优化。当你通过版本控制工具把代码推送到开发环境之后，以及你要设置一个virtualenv，你可能会运行下面命令：
```
cd <repository>
pip install -e .  # Environment dependent alternatives include
                  # 'python setup.py develop' and 'conda develop'
```
在项目的根目录执行上面的命令，则你的代码将会和site-packages关联起来，允许在测试的过程中编辑代码，就好像它被安装了一样。

在开发模式下设置项目，可以避免每次运行测试时都要重新安装，而且比摆弄sys将测试指向本地代码的路径更灵活。

也可以考虑使用[tox](https://docs.pytest.org/en/latest/goodpractices.html#use-tox)
