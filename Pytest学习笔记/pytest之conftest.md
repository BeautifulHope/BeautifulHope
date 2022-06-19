## 实例场景
首先们思考这样一个问题：如果我们在编写测试用的时候，每一个测试文件里面的用例都需要先登录后才能完成后面的操作，那么们该如何实现呢？这就需要我们掌握conftest.py文件的使用了。
## 代码
目录结构：
```
UserConftest
    |conftest.py
    |test_file_01.py
    |test_file_02.py
    |__init__.py
```
conftest.py：
```
# conftest.py
import pytest
@pytest.fixture()
def fake_login():　　
　　print('\n---------------conftest文件login方法开始执行----------------------------')
　　print('login in conftest.py')
　　print('----------------conftest.py文件login方法执行结束---------------------------')
```
demo1：
```
# demo1.py
class Testattempt():

    def test_one(self,fake_login):
        assert 23 == 23
    def test_two(self,fake_login):
        assert 0.1 + 0.2 == approx(0.3)
```
demo2：
```
# demo2.py
def test_with_global_fixture(fake_login):
    """
    Fixtures can also be shared across test files (see conftest.py)
    """
    print("Running test_with_global_fixture...")
```

cmd输入：`pytest -v -s # demo1.py.py::Testattempt`

输出：
```

---------------conftest文件login方法开始执行----------------------------
login in conftest.py
----------------conftest.py文件login方法执行结束---------------------------
PASSED
03_simple_fixture_test.py::Testattempt::test_two
 autouse = True


---------------conftest文件login方法开始执行----------------------------
login in conftest.py
----------------conftest.py文件login方法执行结束---------------------------
PASSED

```
每个测试文件的测试用例执行前都执行了conftest.py文件中的fake_login方法，通过这种模式我们就可以实现测试用例前置条件的准备工作了！
## conftest文件与fixture
conftest文件实际应用中需要结合fixture来使用，那么fixture中参数scope也适用conftest中fixture的特性，这里再说明一下
1. conftest中fixture的scope参数为`session`，那么所有的测试文件执行前执行一次

2. conftest中fixture的scope参数为`module`，那么每一个测试文件执行前都会执行一次conftest文件中的fixture

3. conftest中fixture的scope参数为`class`，那么每一个测试文件中的测试类执行前都会执行一次conftest文件中的fixture

4. conftest中fixture的scope参数为`function`，那么所有文件的测试用例执行前都会执行一次conftest文件中的fixture
   
## Summary
实际工作中不仅仅只有函数使用，也往往不仅存在一个conftest.py文件。
1. conftest.py文件名字是固定的，不可以做任何修改

2. 文件和用例文件在同一个目录下，那么conftest.py作用于整个目录

3. conftest.py文件所在目录必须存在__init__.py文件

4. conftest.py文件不能被其他文件导入

5. 所有同目录测试文件运行前都会执行conftest.py文件