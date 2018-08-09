### unittest模块：

**TestCase** 测试用例，就是功能里那样一条条用例

**TestSuite** 多个测试用例集合在一起，就是TestSuite，就是一个功能模块的所有用例放这里了

**TestLoader** 是用来加载TestCase到TestSuite中的，这个方法很好玩，可以将一个目录下的所有python文件里的测试用例抠出来

**TestRunner** 是来执行测试用例的,测试的结果会保存到TestResult实例中，包括运行了多少测试用例，成功了多少，失败了多少等信息

1. 测试模块中常用的断言assert语句

```Python
# 用法为 
assert （表达式） # 如果正确就表示 pass  如果不正确则抛出一个assertioerror

```

2. 单元测试流程

- 导入相关模块 unittest, xmlrunner(生成报告的模块)

- 定义测试用例(可用一个类里不同测试函数，作为测试用例，测试用例必须以test开通) 继承自unittest.TestCase 
  ```Python
  class MyTest(unittest.TestCase):
      def testa(self):    #定义一个测试用例，注意，测试用例必须test开始，不然不会当做是测试用例
          '''a''' #描述，会同测试用例标题一并显示在测试报告里
          self.assertEqual(1,1)   #测试用例断言，比较预期结果与实际结果，这里1==1，显然结果是pass
      def testhaha(self):
          '''b'''
          self.assertEqual(2, 1)  #测试用例断言，比较预期结果与实际结果，这里2==1，显然结果是Fail
      def testb(self):
          '''c'''
          self.assertEqual(3, 2)  #测试用例断言，比较预期结果与实际结果，这里3==1，显然结果是Fail
  
  ``` 
  

- 定义测试套件  `suite=unittest.TestSuite()`

- 添加测试用例，两种方法，一是把类下的所有测试用例加入到测试套件中`suite.addTest(unittest.makeSuite(MyTest))` 其中MyTest为自定义的测试用例类；二是在测试套件里增加一个测试用例方法，`suite.addTest(MyTest('testa')`；**测试用例添加方法只能有一种，两种同时使用会抛出错误**

- `runner=xmlrunner.XMLTestRunner(output='.')` # 生成测试报告方法：**xmlrunner**，这个方法只要指定测试报告目录就可以

  方法2：runner = unittest.TextTestRunner(stream=Unbuffered(sys.stdout, 'TestReport.log') , verbosity=2)
  **使用unittest类自带的TextTestRunner函数，生成log文件**verbosity指定report的详细程度。
  
- `runner.run(suite)`   #运行模块

2. unittest中的setUp和tearDown的应用

```Python
#setUp和tearDown的运用
import unittest
class MyTest(unittest.TestCase):
    @classmethod
    def setUpClass(cls):  #类开始前运行，比如在执行这些用例之前需要备份数据库
        print('1')

    @classmethod
    def tearDownClass(cls):   #类结束后运行，比如在执行这些用例之后需要还原数据库
        print('0')
    def setUp(self):  #测试用例执行前运行
        print('a')
    def tearDown(self):   #测试用例执行后运行
        print('z')
    def testa(self):
        print('测试用例1')
        self.assertEqual(1,1)
    def testb(self):
        print('测试用例2')
        self.assertEqual(1,1)
if __name__=='__main__':
    unittest.main()
```
3. 参数化的unittest

通过parameterized扩展unittest的功能，使其可以一次性测试多个相似结构的case

```Python
import unittest,xmlrunner
from nose_parameterized import parameterized
def login(username,passwd):
    if username=='xiaogang' and passwd=='123456':
        return True
    else:
        return False

class Login(unittest.TestCase):
    @parameterized.expand(
        [
            ['xiaogang','123456',True], #可以是list，也可以是元祖
            ['','123456',True],
            ['xiaogang','',False],
            ['adgadg','123456',False]
        ]
    )
    def testlogin(self,username,passwd,exception): #这里的参数对应上述列表里的元素，运行的时候会遍历上述列表里的二维列表直到所有元素都调用运行完成
        '''登录'''
        print('test login')
        res=login(username,passwd)
        self.assertEqual(res,exception)

suite = unittest.TestSuite()
suite.addTest(unittest.makeSuite(Login))
runner=xmlrunner.XMLTestRunner(output='.') #生成测试报告方法：xmlrunner，这个方法只要指定测试报告目录就可以
runner.run(suite)

# 结果
test login
test login
test login
test login


Running tests...
----------------------------------------------------------------------
.F..
======================================================================
ERROR [0.001s]: testlogin_1_ (__main__.Login)
登录 [with username='', passwd='123456', exception=True]
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/usr/local/lib/python3.6/dist-packages/nose_parameterized/parameterized.py", line 392, in standalone_func
    return func(*(a + p.args), **p.kwargs)
  File "<ipython-input-20-8026ef0c8265>", line 22, in testlogin
AssertionError: False != True

----------------------------------------------------------------------
Ran 4 tests in 0.004s

FAILED (errors=1)

Generating XML reports...

```
因为通过parameterized扩展实现了参数化，对四个相似用例进行了测试，其中exception是期望输出；分析易知，四个之中有一个不符合期望的用例。

4. 通过文件实现参数化

使用parameterized进行扩展 `@parameterized.expand(redCvs('a.txt'))`

5. 批量读取测试用例 

  方法一： `unittest.TestLoader().loadTestsFromTestCase()  # 直接从类里读取 ` 
  
  方法二： 每个python文件里放不同的功能测试用例，方便维护，那么怎么把他们一次性运行完呢，这里就用到了unittest里的 defaultTestLoader.discover方法了
  
  ```Python
  import unittest,HTMLTestRunner
  suite = unittest.TestSuite()#定义测试集合
  all_case = unittest.defaultTestLoader.discover(
      r'E:\szg\bestTest\day11\AUTO\case','test_*.py'
  )#找到case目录下所有的.py文件

  for case in all_case:
      #循环添加case到测试集合里面
      suite.addTests(case)

  fw = open('report.html','wb')
  runner = HTMLTestRunner.HTMLTestRunner(
      stream=fw,title='多个文件运行'
  )
  runner.run(suite)
  ```
