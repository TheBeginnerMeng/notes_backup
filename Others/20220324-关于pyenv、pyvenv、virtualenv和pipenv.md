# 关于pyenv、pyvenv、virtualenv和pipenv

## 1. Python版本管理

首先介绍专门用来管理Python版本的工具：pyenv

### pyenv

pyenv 不是用来管理同一个库的多个版本，而是用来管理一台机器上的多个 Python 版本。主要解决开发中有的项目需要用Python 2.x，有的项目需要用Python 3.x的场景。

pyenv没有Windows版本，如果需要使用，可以安装pyenv-win

pyenv的使用方法：

```cmd
# 安装 pyenv（推荐方法，此脚本会自动安装若干插件，包括下文即将提到的 pyenv virtualenv）
curl https://pyenv.run | bash
# 查看所有支持安装的 Python 版本
pyenv install -l
# 安装 Python 2.7.17 和 3.8.2
pyenv install 2.7.17
pyenv install 3.8.2
# 指定全局使用 Python 2.7.17
pyenv global 2.7.17
# 指定 myproject 使用 Python 3.8.2
cd myproject
pyenv local 3.8.2
# 在当前 shell 中临时使用 Python 3.8.2
pyenv shell 3.8.2
```

上面例子中在在 myproject 项目目录设置了 pyenv local 3.8.2 之后，后续进入该目录及其子目录时，所执行的 python 命令就是 3.8.2 版本，不需手动执行 activate；离开该目录之后，执行的的 python 命令就是系统安装的或者 pyenv global 指定的版本，不需要手动执行 deactivate。

上述几种用法中，优先级为：pyenv shell > pyenv local > pyenv global > system。即优先使用 pyenv shell 设置的版本，三种级别都没设置时才使用系统安装的版本。

