---
layout: post
comments: true
title: "python学习笔记-变量、字符串与长字符串"
categories: python

---
#### python下载地址
* [python下载地址](http://www.python.org)

---
### 变量、字符串与长字符串 

#### 变量

示例

{%highlight ruby%}
>>>teacher="卢禹轩"  
>>>print(teacher)  
卢禹轩  
>>>teacher="卢总"  
>>>print(teacher)  
卢总  

>>>x=3  
>>>x=5  
>>>y=8  
>>>z=x+y  
>>>print(z)  
13  
{%endhighlight%}

同样也可以运用至字符串中

{%highlight ruby%}
>>>myteacher="卢总"  
>>>yourteacher="最帅"  
>>>ourteacher=myther+yourteacher  
>>>print(ourteacher)  
卢总最帅  
{%endhighlight%}

---
#### 字符串
示例

{%highlight ruby%}
>>>5+8  
13  
>>>'5'+'8'  
'58'  
{%endhighlight%}

字符串内容需要出现单引号、双引号或反斜杠  
需要使用转义符号(\)对字符串中的引号进行转义  

{%highlight ruby%}
>>>'Let's go'  
SyntaxError:invalid syntax

>>>'Let\'s go'  
"Let's go"

>>>"Let's go"  
"Let's go"

>>>string='C:\now'  
>>>string  
'C:\now'  
>>>print(string)  
C:  
ow

>>>string='C:\\now'
>>>string  
'C:\\now'  
>>>print(string)  
C:\now

>>>string=r'C:\now'  
>>>string  
'C:\\now'  
>>>print(string)  
C:\now  
{%endhighlight%}

使用字符串需要注意，不能以反斜杠作为结尾，反斜杠放在字符串末尾表示字符串还未结束，换行继续，如果继续这样做就会报错

{%highlight ruby%}
>>>string='卢总最帅\'  
SyntaxError:EOL while scanning string literal  
>>>string=r'卢总最帅\'  
SyntaxError:EOL while scanning string literal  
{%endhighlight%}

---
#### 长字符串
使用三重引号字符串解决长字符串需要使用多个换行符的问题(“““内容”””)

{%highlight ruby%}
>>>print("""  
路上行人急匆而过  
在无人的街头  
呼喊却已无效果  
不原谅我  
""")  
路上行人急匆而过  
在无人的街头  
呼喊却已无效果  
不原谅我  
{%endhighlight%}
