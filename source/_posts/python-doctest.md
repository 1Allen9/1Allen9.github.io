---
title: python-doctest
date: 2025-03-16 18:38:01
tags: [python, doctest]
---
# python测试工具——doctest
## brief introduction

[doctest](https://docs.python.org/zh-cn/3/library/doctest.html)是python自带的一个调试测试工具，可用于初步检测你写的python程序的输入与输出结果是否符合你的预期
通过在自己写的函数内，按照规定的格式写上你想要对比的输入输出结果，再使用```python3 -m doctest example.py```(example.py是你要test的python程序)，python会自动帮你用你的输入和你写的程序能否得到你给定的输出

>    python3是linux里运行python的指令
    在Windows中应该是python -m doctest example.py

>    注：本次测试实验我是在linux系统上测试的，可能与windows操作的情况略有不同，如有操作和你的系统不太一样，可尝试搜索windows系统的操作方法

下面演示一个最直接简单的用法
## example

我们写一个简单的3个数求和的python函数，这里我的python文件名是pydoctest.py
```py
def add_three_num(a, b, c):
    return (a + b + c)
```
现在python运行一下，看看python显示的样子
可以使用python交互式运行指令```python3 -i pydoctest.py```
```bash
$ python3 -i pydoctest.py 
>>> add_three_num(1,1,1)
3
>>> add_three_num(2,2,2)
6
>>> add_three_num(1,2,3)
6
>>> 
```
注意python IDE或者你的cli界面的显示样式：```>>>```后有一个空格，直接return的结果前面没有空格，python交互界面显示的样子，就是你要写给doctest的样子，要完全一致，doctest会匹配检查每个字符，只要有一个字符不一致就判定为fail

现在参考[doctest](https://docs.python.org/zh-cn/3/library/doctest.html)文档，在函数内写上你想测试的内容
```py
def add_three_num(a, b, c):
    """
    >>> add_three_num(1,2,3)
    6
    
    >>> add_three_num(3,2,3)
    8
    """
    return (a + b + c)
```
现在我们使用doctest来看看效果
```bash
$ python3 -m doctest pydoctest.py 
$ 
```
没有任何输出，因为测试pass，不会显示任何信息，可以在脚本命令加上-v来显示所有信息
```bash
$ python3 -m doctest pydoctest.py -v
```
输出结果为：
```bash
Trying:
   add_three_num(1,2,3)
Expecting:
   6
ok
Trying:
   add_three_num(3,2,3)
Expecting:
   8
ok
1 items had no tests:
   pydoctest
1 items passed all tests:
  2 tests in pydoctest.add_three_num
2 tests in 2 items.
2 passed and 0 failed.
Test passed.
```
如果我们python程序有误，比如我把最后的return内容改为`return (a + b - c)
再来运行一下看看
```bash
$ python3 -m doctest pydoctest.py
**********************************************************************
File "/home/mao/MyBlog/blog/source/_posts/Python-doctest-post/pydoctest.py", line 3, in pydoctest.add_three_num
Failed example:
    add_three_num(1,2,3)
Expected:
    6
Got:
    0
**********************************************************************
File "/home/mao/MyBlog/blog/source/_posts/Python-doctest-post/pydoctest.py", line 6, in pydoctest.add_three_num
Failed example:
    add_three_num(3,2,3)
Expected:
    8
Got:
    2
**********************************************************************
1 items had failures:
   2 of   2 in pydoctest.add_three_num
***Test Failed*** 2 failures.
```
可以看到它会提示你出错的文件，与期望不一致的结果，测试pass、fail了几项等等

如果要doctest的py程序和函数较多，也可以把doctest内容单独编写一个文件，运行doctest.testfile("example.txt")来测试，详情可阅读doctest官方文档，有机会的话，我在尝试一下出个blog