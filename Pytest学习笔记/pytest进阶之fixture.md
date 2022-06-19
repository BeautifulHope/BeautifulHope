# pytest进阶之fixture

## fixture用途

1.做测试前后的初始化设置，如测试数据准备，链接数据库，打开浏览器等这些操作都可以使用fixture来实现

2.测试用例的前置条件可以使用fixture实现

3.支持经典的xunit fixture ，像unittest使用的setup和teardown

4.fixture可以实现unittest不能实现的功能，比如unittest中的测试用例和测试用例之间是无法传递参数和数据的，但是fixture却可以解决这个问题

## fixture定义

fixture通过@pytest.fixture()装饰器装饰一个函数，那么这个函数就是一个fixture，看个实例
```
@pytest.fixture
def local_fixture():
    """
    Fixtures are callables decorated with @fixture
    """
    print("\n(Doing Local Fixture setup stuff!--------------)")
    start = time.time()
    yield
    print("\ndurate time:", time.time() - start)
#1
def test_with_local_fixture(local_fixture):
    """
    Fixtures can be invoked simply by having a positional arg
    with the same name as a fixture:
    """
    print("Running test_with_local_fixture...")
    assert True
#2
@pytest.mark.usefixtures('local_fixture')
def test_user_decorate_for_fixture():
    print('233333')
#3
@pytest.fixture(autouse=True)
def local_fixture_autouse():
    print("\n autouse = True \n")
```
输出：
```

03_simple_fixture_test.py::test_with_local_fixture
 autouse = True


(Doing Local Fixture setup stuff!--------------)
Running test_with_local_fixture...
PASSED
durate time: 0.002992391586303711

03_simple_fixture_test.py::test_user_decorate_for_fixture
 autouse = True


(Doing Local Fixture setup stuff!--------------)
233333
PASSED
durate time: 0.0009856224060058594

```

## fixture使用

调用fixture有三种方式
Fixture名字作为用例的参数

1. fixture的名字直接作为测试用例的参数，上面的实例就这这种方式，再来看一个实例
```
# test_fixture.py

import pytest

@pytest.fixture()
def fixtureFunc():
    return 'fixtureFunc'

def test_fixture(fixtureFunc):
    print('我调用了{}'.format(fixtureFunc))

class TestFixture(object):
    def test_fixture_class(self, fixtureFunc):
        print('在类中使用fixture "{}"'.format(fixtureFunc))

if __name__=='__main__':
    pytest.main(['-v', 'test_fixture.py'])
```
2. 使用`@pytest.mark.usefixtures('fixture')`装饰器

每个函数或者类前使用@pytest.mark.usefixtures('fixture')装饰器装饰

实例
```
# test_fixture.py
import pytest
@pytest.fixture()
def fixtureFunc():
    print('\n fixture->fixtureFunc')

@pytest.mark.usefixtures('fixtureFunc')
def test_fixture():
    print('in test_fixture')

@pytest.mark.usefixtures('fixtureFunc')
class TestFixture(object):
    def test_fixture_class(self):
        print('in class with text_fixture_class')

if __name__=='__main__':
    pytest.main(['-v', 'test_fixture.py'])
```
3. 使用autouse参数

指定fixture的参数`autouse=True`这样每个测试用例会自动调用fixture(其实这里说的不是很准确，因为还涉及到fixture的作用范围，那么我们这里默认是函数级别的，后面会具体说fixture的作用范围)

实例
```
# test_fixture.py
import pytest
@pytest.fixture(autouse=True)
def fixtureFunc():
    print('\n fixture->fixtureFunc')

def test_fixture():
    print('in test_fixture')

class TestFixture(object):
    def test_fixture_class(self):
        print('in class with text_fixture_class')

if __name__=='__main__':
    pytest.main(['-v', 'test_fixture.py'])
```
结果
```
 fixture->fixtureFunc
.in test_fixture

 fixture->fixtureFunc
.in class with text_fixture_class
                                                       [100%]

========================== 2 passed in 0.04 seconds ===========================
```
从结果可以看到每个测试用例执行前都自动执行了fixture
>三种方式的区别：如果测试用例需要使用fixture中返回的参数，那么通过后面这两种方式是无法使用返回的参数的，因为fixture中返回的数据默认存在fixture名字里面存储，所以只能使用第一种方式才可以调用fixture中的返回值。(理论永远是理论，看文章的老铁还是自己试试吧！)

# fixtur作用范围

上面所有的实例默认都是函数级别的，所以测试函数只要调用了fixture，那么在测试函数执行前都会先指定fixture。说到作用范围就不得不说fixture 的第二个参数scope参数。

scope参数可以是`session， module，class，function`； 默认为`function`

1. session 会话级别（通常这个级别会结合conftest.py文件使用，所以后面说到conftest.py文件的时候再说）

