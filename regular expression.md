### regular expression

####　字符串转义与正则转义:

涉及到\\的正则表达式：https://blog.csdn.net/jinixin/article/details/56705284

正则表达式（re）用于字符串的模式匹配的字符序列；描述了一种字符串匹配的模式（pattern），可以用来检查一个串是否含有某种子串、将匹配的子串替换或者从某个串中取出符合某个条件的子串等。

re 模块使 Python 语言拥有全部的正则表达式功能。

re 模块也提供了与这些方法功能完全一致的函数，这些函数使用一个模式字符串做为它们的第一个参数。

#### 匹配语法

正则表达式是由普通字符（例如字符 a 到 z）以及特殊字符（称为"元字符"）组成的文字模式。模式描述在搜索文本时要匹配的一个或多个字符串。正则表达式作为一个模板，将某个字符模式与所搜索的字符串进行匹配。

##### 普通字符
包括没有显式指定为元字符的所有可打印和不可打印字符。这包括所有大写和小写字母、所有数字、所有标点符号和一些其他符号。

w 元字符(\\w)用于查找单词字符。单词字符包括：a-z、A-Z、0-9，以及下划线, 包含 _ (下划线) 字符。

##### 非打印字符

非打印字符也可以是正则表达式的组成部分。下表列出了表示非打印字符的转义序列：

<table class="reference">
<tr>
	<th width="20%">字符</th>
	<th width="80%">描述</th>
</tr>
<tr>
	<td>\cx</td>
    <td>匹配由x指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。</td>
</tr>
<tr>
	<td>\f</td>
    <td>匹配一个换页符。等价于 \x0c 和 \cL。</td>
</tr>
<tr>
	<td>\n</td>
    <td>匹配一个换行符。等价于 \x0a 和 \cJ。</td>
</tr>
<tr>
	<td>\r</td>
    <td>匹配一个回车符。等价于 \x0d 和 \cM。</td>
</tr>
<tr>
	<td>\s</td>
    <td>匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。注意 Unicode 正则表达式会匹配全角空格符。</td>
</tr>
<tr>
	<td>\S</td>
    <td>匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。</td>
</tr>
<tr>
	<td>\t</td>
    <td>匹配一个制表符。等价于 \x09 和 \cI。</td>
</tr>
<tr>
	<td>\v</td>
    <td>匹配一个垂直制表符。等价于 \x0b 和 \cK。</td>
</tr>
</table>

##### 特殊字符

所谓特殊字符，就是一些有特殊含义的字符，如上面说的 runoo\*b 中的 \* ，简单的说就是表示任何字符串的意思。如果要查找字符串中的 \* 符号，则需要对 \* 进行转义，即在其前加一个 \\: runo\\\*ob 匹配 runo\*ob。

许多元字符要求在试图匹配它们时特别对待。若要匹配这些特殊字符，必须首先使字符"转义"，即，将反斜杠字符\\ 放在它们前面。下表列出了正则表达式中的特殊字符：

<table class="reference">
<tr>
	<th width="20%">特别字符</th>
	<th width="80%">描述</th>
</tr>
<tr>
	<td>$</td>
    <td>匹配输入字符串的结尾位置。如果设置了 RegExp 对象的 Multiline 属性，则 $ 也匹配 '\n' 或 '\r'。要匹配 $ 字符本身，请使用 \$。</td>
</tr>
<tr>
	<td>( )</td>
    <td>标记一个子表达式的开始和结束位置。子表达式可以获取供以后使用。要匹配这些字符，请使用\\\( 和 \\\)。</td>
</tr>
<tr>
	<td>\*</td>
    <td>匹配前面的子表达式零次或多次。要匹配 \* 字符，请使用 \*。</td>
</tr>
<tr>
	<td>+</td>
    <td>匹配前面的子表达式一次或多次。要匹配 + 字符，请使用 \+。</td>
</tr>
<tr>
	<td>.</td>
    <td>匹配除换行符 \n 之外的任何单字符。要匹配 . ，请使用 \. 。</td>