![img](https://miro.medium.com/max/875/1*8szW_MdMsG25wRp0H3kFvw.png)

1. 版本管理

- `pyenv versions` 查看本机已有版本
- `pyenv install -l` 查看可安装的版本
- `pyenv install 2.7.3` 安装指定的版本
- `pyenv uninstall 2.7.3` 卸载指定的版本

2. 切换版本，分为3种，按优先级排序:shell local global

- `pyenv shell 2.7.3` 设置面向 shell 的 Python 版本，通过设置当前 shell 的 PYENV_VERSION 环境变量的方式。这个版本的优先级比 local 和 global 都要高。`–unset` 参数可以用于取消当前 shell 设定的版本 `pyenv shell --unset`。
- `pyenv local 2.7.3` 设置 Python 本地版本，通过将版本号写入当前目录下的 .python-version 文件的方式。通过这种方式设置的 Python 版本优先级较 global 高。这种方式，每次进入目录，执行python命令运行脚本时，会自动使用设置的版本。而且不会影响全局环境
- `pyenv global 2.7.3`  设置全局的 Python 版本，通过将版本号写入 ~/.pyenv/version 文件的方式。这种方式会营销全局环境，要谨慎使用
- `pyenv rehash` 创建垫片路径（为所有已安装的可执行文件创建 shims，如：~/.pyenv/versions/*/bin/*，因此，每当你增删了 Python 版本或带有可执行文件的包（如 pip）以后，都应该执行一次本命令）

3. 虚拟环境管理

- `pyenv virtualenv 2.7.10 env-2.7.10` 创建虚拟环境，若不指定 python 版本，会默认使用当前环境 python 版本。如果指定 Python 版本，则一定要是已经安装过的版本，否则会出错。环境的真实目录位于 ~/.pyenv/versions 下
- `pyenv virtualenvs` 列出当前虚拟环境
- `pyenv activate env-name`   激活虚拟环境
- `pyenv deactivate` 退出虚拟环境，回到系统环境
- `pyenv uninstall my-virtual-env` 删除虚拟环境，或者直接删除目录`rm -rf ~/.pyenv/versions/env-name`

> 小技巧 pyenv切换版本，也可以使用虚拟环境，比如可以使用`pyenv local env-name`，来达到当前目录使用虚拟环境的目的。相比`pyenv activate env-name`更加方便，每次进入目录自动切换版本。

## 2. Python虚拟环境管理

### pyenv virtualenv

用来创建和管理虚拟环境，让不同的项目拥有独立的运行环境，互不干扰。

前面提到 pyenv 要解决的是多个 Python 的版本管理问题，virtualenv 要解决的是同一个库的版本管理问题。但如果两个问题都需要解决呢？分别使用不同工具就很麻烦了，而且容易有冲突。为此，pyenv 引入了 virtualenv 插件，可以在 pyenv 中解决同一个库的版本管理问题。

通过 pyenv virtualenv 命令，可以与 virtualenv 类似的创建、使用虚拟环境。但由于 pyenv 的垫片功能，使用虚拟环境跟使用 Python 版本的体验一样，**不需要手动执行 activate 和 deactivate，只要进入目录即生效，离开目录即失效**。

pyenv virtualenv 的用法和 pyenv 类似（使用上述安装 pyenv 方法会自动安装 virtualenv 插件）：

```cmd
# 分别安装基于 Python 2.7.17 和 Python 3.8.2 的虚拟环境
pyenv virtualenv 2.7.17 venv2
pyenv virtualenv 3.8.2 venv3
# 指定全局使用虚拟环境 venv2
pyenv global venv2
# 指定 myproject 使用虚拟环境 venv3
cd myproject
pyenv local venv3
# 在当前 shell 中临时使用虚拟环境 venv3
pyenv shell venv3
```

### virtualenv（不建议用，会有问题）

virtualenv解决的是同一个库不同版本需要在不同项目中共存的兼容问题。为此，virtualenv的解决方案是为每个项目创建一个独立的虚拟环境，在每个虚拟环境中安装对应的第三方库和相应的版本，对其他虚拟环境没有影响。

virtualenv的使用方法：

```cmd
# 安装 virtualenv
pip install virtualenv
# 创建虚拟环境 myenv
virtualenv /path/to/myenv
# 切换到虚拟环境 myenv，此时命令提示符前会有 (myenv) 提示符
cd /path/to/myenv/ && source bin/activate
# 安装库，安装到 /path/to/myenv/lib 中，不会与其他虚拟环境冲突
pip install requests
# 执行 python 相关命令
python demo.py
# 退出虚拟环境
deactivate
```

### pyvenv(venv，不建议用)

pyvenv与virtualenv功能和用法类似，不同点在于：

1. pyvenv 只支持 Python 3.3 及更高版本，而 virtualenv 同时支持 Python 2.x 和 Python 3.x
2. pyvenv 是 Python 3.x 自带的工具，不需要安装，而 virtualenv 是第三方工具，需要安装

pyvenv 实际上是 Python 3.x 的一个模块 venv，等价于 python -m venv。

pyvenv的用法：

```cmd
# 创建虚拟环境 myenv
pyvenv /path/to/myenv
# 或者
python -m venv /path/to/myenv
# 切换到虚拟环境 myenv，此时命令提示符前会有 (myenv) 提示符
cd /path/to/myenv/ && source bin/activate
# 安装库，安装到 /path/to/myenv/lib 中，不会与其他虚拟环境冲突
pip install requests
# 执行 python 相关命令
python demo.py
# 退出虚拟环境
deactivate
```

## pipenv

pipenv有两个功能：一是管理依赖包（替代pip管理工具），二是创建虚拟环境

![img](https://miro.medium.com/max/875/1*wmfcor4t8qlD9ASawFb8Sw.png)

![img](https://miro.medium.com/max/875/1*uSqdQ-jGyhgIXzl456qqjg.png)

![img](https://miro.medium.com/max/875/1*0u6Adynt2W8wIX-0WD6vFQ.png)