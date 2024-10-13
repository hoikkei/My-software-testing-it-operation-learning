## pytest 命令规则

### 1. 运行基本的测试
运行所有测试：

 ```bash
pytest
 ```
该命令会默认在当前目录及子目录下递归地查找以 test_ 开头或 _test 结尾的文件并执行其中的测试用例。

指定某个测试文件：

 ```bash
pytest test_sample.py
 ```
仅运行指定文件中的测试用例。

指定目录：

 ```bash
pytest tests/
 ```
运行 tests/ 目录下的所有测试文件。

运行某个测试函数：

 ```bash
pytest test_sample.py::test_addition
 ```
仅运行 test_sample.py 文件中的 test_addition 测试函数。

### 2. 运行测试时的常用选项
显示详细的输出：

 ```bash
pytest -v
 ```
显示每个测试用例的详细信息，包括测试函数的名称、状态（通过、失败等）和输出。

显示测试失败的详细信息：

 ```bash
pytest --tb=short
pytest --tb=long
pytest --tb=auto
pytest --tb=none
--tb=short：简洁的回溯信息。
--tb=long：详细的回溯信息。
--tb=auto：自动选择合适的回溯格式。
--tb=none：不显示回溯。
 ```
只运行失败的测试：

 ```bash
pytest --last-failed
 ```
仅运行上一次运行时失败的测试用例。

停止运行测试（N 次失败后退出）：

 ```bash
pytest --maxfail=2
 ```
该选项会在出现 2 次失败后停止运行。

禁用警告信息：

 ```bash
pytest --disable-warnings
 ```
禁用警告信息的输出。

运行标记的测试：

 ```bash
pytest -k "expression"
 ```
运行符合指定表达式的测试。例如，运行包含 addition 的测试函数：

 ```bash
pytest -k "addition"
 ```
运行带有特定标记的测试：

 ```bash
pytest -m "slow"
 ```
仅运行标记为 slow 的测试用例。

跳过某些测试：

 ```bash
pytest --skip
 ```
跳过标记为 @pytest.mark.skip 的测试。

### 3. 测试输出与日志
保存日志信息：

 ```bash
pytest --log-cli-level=INFO
 ```
配置日志输出级别，支持 DEBUG, INFO, WARNING, ERROR, CRITICAL 等。

保存测试报告到文件：

 ```bash
pytest --html=report.html
 ```
将测试结果保存为 HTML 格式的报告。

保存 JUnit 格式的测试报告：

 ```bash
pytest --junitxml=report.xml
 ```
生成一个 JUnit 格式的 XML 报告，通常用于与 CI/CD 工具集成。

### 4. 运行特定配置的测试
指定测试配置文件（例如 pytest.ini）：
 ```bash
pytest --confcutdir=path/to/config
 ```
仅加载位于指定目录及其子目录中的配置文件。

### 5. 运行前置和后置操作
运行测试前/后执行的操作：
 ```bash
pytest --setup-show
 ```
在测试执行前，显示夹具的设置过程。
常用 pytest 选项总结：
选项	描述

 ```
pytest	运行当前目录下所有测试文件
pytest -v	显示详细输出
pytest --tb=short	显示简洁的回溯信息
pytest --tb=long	显示详细的回溯信息
pytest --disable-warnings	禁用警告信息
pytest --maxfail=3	当失败次数超过指定次数时停止运行
pytest --last-failed	只运行上次运行失败的测试
pytest -k "expression"	运行符合指定表达式的测试
pytest --html=report.html	生成 HTML 格式的测试报告
pytest --junitxml=report.xml	生成 JUnit 格式的 XML 测试报告
pytest --log-cli-level=INFO	配置日志级别为 INFO
pytest --setup-show	显示每个夹具的设置过程

 ```



## pytest 测试用例结构

### 1. 基本的测试用例结构
一个最简单的 pytest 测试用例通常是一个以 test_ 开头的函数。在该函数中，你可以使用 assert 语句来进行断言检查。示例如下：

 ```python
# test_sample.py

def test_addition():
    result = 2 + 2
    assert result == 4
 ```
测试函数命名：测试函数的名称应该以 test_ 开头，pytest 会自动识别以 test_ 开头的函数为测试用例。
断言：assert 语句用于断言条件，如果条件不成立，pytest 会抛出异常，测试用例会失败。

