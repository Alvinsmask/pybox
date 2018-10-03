### getopt

该模块是专门用来处理命令行参数的

函数用法格式：getopt.getopt(args, options[, long_options])

args：命令行参数，一般是sys.argv[1:]，0为脚本本身的名字；

options：shortopts 短格式（“-”）

long_options：longopts 长格式（“--”）

**命令行示例：python config.py -h -d 13 -c allow --help**

```
def getopttest():
    try:
        options,args = getopt.getopt(sys.argv[1:],"d:c:hv",["domain=","cache=","help","version"])
    except getopt.GetoptError as err:
        print str(err)
```

getopt.GetoptError为getopt模块函数异常错误，这里捕获该异常并打印出相关信息等。

sys.argv[1:]为获取到的命令行参数，赋值给options，options变量在getopt分析完后实际包含两个值，参数和参数值，
args值为不属于getopt函数分析内的参数和参数值，例如python config.py -d 13 aaa,则aaa为args变量。

对于短格式:

“d:c:hv”: 此为短格式，“：”表示该参数后面需要加参数值，不加冒号使用时则无需添加参数值，例如：-d 13。

["domain=","cache=","help","version"]: 此为长格式，
长格式参数后面跟随等号即“=”表示该长格式参数后面需要添加参数值，不加等号则使用时无需添加参数值，例如：--cache allow。

**然后:**

如上面解释的一个命令行例子为： 
'-h -o file --help --output=out file1 file2'

在分析完成后，opts 应该是： 
[('-h', ''), ('-o', 'file'), ('--help', ''), ('--output', 'out')]

而args 则为： 
['file1', 'file2']

接下来主要是对分析出的参数进行判断是否存在，然后再进一步处理
