# unittest模块

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

   - 定义测试用例(可用一个类里不同测试函数，作为测试用例，测试用例必须以`test`开头) 继承自unittest.TestCase

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

       ```Python
       0 (静默模式): 你只能获得总的测试用例数和总的结果 比如 总共100个 失败20 成功80
       1 (默认模式): 非常类似静默模式 只是在每个成功的用例前面有个“.” 每个失败的用例前面有个 “F”
       2 (详细模式):测试结果会显示每个测试用例的所有相关的信息
       并且 你在命令行里加入不同的参数可以起到一样的效果
       加入 --quiet 参数 等效于 verbosity=0
       加入--verbose参数等效于 verbosity=2
       什么都不加就是 verbosity=1

       ```

   - `runner.run(suite)`   #运行模块

3. unittest中的setUp和tearDown的应用

    `setUp tearDown方法在前面加上类方法装饰器，可以在类初始化，结束后运行`

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
        def setUp(self):  #测试用例执行前运行，每一个用例执行前都会运行
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

4. 参数化的unittest

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

5. 通过文件实现参数化

    使用parameterized进行扩展 `@parameterized.expand(redCvs('a.txt'))`

6. 批量读取测试用例——在不同的脚本文件对应不同的测试用例的时候

    方法一： `unittest.TestLoader().loadTestsFromTestCase()  # 直接从类里读取`

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

7. 命令行中的unittest

    - 命令行中运行可以指定模块、类、独立测试方法进行测试，使用方法如下：
  
        ```Python
        python -m unittest test_module1 test_module2
        python -m unittest test_module.TestClass
        python -m unittest test_module.TestClass.test_method

        ```

    - 获得更详细的信息：`python -m unittest -v test_module`
    - 探索性测试：`python -m unittest`等价于`python -m unittest discover`
    - 获取命令行选项列表：`python -m unittest -h`
    - 其他命令行选项：
        - -b, --buffer 在测试运行时，标准输出流与标准错误流会被放入缓冲区。**成功的测试的运行时输出会被丢弃；测试不通过时，测试运行中的输出会正常显示，错误会被加入到测试失败信息**。

        - -c, --catch 当测试正在运行时， Control-C 会等待当前测试完成，并在完成后报告已执行的测试的结果。当再次按下 Control-C 时，引发平常的 KeyboardInterrupt 异常。

        - -f, --failfast 当出现第一个错误或者失败时，停止运行测试。

        - -k 只运行匹配模式或子串的测试方法和类。可以多次使用这个选项，以便包含匹配子串的所有测试用例。

            包含通配符（*）的模式使用 fnmatch.fnmatchcase() 对测试名称进行匹配。另外，**该匹配是大小写敏感的**。

            模式对测试加载器导入的测试方法全名进行匹配。

            例如，-k foo 可以匹配到 foo_tests.SomeTest.test_something 和 bar_tests.SomeTest.test_foo ，但是不能匹配到 bar_tests.FooTest.test_something 。

        - --locals 在回溯中显示局部变量。
    - 探索性测试（指在工程目录中进行测试搜索）

        若要使用探索性测试，所有的测试文件必须是 modules 或 packages （包括 namespace packages )并可从项目根目录导入（即它们的文件名必须是有效的 identifiers ）。

        探索性测试在 TestLoader.discover() 中实现（详见6-方法二），但也可以通过命令行使用。它在命令行中的基本用法如下：

        ```Shell
        cd project_directory
        python -m unittest discover

        ```

        探索性测试命令选项

        ```Shell

        -v, --verbose 更详细地输出结果。

        -s, --start-directory directory
        开始进行搜索的目录(默认值为当前目录 . )。

        -p, --pattern pattern
        用于匹配测试文件的模式（默认为 test*.py ）。

        -t, --top-level-directory directory
        指定项目的最上层目录（通常为开始时所在目录）。

        ```

      -s ，-p 和 -t 选项可以按顺序作为位置参数传入。以下两条命令是等价的：

        ```shell
        python -m unittest discover -s project_directory -p "*_test.py"
        python -m unittest discover project_directory "*_test.py"

        ```

    探索性测试通过导入测试对测试进行加载。在找到所有你指定的开始目录下的所有测试文件后，它把路径转换为包名并进行导入。如 foo/bar/baz.py 会被导入为 foo.bar.baz 。

    如果你有一个全局安装的包，并尝试对这个包的副本进行探索性测试，可能会从错误的地方开始导入。如果出现这种情况，测试会输出警告并退出。

    如果你使用包名而不是路径作为开始目录，搜索时会假定它导入的是你想要的目录，所以你不会收到警告。

8. 跳过测试与预计失败

    一些装饰器的使用方法：

    ```Python
    @unittest.skip(reason)
    跳过被此装饰器装饰的测试。 reason 为测试被跳过的原因。

    @unittest.skipIf(condition, reason)
    当 condition 为真时，跳过被装饰的测试。

    @unittest.skipUnless(condition, reason)
    跳过被装饰的测试，除非 condition 为真。

    @unittest.expectedFailure
    把测试标记为预计失败。如果测试不通过，会被认为测试成功；如果测试通过了，则被认为是测试失败。

    exception unittest.SkipTest(reason)
    引发此异常以跳过一个测试。
    '''
    通常来说，你可以使用 TestCase.skipTest() 或其中一个跳过测试的装饰器实现跳过测试的功能，而不是直接引发此异常。

    被跳过的测试的 setUp() 和 tearDown() 不会被运行。被跳过的类的 setUpClass() 和 tearDownClass() 不会被运行。被跳过的模组的 setUpModule() 和 tearDownModule() 不会被运行。
    '''

    ```
