
# Linux command line的基本知识  
- 命令行（Command Line）： （Linux 作为服务方）相当于我们请求服务使用的专业术语。  

# 1 CLI and GUI    
CLI（command-line interface，命令行界面）        
GUI（Graphical User Interface，图形用户接口/图形用户界面）        
“图形用户界面让简单的任务更容易完成， 而命令行界面使完成复杂的任务成为可能”   

# 2 Shell
## 什么是shell？  
命令行，是指shell。shell 就是一个程序，它接受从键盘输入的命令， 然后把命令传递给操作系统去执行。每一个Linux发行版都有一个shell。  

## 终端仿真器 ?
当使用图形用户界面时，我们需要另一个和 shell 交互的叫做终端仿真器的程序。它通常被称为"terminal".每一个Linux发行版的terminal可能不同，但它们都完成同样的事情， 让我们能访问 shell。          

## shell提示符
```
[test@localhost ~]$ 
```
shell 提示符表示shell 准备好了可以去接受输入了。      
不同的Linux发行版本，显示可能不同，它通常包括`用户名@主机名 当前工作目录 权限符号`：   
- 权限符号`#` - 超级用户权限.  
例如以root用户登陆 ，或选择的终端仿真器提供超级用户（管理员）权限。     
- 权限符号`$` - 普通权限     


```
[test@localhost ~]$ dogag
bash: dogag: command not found...
[test@localhost ~]$ 
```
- 命令没有任何意义，所以 shell 会提示错误信息    

## 命令历史
下上箭头按键，刚才输入的命令重新出现在提示符之后。 这就叫做命令历史。    
许多 Linux 发行版默认保存最后输入的500个命令。 按下下箭头按键，先前输入的命令就消失了。  

## 关于鼠标和光标   
按下鼠标左键，沿着文本拖动鼠标（或者双击一个单词）高亮了一些文本， 那么这些高亮的文本就被拷贝到了一个由 X 管理的缓冲区里面。然后按下鼠标中键， 这些文本就被粘贴到光标所在的位置。   
不要在一个终端窗口里使用 Ctrl-c 和 Ctrl-v 快捷键来执行拷贝和粘贴操作。 它们不起作用。对于 shell 来说，这两个控制代码有着不同的含义


## 结束终端会话
通过关闭终端仿真器窗口,或 在 shell 提示符下输入 exit 命令

  
## 幕后控制台
即使终端仿真器没有运行，在后台仍然有几个终端会话运行着。它们叫做虚拟终端 或者是虚拟控制台。   
在大多数 Linux 发行版中，这些终端会话都可以通过按下 Ctrl-Alt-F1 到 Ctrl-Alt-F6 访问。    
当一个会话被访问的时候， 它会显示登录提示框，我们需要输入用户名和密码。要从一个虚拟控制台转换到另一个， 按下 Alt 和 F1-F6(中的一个)。返回图形桌面，按下 Alt-F7。    

# 3 文件系统  

## 理解文件系统树  
类似Windows，一个“类 Unix” 的操作系统，比如说 Linux，以分层目录结构来组织所有文件。 所有文件组成了一棵树型目录（即文件夹）， 这个目录树可能包含文件和其它的目录。文件系统中的第一级目录称为根目录。 根目录包含文件和子目录，子目录包含更多的文件和子目录，依此类推。    


Windows vs 类Unix ：    
Windows ： 每个存储设备都有一个独自的文件系统。    
类 Unix： 只有一个单一的文件系统树，不管有多少个磁盘或者存储设备连接到计算机上。 存储设备挂载到目录树的各个节点上。     


## 关于Linux中的文件名：
隐藏文件以.开头；
没有“文件扩展名”的概念，但程序会用来决定用途；
文件名中可以包含空格和标点符号（“.”，“－”，“_”），但尽量别用空格。

## 命令
```
command -options arguments
```

大多数命令使用的选项，是由一个中划线加上一个字符组成.  
但是许多命令，包括来自于 GNU 项目的命令，也支持长选项，长选项由两个中划线加上一个字组成. 
许多命令也允许把多个短选项串在一起使用.  

Example:
```
ls -lt
ls -lt --reverse
```
