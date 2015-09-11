# Python模块介绍-cmd
摘自 2011-06-30 磁针石  
*原文：http://blog.chinaunix.net/uid-20393955-id-752909.html*
*承接软件自动化实施与培训等gtalk： ouyangchongwu#gmail.com qq 37391319 博客:testing.blog.chinaunix.net*
*版权所有，转载刊登请来函联系*
*自动化测试和python群组： http://groups.google.com/group/automation_testing_python*
*python qq group: 深圳自动化测试python群：113938272*
*武冈深圳qq群：66250781*
*参考资料：《The Python Standard Library by Example》*
 
## 14.6. cmd 模块  
> 在cmd模块包含一个公共类:cmd，设计为交互式shell和其他命令解释器等的基类。默认情况下，它使用的ReadLine来进行
互动提示操作，命令行编辑和命令完成。  
 
## 14.6.1 处理命令  
 
cmd创建的命令解释器循环读取其输入的所有行，解析它们，然后发送命令到一个适当的命令处理器。输入行解析成两部分：命令和任何其他文字。如果用户输入foo bar，并且解释类包含一个名为do_foo（）的方法，bar就是参数。  
 
结束文件标记被分派到`do_EOF()`。如果一个命令处理器返回真，程序将干净退出。因此，要干净的退出解释器，一定要实现`do_EOF()`，并将它返回True。  
 
这个简单的例子程序支持“打招呼”的命令。  
```python
#!/usr/bin/env python
# encoding: utf-8
#
# Copyright (c) 2008 Doug Hellmann All rights reserved.
#
 
__version__ = "$Id$"
#end_pymotw_header
 
import cmd
 
class HelloWorld(cmd.Cmd):
    """Simple command processor example."""
   
    def do_greet(self, line):
        print "hello"
   
    def do_EOF(self, line):
        return True
 
if __name__ == '__main__':
HelloWorld().cmdloop()

```  

### Windows下执行实例：
```
D:\language\python\PyMOTW-2.0.1\PyMOTW\cmd>cmd_simple.py
(Cmd) greet
Hello
(Cmd) help
 
Undocumented commands:
======================
EOF  greet  help
(Cmd) ^Z
```  
其中的(Cmd)提示符，可以通过修改属性prompt来更改。  


## 14.6.2 命令参数  
下面例子针对参数进行增强，并给greet增加了文档字符串，它会自动成为帮助文本。
```python
#!/usr/bin/env python
# encoding: utf-8
#
# Copyright (c) 2008 Doug Hellmann All rights reserved.
#
 
__version__ = "$Id$"
#end_pymotw_header
 
import cmd
 
class HelloWorld(cmd.Cmd):
    """Simple command processor example."""
   
    def do_greet(self, person):
        """greet [person]
        Greet the named person"""
        if person:
            print "hi,", person
        else:
            print 'hi'
   
    def do_EOF(self, line):
        return True
   
    def postloop(self):
        print
 
if __name__ == '__main__':
    HelloWorld().cmdloop()
 
```  

### Windows下执行实例：
```
D:\language\python\PyMOTW-2.0.1\PyMOTW\cmd>cmd_arguments.py
(Cmd) help
 
Documented commands (type help ):
========================================
greet
 
Undocumented commands:
======================
EOF  help
 
(Cmd)   greet
hi
(Cmd) greet Tom
hi, Tom
(Cmd) help greet
greet [person]
        Greet the named person
```  
 
## 14.6.3 在线帮助
上例种的帮助，保留了文档字符串中的缩进，不太美观。下面方法使用帮助处理器可以是帮助更好看一点。
```
#!/usr/bin/env python
# encoding: utf-8
#
# Copyright (c) 2008 Doug Hellmann All rights reserved.
#
"""
"""
 
__version__ = "$Id$"
#end_pymotw_header
 
import cmd
 
class HelloWorld(cmd.Cmd):
    """Simple command processor example."""
   
    def do_greet(self, person):
        if person:
            print "hi,", person
        else:
            print 'hi'
   
    def help_greet(self):
        print '\n'.join([ 'greet [person]',
                          'Greet the named person',
                          ])
   
    def do_EOF(self, line):
        return True
 
if __name__ == '__main__':
    HelloWorld().cmdloop()
```  

### Windows下执行实例：
```
D:\language\python\PyMOTW-2.0.1\PyMOTW\cmd>cmd_do_help.py
(Cmd) help greet
greet [person]
Greet the named person
(Cmd)
```  