### 2. 测试类
可以将多个测试方法组织在一个类中。虽然不是必须的，但将相关的测试用例组织成类可以提高代码的可读性和可维护性：

 ```python
class TestMathOperations:
    def test_addition(self):
        assert 2 + 2 == 4

    def test_subtraction(self):
        assert 5 - 3 == 2
 ```
pytest 允许将测试方法放在类中，但类名不需要以 Test 结尾，pytest 会自动识别。
类中的每个方法都应以 test_ 开头。

### 3. 组织测试用例
可以使用模块、类、函数来组织测试用例。通常会将测试用例分为多个文件和目录，每个测试文件可以独立运行。

测试文件结构示例：

 ```markdown
tests/
    test_math_operations.py
    test_string_operations.py
    test_data_processing.py
 ```
    
## pytest 测试用例断言
断言是一种在程序中的一阶逻辑(如:一个结果为真或假的逻辑判断式),目的是为了表示与验证软件开发者预期的结果.但程序执行到断言的位置时,对应的断言应该为真.若断言不为真,程序会中止执行,并给出错误信息.

断言是测试用例的核心部分，pytest 提供了丰富的断言机制来判断测试结果是否符合预期。以下是常用的断言方式：

### 1. 基本断言
相等断言：
 ```python
def test_addition():
    assert 2 + 2 == 4
 ```
不等断言：
 ```python
def test_subtraction():
    assert 5 - 3 != 3
 ```
真值断言：
 ```python
def test_truth():
    assert 3 > 2
 ```
    
### 2. 使用 assert 语句结合 pytest 提供的比较方法
近似断言：
 ```python
def test_float_comparison():
    assert round(0.1 + 0.2, 1) == 0.3
 ```
包含断言（检查某个元素是否在列表中）：
 ```python
def test_element_in_list():
    assert 2 in [1, 2, 3]
 ```
抛出异常断言：
 ```python
import pytest

def test_raises_exception():
    with pytest.raises(ZeroDivisionError):
        1 / 0
 ```
pytest 测试框架结构
pytest 的测试框架一般包括以下几个部分：

### 1. 测试文件
测试文件名通常以 test_ 开头，以确保 pytest 可以自动识别这些文件为测试文件。一个典型的测试文件结构如下：
 ```
tests/
│
├── test_module1.py
├── test_module2.py
└── test_helpers.py
 ```
### 2. 测试目录
通常，测试文件会放在一个专门的测试目录下，目录名称一般为 tests，以帮助组织和区分生产代码和测试代码。

 ```bash
project_root/
│
├── src/         # 源代码
│   └── module1.py
│   └── module2.py
│
└── tests/       # 测试代码
    ├── test_module1.py
    ├── test_module2.py
    └── test_helpers.py
 ```
### 3. 夹具（Fixtures）
在 pytest 中，夹具（fixtures）用于提供测试的前置条件，减少测试用例重复的代码。夹具可以是测试数据、模拟对象、数据库连接等。

 ```python
import pytest

@pytest.fixture
def sample_data():
    return {"name": "Alice", "age": 30}

def test_name(sample_data):
    assert sample_data["name"] == "Alice"

 ```
夹具是通过 @pytest.fixture 装饰器定义的。
通过将夹具的名称传递给测试函数，pytest 会自动注入相应的夹具数据。

### 4. 配置文件
pytest 支持通过配置文件来设置一些全局选项，如 pytest.ini、tox.ini 或 setup.cfg。这些文件可以定义一些默认行为，比如测试标记、测试路径、日志记录等。

一个典型的配置文件 pytest.ini 例子：

 ```ini
[pytest]
testpaths = tests
addopts = --maxfail=3 --disable-warnings
 ```
testpaths：定义 pytest 搜索测试文件的路径。
addopts：传递给 pytest 的额外命令行选项，如最大失败次数 --maxfail 和禁用警告 --disable-warnings。

### 5. 标记和跳过
标记测试用例：可以通过 @pytest.mark 来标记某些测试用例，后续可以选择性地运行这些测试。
 ```python
import pytest

@pytest.mark.slow
def test_long_running():
    assert 1 == 1
 ```

跳过测试：使用 @pytest.mark.skip 跳过某个测试。

 ```python
@pytest.mark.skip(reason="Test not ready")
def test_not_implemented():
    assert False
 ```
条件跳过：如果某些条件不满足，可以跳过测试。

 ```python
@pytest.mark.skipif(condition=True, reason="Condition not met")
def test_conditional_skip():
    assert False
 ```

    
