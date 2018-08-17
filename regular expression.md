### regular expression

正则表达式（re）用于字符串的模式匹配的字符序列；描述了一种字符串匹配的模式（pattern），可以用来检查一个串是否含有某种子串、将匹配的子串替换或者从某个串中取出符合某个条件的子串等。

re 模块使 Python 语言拥有全部的正则表达式功能。

compile 函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象。该对象拥有一系列方法用于正则表达式匹配和替换。

re 模块也提供了与这些方法功能完全一致的函数，这些函数使用一个模式字符串做为它们的第一个参数。

#### 匹配语法

##### 普通字符
包括没有显式指定为元字符的所有可打印和不可打印字符。这包括所有大写和小写字母、所有数字、所有标点符号和一些其他符号。

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



#### matchObj对象

re模块的函数返回的是matchObj对象, matchObj对象有group()方法

匹配对象方法 |描述
:--- | :---
group(num=0) | 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。
groups() |  返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。

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

#### re.search()方法