## 14.6.4 自动完成
通过按TAB可以实现自动完成，多个选项的情况，按下2个TAB可以实现列出所有选项。自动完成的方法添加前缀：complete_
```python
#!/usr/bin/env python
# encoding: utf-8
#
# Copyright (c) 2008 Doug Hellmann All rights reserved.
#
"""
"""
 
__version__ = "$Id$"
#end_pymotw_header
 
import cmd
 
class HelloWorld(cmd.Cmd):
    """Simple command processor example."""
   
    FRIENDS = [ 'Alice', 'Adam', 'Barbara', 'Bob' ]
   
    def do_greet(self, person):
        "Greet the person"
        if person and person in self.FRIENDS:
            greeting = 'hi, %s!' % person
        elif person:
            greeting = "hello, " + person
        else:
            greeting = 'hello'
        print greeting
   
    def complete_greet(self, text, line, begidx, endidx):
        if not text:
            completions = self.FRIENDS[:]
        else:
            completions = [ f
                            for f in self.FRIENDS
                            if f.startswith(text)
                            ]
        return completions
   
    def do_EOF(self, line):
        return True
 
if __name__ == '__main__':
HelloWorld().cmdloop()

```  


正常的程序，一般会把上面的列表cache，只读一次。
执行结果：
```
$ python cmd_arg_completion.py
(Cmd) greet
Adam Alice Barbara Bob
(Cmd) greet A
Adam Alice
(Cmd) greet Ad
(Cmd) greet Adam
hi, Adam!
```  
注意，只在linux下有效。  

## 14.6.5 重载基类方法
cmd包括几种可以重载的方法，可以勾住动作或者改变基类的行为。这个例子并不详尽，但它包含许多常用的方法。
一般来说因为preloop()和 postloop()的存在，重载cmdloop()没有什么必要。
每次迭代cmdloop（）调用onecmd（）来分发命令到处理器。实际输入行用parseline（）解析，创建一个元组包含命令和该行的剩余部分。如果该行是空的，emptyline（）被调用。默认实现运行以前的命令。如果该行包含一个命令，首先precmd（）被调用，
然后查找调用处理器。如果没有找到， postcmd()被调用。

```python
#!/usr/bin/env python
# encoding: utf-8
#
# Copyright (c) 2008 Doug Hellmann All rights reserved.
#
 
__version__ = "$Id$"
#end_pymotw_header
 
import cmd
 
class Illustrate(cmd.Cmd):
    "Illustrate the base class method use."
   
    def cmdloop(self, intro=None):
        print 'cmdloop(%s)' % intro
        return cmd.Cmd.cmdloop(self, intro)
   
    def preloop(self):
        print 'preloop()'
   
    def postloop(self):
        print 'postloop()'
       
    def parseline(self, line):
        print 'parseline(%s) =>' % line,
        ret = cmd.Cmd.parseline(self, line)
        print ret
        return ret
   
    def onecmd(self, s):
        print 'onecmd(%s)' % s
        return cmd.Cmd.onecmd(self, s)
 
    def emptyline(self):
        print 'emptyline()'
        return cmd.Cmd.emptyline(self)
   
    def default(self, line):
        print 'default(%s)' % line
        return cmd.Cmd.default(self, line)
   
    def precmd(self, line):
        print 'precmd(%s)' % line
        return cmd.Cmd.precmd(self, line)
   
    def postcmd(self, stop, line):
        print 'postcmd(%s, %s)' % (stop, line)
        return cmd.Cmd.postcmd(self, stop, line)
   
    def do_greet(self, line):
        print 'hello,', line
 
    def do_EOF(self, line):
        "Exit"
        return True
 
if __name__ == '__main__':
    Illustrate().cmdloop('Illustrating the methods of cmd.Cmd')

```  

执行结果：
```
$ python cmd_illustrate_methods.py
cmdloop(Illustrating the methods of cmd.Cmd)
preloop()
Illustrating the methods of cmd.Cmd
(Cmd) greet Bob
precmd(greet Bob)
onecmd(greet Bob)
parseline(greet Bob) => (’greet’, ’Bob’, ’greet Bob’)
hello, Bob
postcmd(None, greet Bob)
(Cmd) ^Dprecmd(EOF)
onecmd(EOF)
parseline(EOF) => (’EOF’, ’’, ’EOF’)
postcmd(True, EOF)
postloop()
```  

## 14.6.6 配置命令行属性
除了前面介绍的方法，有几个属性控制命令解释器。  
prompt可以设置为一个字符串当打印每次用户是要求一个新的命令时。
intro是启动程序时的“欢迎”消息。   
cmdloop（）接受该值的参数，也可以在类中设置。  
当打印的帮助时，`doc_header，misc_header`，`undoc_header`，和`ruler`属性用于格式化输出。  
```python
#!/usr/bin/env python
# encoding: utf-8
#
# Copyright (c) 2008 Doug Hellmann All rights reserved.
#
 
__version__ = "$Id$"
#end_pymotw_header
 
import cmd
 
class HelloWorld(cmd.Cmd):
    """Simple command processor example."""
 
    prompt = 'prompt: '
    intro = "Simple command processor example."
 
    doc_header = 'doc_header'
    misc_header = 'misc_header'
    undoc_header = 'undoc_header'
   
    ruler = '-'
   
    def do_prompt(self, line):
        "Change the interactive prompt"
        self.prompt = line + ': '
 
    def do_EOF(self, line):
        return True
 
if __name__ == '__main__':
    HelloWorld().cmdloop()

```  

