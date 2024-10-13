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

    
