# Python3 正则表达式

正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。

re 模块使 Python 语言拥有全部的正则表达式功能。 

re 模块也提供了与这些方法功能完全一致的函数，这些函数使用一个模式字符串做为它们的第一个参数。

## 正则表达式模式

模式字符串使用特殊的语法来表示一个正则表达式：

字母和数字表示他们自身。一个正则表达式模式中的字母和数字匹配同样的字符串。

多数字母和数字前加一个反斜杠时会拥有不同的含义。

标点符号只有被转义时才匹配自身，否则它们表示特殊的含义。

反斜杠本身需要使用反斜杠转义。

|模式|描述|
|----|----|
|^|匹配字符串的开头|
|$|匹配字符串的末尾|
|.|匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符|
|[...]|用来表示一组字符,单独列出：[amk] 匹配 'a'，'m'或'k'|
|[^...]|不在[]中的字符：[^abc] 匹配除了a,b,c之外的字符|
|*|匹配0个或多个的表达式|
|+|匹配1个或多个的表达式|
|?|配0个或1个由前面的正则表达式定义的片段，非贪婪方式|
|{ n,}|精确匹配n个前面表达式|
| n, m|匹配 n 到 m 次由前面的正则表达式定义的片段，贪婪方式|
|a| b|匹配a或b|
|()|匹配括号内的表达式，也表示一个组|
|\w|匹配字母数字|
|\W|匹配非字母数字|
|\s|匹配任意空白字符，等价于 [\t\n\r\f]|
|\S|匹配任意非空字符|
|\d|匹配任意数字，等价于 [0-9]|
|\D|匹配任意非数字|


## 正则表达式实例 

|实例|描述|
|----|----|
|python|匹配 "python"|
|[Pp]ython |匹配 "Python" 或 "python"|
|rub[ye]|匹配 "ruby" 或 "rube"|
|[aeiou]|匹配中括号内的任意一个字母|
|[0-9]|匹配任何数字。类似于 [0123456789]|
|[a-z]|匹配任何小写字母|
|[A-Z]|匹配任何大写字母|
|[a-zA-Z0-9]|匹配任何字母及数字|
|[^aeiou]|除了aeiou字母以外的所有字符|
|[^0-9]|匹配除了数字外的字符 |


## re.match函数

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

函数语法：

	re.match(pattern, string, flags=0)

函数参数说明：

|参数|描述|
|----|----|
|pattern|匹配的正则表达式|
|string|要匹配的字符串|
|flags|标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等|

匹配成功re.match方法返回一个匹配的对象，否则返回None。

我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

|匹配对象方法|描述|
|----|----|
|group(num=0)|	匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组|
|groups()|返回一个包含所有小组字符串的元组，从 1 到 所含的小组号|


**实例1**

	#!/usr/bin/python
	 
	import re
	print(re.match('www', 'www.runoob.com').span())  # 在起始位置匹配
	print(re.match('com', 'www.runoob.com'))         # 不在起始位置匹配


**实例2**

	import re
	 
	line = "Cats are smarter than dogs"
	 
	matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)
	 
	if matchObj:
	   print ("matchObj.group() : ", matchObj.group())
	   print ("matchObj.group(1) : ", matchObj.group(1))
	   print ("matchObj.group(2) : ", matchObj.group(2))
	else:
	   print ("No match!!")


## re.search方法

re.search 扫描整个字符串并返回第一个成功的匹配。

函数语法：

	re.search(pattern, string, flags=0)

函数参数说明：

|参数|描述|
|----|----|
|pattern|匹配的正则表达式|
|string|要匹配的字符串|
|flags|标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等|

匹配成功re.search方法返回一个匹配的对象，否则返回None。

我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式


|匹配对象方法|描述|
|----|----|
|group(num=0)|	匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组|
|groups()|返回一个包含所有小组字符串的元组，从 1 到 所含的小组号|

**实例1**
	import re
	 
	print(re.search('www', 'www.runoob.com').span())  # 在起始位置匹配
	print(re.search('com', 'www.runoob.com').span())  # 不在起始位置匹配


**实例2**
	import re
	 
	line = "Cats are smarter than dogs";
	 
	searchObj = re.search( r'(.*) are (.*?) .*', line, re.M|re.I)
	 
	if searchObj:
	   print ("searchObj.group() : ", searchObj.group())
	   print ("searchObj.group(1) : ", searchObj.group(1))
	   print ("searchObj.group(2) : ", searchObj.group(2))
	else:
	   print ("Nothing found!!")


## re.match与re.search的区别

re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

**实例1**

	import re
	 
	line = "Cats are smarter than dogs";
	 
	matchObj = re.match( r'dogs', line, re.M|re.I)
	if matchObj:
	   print ("match --> matchObj.group() : ", matchObj.group())
	else:
	   print ("No match!!")
	 
	matchObj = re.search( r'dogs', line, re.M|re.I)
	if matchObj:
	   print ("search --> matchObj.group() : ", matchObj.group())
	else:
	   print ("No match!!")


## 检索和替换

Python 的re模块提供了re.sub用于替换字符串中的匹配项。

语法：
	
	re.sub(pattern, repl, string, count=0)

参数：

- pattern : 正则中的模式字符串。

- repl : 替换的字符串，也可为一个函数。

- string : 要被查找替换的原始字符串。

- count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配

**实例1**

	import re
	 
	phone = "2004-959-559 # 这是一个电话号码"
	 
	# 删除注释
	num = re.sub(r'#.*$', "", phone)
	print ("电话号码 : ", num)
	 
	# 移除非数字的内容
	num = re.sub(r'\D', "", phone)
	print ("电话号码 : ", num)


## findall

在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。

注意： match 和 search 是匹配一次 findall 匹配所有。

语法格式为：

	findall(string[, pos[, endpos]])

参数：


- `string` 待匹配的字符串。

- `pos` 可选参数，指定字符串的起始位置，默认为 0。

- `endpos` 可选参数，指定字符串的结束位置，默认为字符串的长度。

**实例1**
	import re
	pattern = re.compile(r'\d+')   # 查找数字
	result1 = pattern.findall('runoob 123 google 456')
	result2 = pattern.findall('run88oob123google456', 0, 10)
	 
	print(result1)
	print(result2)


## re.finditer

和 findall 类似，在字符串中找到正则表达式所匹配的所有子串，并把它们作为一个迭代器返回。

	re.finditer(pattern, string, flags=0)


参数说明

|参数|描述|
|----|----|
|pattern|匹配的正则表达式|
|string|要匹配的字符串|
|flags|标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等|


**实例1**

	import re
	 
	it = re.finditer(r"\d+","12a32bc43jf3") 
	for match in it: 
	    print (match.group() )


## re.split

split 方法按照能够匹配的子串将字符串分割后返回列表，它的使用形式如下：

	re.split(pattern, string[, maxsplit=0, flags=0])

参数：

|参数|描述|
|----|----|
|pattern|匹配的正则表达式|
|string|要匹配的字符串|
|maxsplit|分隔次数，maxsplit=1 分隔一次，默认为 0，不限制次数。|
|flags|标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等|