执行结果：
```
$ ./cmd_attributes.py
Simple command processor example.
prompt:
EOF     help    prompt 
prompt: prompt #
#:
#:
#: help
 
doc_header
----------
prompt
 
undoc_header
------------
EOF  help
 
#:
```  

## 14.6.7 执行shell命令
为补充的标准命令处理，cmd包括两个特殊命令前缀。  
问号（？）相当于内置的帮助命令。一个感叹号（！）映射到`do_shell（）`，以运行其他命令，
如这个例子。这个感叹号是要有do_shell方法才会开启。  

```python 
#!/usr/bin/env python
# encoding: utf-8
#
# Copyright (c) 2008 Doug Hellmann All rights reserved.
#
"""
"""
 
__version__ = "$Id$"
#end_pymotw_header
 
import cmd
import subprocess
 
class ShellEnabled(cmd.Cmd):
   
    last_output = ''
 
    def do_shell(self, line):
        "Run a shell command"
        print "running shell command:", line
        sub_cmd = subprocess.Popen(line,
                                   shell=True,
                                   stdout=subprocess.PIPE)
        output = sub_cmd.communicate()[0]
        print output
        self.last_output = output
   
    def do_echo(self, line):
        """Print the input, replacing '$out' with
        the output of the last shell command.
        """
        # Obviously not robust
        print line.replace('$out', self.last_output)
   
    def do_EOF(self, line):
        return True
   
if __name__ == '__main__':
    ShellEnabled().cmdloop()
```  

执行结果：
```
$ python cmd_do_shell.py
(Cmd) ?
Documented commands (type help ):
========================================
echo shell
Undocumented commands:
======================
EOF help
(Cmd) ? shell
Run a shell command
(Cmd) ? echo
Print the input, replacing ’$out’ with
the output of the last shell command
(Cmd) shell pwd
running shell command: pwd
/Users/dhellmann/Documents/PyMOTW/in_progress/cmd
(Cmd) ! pwd
running shell command: pwd
/Users/dhellmann/Documents/PyMOTW/in_progress/cmd
(Cmd) echo $out
/Users/dhellmann/Documents/PyMOTW/in_progress/cmd
(Cmd)
 ```
## 14.6.8 替代输入
Cmd的默认模式（）是通过的readline库与用户交互，它也可以通过UNIX shell重定向传递一系列命令到标准输入，这种方式很利于自动化。比如：  
```
$ echo help | python cmd_do_help.py
(Cmd)
Documented commands (type help ):
========================================
greet
 
Undocumented commands:
======================
EOF  help
 ```
为了直接读取脚本文件，需要一些其他的变化。  
因为readLine和terminal/tty，而不是标准输入流，读取文件时，它应该被禁用。  
此外，为避免过多的提示符，提示可以设置为空字符串。这个例子演示如何打开一个文件作为输入。
```python 
#!/usr/bin/env python
# encoding: utf-8
#
# Copyright (c) 2008 Doug Hellmann All rights reserved.
#
"""
"""
 
__version__ = "$Id$"
#end_pymotw_header
 
import cmd
 
class HelloWorld(cmd.Cmd):
    """Simple command processor example."""
   
    # Disable rawinput module use
    use_rawinput = False
   
    # Do not show a prompt after each command read
    prompt = ''
   
    def do_greet(self, line):
        print "hello,", line
   
    def do_EOF(self, line):
        return True
 
if __name__ == '__main__':
    import sys
    with open(sys.argv[1], 'rt') as input:
        HelloWorld(stdin=input).cmdloop()
```   
运行结果：
 ```
[andrew@SZX-W-SITServer cmd]$ python cmd_file.py cmd_file.txt
hello,
hello, Alice and Bob
```
禁用use_rawinput，并提示设置为空字符串，脚本就可以调用输入文件。
 
## 14.6.9 sys.argv命令
 
要使用命令行参数，直接调用onecmd（），见下例。  
```python
#!/usr/bin/env python
# encoding: utf-8
#
# Copyright (c) 2008 Doug Hellmann All rights reserved.
#
"""
"""
 
__version__ = "$Id$"
#end_pymotw_header
 
import cmd
 
class InteractiveOrCommandLine(cmd.Cmd):
    """Accepts commands via the normal interactive
    prompt or on the command line.
    """
 
    def do_greet(self, line):
        print 'hello,', line
   
    def do_EOF(self, line):
        return True
 
if __name__ == '__main__':
    import sys
    if len(sys.argv) > 1:
        InteractiveOrCommandLine().onecmd(' '.join(sys.argv[1:]))
    else:
        InteractiveOrCommandLine().cmdloop()
 ```  
运行结果：
```
[andrew@SZX-W-SITServer cmd]$ python cmd_argv.py greet Command-Line User
hello, Command-Line User
```

# 附上其他参考资料：
cmd (http://docs.python.org/library/cmd.html)
cmd2 (http://pypi.python.org/pypi/cmd2
GNU Readline (http://tiswww.case.edu/php/chet/readline/rltop.html)