2. module 模块级别： 模块里所有的用例执行前执行一次module级别的fixture

3. class 类级别 ：每个类执行前都会执行一次class级别的fixture

4. function ：前面实例已经说了，这个默认是默认的模式，函数级别的，每个测试用例执行前都会执行一次function级别的fixture

下面我们通过一个实例具体看一下 fixture的作用范围
```
# test_fixture.py
import pytest

@pytest.fixture(scope='module', autouse=True)
def module_fixture():
    print('\n-----------------')
    print('我是module fixture')
    print('-----------------')
@pytest.fixture(scope='class')
def class_fixture():
    print('\n-----------------')
    print('我是class fixture')
    print('-------------------')
@pytest.fixture(scope='function', autouse=True)
def func_fixture():
    print('\n-----------------')
    print('我是function fixture')
    print('-------------------')

def test_1():
    print('\n 我是test1')

@pytest.mark.usefixtures('class_fixture')
class TestFixture1(object):
    def test_2(self):
        print('\n我是class1里面的test2')
    def test_3(self):
        print('\n我是class1里面的test3')
@pytest.mark.usefixtures('class_fixture')
class TestFixture2(object):
    def test_4(self):
        print('\n我是class2里面的test4')
    def test_5(self):
        print('\n我是class2里面的test5')

if __name__=='__main__':
    pytest.main(['-v', '--setup-show', 'test_fixture.py'])
```
我们在cmd里面执行使用 --setup-show 可以查看到具体setup和teardoen顺序:  
运行结果
```
est_fixture.py 
    SETUP    M module_fixture
        SETUP    F func_fixture
-----------------
我是module fixture
-----------------

-----------------
我是function fixture
-------------------

        test_fixture.py::test_1 (fixtures used: func_fixture, module_fixture).
 我是test1

        TEARDOWN F func_fixture
      SETUP    C class_fixture
        SETUP    F func_fixture
-----------------
我是class fixture
-------------------

-----------------
我是function fixture
-------------------

        test_fixture.py::TestFixture1::test_2 (fixtures used: class_fixture, func_fixture, module_fixture).
我是class1里面的test2

        TEARDOWN F func_fixture
        SETUP    F func_fixture
-----------------
我是function fixture
-------------------

        test_fixture.py::TestFixture1::test_3 (fixtures used: class_fixture, func_fixture, module_fixture).
我是class1里面的test3

        TEARDOWN F func_fixture
      TEARDOWN C class_fixture
      SETUP    C class_fixture
        SETUP    F func_fixture
-----------------
我是class fixture
-------------------

-----------------
我是function fixture
-------------------

        test_fixture.py::TestFixture2::test_4 (fixtures used: class_fixture, func_fixture, module_fixture).
我是class2里面的test4

        TEARDOWN F func_fixture
        SETUP    F func_fixture
-----------------
我是function fixture
-------------------

        test_fixture.py::TestFixture2::test_5 (fixtures used: class_fixture, func_fixture, module_fixture).
我是class2里面的test5

        TEARDOWN F func_fixture
      TEARDOWN C class_fixture
    TEARDOWN M module_fixture

========================== 5 passed in 0.05 seconds ===========================
```
# fixture实现teardown

其实前面的所有实例都只是做了测试用例执行之前的准备工作，那么用例执行之后该如何实现环境的清理工作呢？这不得不说`yield`关键字了，相比大家都或多或少的知道这个关键字，他的作用其实和return差不多，也能够返回数据给调用者，**唯一的不同是被掉函数执行遇到yield会停止执行，接着执行调用处的函数，调用出的函数执行完后会继续执行yield关键后面的代码**（具体原理可以看下我之前的文章关于生成器）。看下下面的实例来了解一下如何实现teardown功能
```
import pytest
from selenium import webdriver
import time
@pytest.fixture()
def fixtureFunc():
　　'''实现浏览器的打开和关闭'''
    driver = webdriver.Firefox()
    yield driver
    driver.quit()
def test_search(fixtureFunc):
    '''访问百度首页，搜索pytest字符串是否在页面源码中'''
    driver = fixtureFunc
    driver.get('http://www.baidu.com')
    driver.find_element_by_id('kw').send_keys('pytest')
    driver.find_element_by_id('su').click()
    time.sleep(3)
    source = driver.page_source
    assert 'pytest' in source
    
if __name__=='__main__':
    pytest.main(['--setup-show', 'test_fixture.py'])
```
这个实例会先打开浏览器，然后执行测试用例，最后关闭浏览器。大家可以试试!  通过yield就实现了 用例执行后的teardown功能

# 总结

1. fixture如何定义
2. fixture的使用方式
3. fixture作用范围
4. fixture用yield实现teardown功能

>最后提一句：实际工作中尽量少用`auto=True`这个参数，可能会引发意想不到的结果！ 最常用的还是通过传递参数最好!