</tr>
<tr>
	<td>[</td>
    <td>标记一个中括号表达式的开始。要匹配 [，请使用 \[。</td>
</tr>
<tr>
	<td>?</td>
    <td>匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。要匹配 ? 字符，请使用 \?。</td>
</tr>
<tr>
	<td>\</td>
    <td>将下一个字符标记为或特殊字符、或原义字符、或向后引用、或八进制转义符。例如， 'n' 匹配字符 'n'。'\n' 匹配换行符。序列 '\\' 匹配 "\"，而 '\(' 则匹配 "("。</td>
</tr>
<tr>
	<td>^</td>
    <td>匹配输入字符串的开始位置，除非在方括号表达式中使用，此时它表示不接受该字符集合。要匹配 ^ 字符本身，请使用 \^。</td>
</tr>
<tr>
	<td>{</td>
    <td>标记限定符表达式的开始。要匹配 {，请使用 \{。</td>
</tr>
<tr>
	<td>|</td>
    <td>指明两项之间的一个选择。要匹配 |，请使用 \|。</td>
</tr>
</table>

##### 限定字符

限定符用来指定正则表达式的一个给定组件必须要出现多少次才能满足匹配。有 * 或 + 或 ? 或 {n} 或 {n,} 或 {n,m} 共6种。

正则表达式的限定符有：

<table class="reference">
<tr>
	<th width="20%">字符</th>
	<th width="80%">描述</th>
</tr>
<tr>
	<td>\*</td>
    <td>匹配前面的子表达式零次或多次。例如，zo\* 能匹配 "z" 以及 "zoo"。\* 等价于{0,}。</td>
</tr>
<tr>
	<td>+</td>
    <td>匹配前面的子表达式一次或多次。例如，'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。</td>
</tr>
<tr>
	<td>?</td>
    <td>匹配前面的子表达式零次或一次。例如，"do(es)?" 可以匹配 "do" 、 "does" 中的 "does" 、 "doxy" 中的 "do" 。? 等价于 {0,1}。</td>
</tr>
<tr>
	<td>{n}</td>
    <td>n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，但是能匹配 "food" 中的两个 o。</td>
</tr>
<tr>
	<td>{n,}</td>
    <td>n 是一个非负整数。至少匹配n 次。例如，'o{2,}' 不能匹配 "Bob" 中的 'o'，但能匹配 "foooood" 中的所有 o。'o{1,}' 等价于 'o+'。'o{0,}' 则等价于 'o*'。</td>
</tr>
<tr>
	<td>{n,m}</td>
    <td>m 和 n 均为非负整数，其中n &lt;= m。最少匹配 n 次且最多匹配 m 次。例如，"o{1,3}" 将匹配 "fooooood" 中的前三个 o。'o{0,1}' 等价于 'o?'。请注意在逗号和两个数之间不能有空格。</td>
</tr>
</table>

**、+限定符都是贪婪的，因为它们会尽可能多的匹配文字，只有在它们的后面加上一个?就可以实现非贪婪或最小匹配。**

##### 定位符

定位符使您能够将正则表达式固定到行首或行尾。它们能够创建出现在一个单词内、在一个单词的开头或者一个单词的结尾的正则表达式。

定位符用来描述字符串或单词的边界，^ 和 $ 分别指字符串的开始与结束，\\b 描述单词的前或后边界，\\B 表示非单词边界。

正则表达式的定位符有：

<table class="reference">
<tr>
	<th width="20%">字符</th>
	<th width="80%">描述</th>
</tr>
<tr>
	<td>^</td>
    <td>匹配输入字符串开始的位置。如果设置了 RegExp 对象的 Multiline 属性，^ 还会与 \\n 或 \\r 之后的位置匹配。</td>
</tr>
<tr>
	<td>$</td>
    <td>匹配输入字符串结尾的位置。如果设置了 RegExp 对象的 Multiline 属性，$ 还会与 \\n 或 \\r 之前的位置匹配。</td>
</tr>
<tr>
	<td>\\b</td>
    <td>匹配一个字边界，即字与空格间的位置。</td>
</tr>
<tr>
	<td>\\B</td>
    <td>非字边界匹配。</td>
</tr>
</table>

##### 一个实例
```
str = "http://www.runoob.com:80/html/html-tutorial.html";
patt1 = /(\w+):\/\/([^/:]+)(:\d*)?([^# ]*)/;
```
第一个括号子表达式捕获 Web 地址的协议部分。该子表达式匹配在冒号和两个正斜杠前面的任何单词。

第二个括号子表达式捕获地址的域地址部分(**域名**)。子表达式匹配 : 和 / 之后的一个或多个字符。

第三个括号子表达式捕获端口号（如果指定了的话）。该子表达式匹配冒号后面的零个或多个数字。只能重复一次该子表达式。

最后，第四个括号子表达式捕获 Web 地址指定的路径和 / 或页信息。该子表达式能匹配不包括 # 或空格字符的任何字符序列。

#### matchObj对象

re模块的函数返回的是matchObj对象, matchObj对象有group()方法

匹配对象方法 |描述
:--- | :---
group(num=0) | 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。**默认参数为0, 返回匹配到的字符串**
groups() |  返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。

- group([group1, …]) 方法用于获得一个或多个分组匹配的字符串，当要获得整个匹配的子串时，可直接使用 group() 或 group(0)；
- start([group]) 方法用于获取分组匹配的子串在整个字符串中的起始位置（子串第一个字符的索引），参数默认值为 0；
- end([group]) 方法用于获取分组匹配的子串在整个字符串中的结束位置（子串最后一个字符的索引+1），参数默认值为 0；
- span([group]) 方法返回 (start(group), end(group))。


实例

```Python
import re

line = "Cats are smarter than dogs"

matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)

if matchObj:
   print "matchObj.group() : ", matchObj.group()
   print "matchObj.group(1) : ", matchObj.group(1)
   print "matchObj.group(2) : ", matchObj.group(2)
else:
   print "No match!!"

>>>
    matchObj.group() :  Cats are smarter than dogs
    matchObj.group(1) :  Cats
    matchObj.group(2) :  smarter
```

#### re.match()函数

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

语法：`re.match(pattern, string, flags=0)`

参数说明

  参数 |描述
  :--- | :---
  pattern | 匹配的正则表达式
  string |  要匹配的字符串。
  flags | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。

flags的具体参数:
- re.I 忽略大小写
- re.L 表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境
- re.M 多行模式
- re.S 即为 . 并且包括换行符在内的任意字符（. 不包括换行符）
- re.U 表示特殊字符集 \w, \W, \b, \B, \d, \D, \s, \S 依赖于 Unicode 字符属性数据库
- re.X 为了增加可读性，忽略空格和 # 后面的注释


#### re.search()方法

re.search 扫描整个字符串并返回第一个成功的匹配。

语法：`re.search(pattern, string, flags=0)`

参数说明同re.match()

**re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。**

<font color=red size=3>实例</font>

```Python
import re
print(re.search('www', 'www.runoob.com').span())  # 在起始位置匹配
print(re.search('com', 'www.runoob.com').span())         # 不在起始位置匹配
>>>
	(0, 3)
	(11, 14)  #  如果是match()返回none
```

#### 检索与替换

Python 的 re 模块提供了re.sub用于替换字符串中的匹配项。

语法: `re.sub(pattern, repl, string, count=0, flags=0)`

**count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。**

实例:

```Python
import re

phone = "2004-959-559 # 这是一个国外电话号码"

# 删除字符串中的 Python注释
num = re.sub(r'#.*$', "", phone)
print ("电话号码是: ", num

# 删除非数字(-)的字符串
num = re.sub(r'\D', "", phone)  # \D匹配非数字 \d匹配数字
print ("电话号码是 : ", num)
```
**当repl参数是一个函数时**
**命名分组就是给具有默认分组编号的组另外再给一个别名**。命名分组的语法格式如下：

`(?P<name>正则表达式) # name是一个合法的标识符`

```Python
import re
# 将匹配的数字乘以 2
def double(matched):
    value = int(matched.group('value'))  # group函数可以接受分组命名作为参数
    return str(value * 2)

s = 'A23G4HFD567'
print(re.sub('(?P<value>\d+)', double, s))
```

#### re.compile() 函数

compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。

语法格式为： `re.compile(pattern[, flags])`

<font color=red size=3>实例</font>

```Python
>>>import re
>>> pattern = re.compile(r'\d+')                    # 用于匹配至少一个数字
>>> m = pattern.match('one12twothree34four')        # 查找头部，没有匹配
>>> print m
None
>>> m = pattern.match('one12twothree34four', 2, 10) # 从'e'的位置开始匹配，没有匹配
>>> print m
None
>>> m = pattern.match('one12twothree34four', 3, 10) # 从'1'的位置开始匹配，正好匹配
>>> print m                                         # 返回一个 Match 对象
<_sre.SRE_Match object at 0x10a42aac0>
>>> m.group(0)   # 可省略 0
'12'
>>> m.start(0)   # 可省略 0
3
>>> m.end(0)     # 可省略 0
5
>>> m.span(0)    # 可省略 0
(3, 5)
```

此外还有`re.findall re.finditer re.split`等函数,

### 最后

<font color=lightgreen size=5>由于正则表达式通常都包含反斜杠，所以最好使用原始字符串来表示它们。模式元素(如 r'\t'，等价于 '\\t')匹配相应的特殊字符。

</font>

所有内容参考自<http://www.runoob.com/python/python-reg-expressions.html>
