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
    
- `runner.run(suite)`   #运行模块
