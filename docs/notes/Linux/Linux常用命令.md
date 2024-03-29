# 特别备注

+ **shell与终端**

  + Shell 是一个程序，它接收由键盘输入的命令并将其传递给操作系统执行，几乎所有的 Linux 发行版都提供了来自 GNU 项目的 Shell 程序 Bash。
  + 当使用图形用户界面时，使用终端仿真器（简称终端）与Shell进行交互。

+ 命令选项

  + 短选项：单个字符前加上单个连字符，如`ls -l`
  + 长选项：在单词前加两个连字符

+ 命令行操作技巧

  + 使用鼠标，双击可以复制，单击中键可以将其粘贴到命令中。



# 一、获取命令帮助信息

## type：显示命令类型

```bash
# 命令格式
type command [command ...]
```

> Linux 中的命令可以分为以下几种：
>
> + 可执行程序：如 /usr/bin 中的文件
> + Shell 中的内建命令
> + Shell 函数
> + 别名（在其他命令的基础上自定义的命令）

```bash
# 命令示例
zhu@ubuntu:~$ type type
type is a shell builtin		# type是一个shell内建命令
zhu@ubuntu:~$ type ls
ls is aliased to `ls --color=auto`	# ls是其他命令的别名 	
zhu@ubuntu:~$ type cp
cp is /bin/cp		# cp命令是一个可执行程序，其位置在/bin/cp
```

## which：显示可执行文件的位置

> 只能用于可执行文件的位置查找

```bash
# 命令格式
which command
```

```bash
# 命令示例
zhu@ubuntu:~$ which ls
/bin/ls
```

## help：获取Shell内建命令帮助信息

```bash
# 命令格式
help command
```

```bash
# 命令示例
zhu@ubuntu:/$ help cd
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.
    
    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.
    
    ......
```

## --help选项：显示命令帮助信息

```bash
# 命令格式
command --help
```

## man：显示命令手册页

Linux 中大多数命令会提供一份命令的正式文档

```bash
# 命令格式
man command
```

## whatis：显示命令的简述

```bash
# 命令格式
whatis command 
```

```bash
# 命令示例
zhu@ubuntu:/$ whatis mv
mv (1)               - move (rename) files
zhu@ubuntu:/$ whatis ls
ls (1)               - list directory contents
```



# 二、文件系统导航

## pwd：查看当前工作目录

```bash
zhu@ubuntu:~$ pwd
/home/zhu
```

> 注：当用户启动终端时，当前工作目录就是用户的主目录。每个用户都有自己的主目录，这是普通用户唯一有权限写入文件的地方。

## ls：列出目录下的文件

```bash
# 简单的列出当前目录内容
ls

# 显示当前目录内容的更多细节
ls -l
# 输出示例：drwxr-xr-x 3 zhu zhu 4.0K Jun 26  2020 Desktop
# 输出解释：
	# “drwxr-xr-x”：表示文件的访问权限。第一个字符指明了文件类型，为d表示目录，为-表示普通文件；接下来的三个字符“rwx”表示文件属主的访问权限，r表示可读取，w表示可修改，x表示可执行；后续的三个字符“r-x”表示文件属组的访问权限；最后三个字符“r-x”表示其他用户的访问权限。
	# “3”：文件的硬链接数量
	# “zhu”：文件属主
	# “zhu”：文件属组
	# “4.0K”：文件大小（单位字节）
	# “Jun 26  2020”：文件最后修改时间
	# “Desktop”：目录名

# 列出所有文件，包括隐藏文件
ls -a
```

## cd：更改当前工作目录

```bash
# 方式一：绝对路径名
# 绝对路径名从根目录开始，直到目标目录或文件，根目录在路径中用/来表示
cd /usr/bin

# 方式二：相对路径名
# 相对路径名从当前工作目录开始，存在两个特殊符号：
# .表示当前工作目录，
# ..表示当前工作目录的父目录
cd .. # 更改当前工作目录为其父目录

# 几个超便捷用法
cd		# 将当前工作目录更改为用户主目录
cd -	# 将当前工作目录切换回上一个工作目录
cd ~user_name		# 将当前工作目录切为用户user_name的主目录
```



# 三、文件和目录

## file：查看文件类型

```bash
zhu@ubuntu:~$ file Desktop
Desktop: directory
```

## mkdir：创建目录

```bash
mkdir DIRECTORY...
# ...表示可以同时创建多个目录
```

## cp：复制文件和目录

```bash
# 将单个文件或目录SOURCE复制到文件或目录DEST
cp SOURCE DEST

# 将多个文件或目录SOURCE复制到目录DEST
cp SOURCE...ST
```

cp命令的一些常用选项如下所示：

| 选项                | 含义                                                         |
| ------------------- | ------------------------------------------------------------ |
| -i, --interactive   | 在覆盖已有文件之前，提示用户进行确认，如果未指定该选项，命令会自动覆盖文件 |
| -R, -r, --recursive | 递归地复制目录，在复制目录时使用该选项（或者-a选项）         |
| -u, --update        | 只有目标目录中不存在或当前文件比目标目录中的文件新时，才复制 |
| -v, --verbose       | 复制时显示相关信息                                           |

## mv：移动和重命名文件

该命令的用法与cp命令大同小异，唯一的区别是源文件被删除了。

```bash
Usage: cp [OPTION]... SOURCE DEST
  or:  cp [OPTION]... SOURCE... DIRECTORY
```

## rm：删除文件和目录

```bash
Usage: rm [OPTION]... [FILE]...

常用选项：
-f, --force		忽略不存在的文件
-i				在每次删除之前，提示用户确认
-r, -R, --recursive		递归地删除目录,即如果被删除的目录内还有子目录，也会一并删除，想要删除目录，必须指定该选项
```

## ln：创建硬链接和符号链接

Linux 系统存在两种链接：硬链接、符号链接。

在大多数 UNIX 系统中，一个文件可以被多个名称引用。

在 Linux 的文件系统中，保存在磁盘分区中的文件不管什么类型都会分配一个编号，称为索引节点号（Inode Index）。

+ **硬链接**

  硬链接的作用是让多个文件名指向同一索引节点，这样只删除一个链接并不影响索引节点本身和其它的链接，只有当最后一个链接被删除后，文件才会被删除。

  硬链接存在两个问题：硬链接不能引用其所在文件系统之外的文件，即**不在同一个磁盘分区**内的文件；硬链接不能引用目录，只能引用文件。

  > 符号链接弥补了硬链接的这两个不足之处。

  ```bash
  # 创建硬链接命令格式，LINK_NAME可以使用绝对路径名或相对路径名表示
  $ ln file LINK_NAME
  
  # 创建硬链接示例，可以看出创建的硬链接的索引节点号相同，指向同一个文件
  $ ls
  passwd
  $ ln passwd fun
  $ ls -il
  679024 -rw-r--r-- 2 zhu zhu 2398 Mar  9 22:14 fun
  679024 -rw-r--r-- 2 zhu zhu 2398 Mar  9 22:14 passwd
  ```

+ **符号链接**

  符号链接类似于 Windows 的文件快捷方式，是一个保存了被引用文件位置信息的特殊文件。

  使用场景：某个程序需要使用包含在文件 foo 中的共享资源，但该文件的版本变化频繁。最好能在文件名中加入版本号，这样超级用户或其他相关用户就知道安装的是文件 foo 的哪个版本。但有一个问题，如果我们改变了共享资源的名称，就必须跟踪所有用到该共享资源的程序，并对其做出改动，以便能够找到新的共享资源。

  符号链接如何起作用：假设我们安装了2.6版本的文件foo，其文件名为foo-2.6，然后创建一个指向foo-2.6的符号链接foo。这意味着当程序打开文件foo的时候，实际上打开的是文件foo-2.6。当要升级到文件foo-2.7时，只需将该文件加入系统，删除符号链接foo，再创建一个指向新版本的符号链接即可。

  ```bash
  # 创建符号链接命令格式，其中，item可以是文件或目录
  ln -s item LINK_NAME
  
  # 创建符号链接示例
  zhu@ubuntu:~/test$ ls -l
  total 4
  -rw-rw-r-- 1 zhu zhu 208 Jul  1 18:20 text-7.txt
  zhu@ubuntu:~/test$ ln -s text-7.txt text
  zhu@ubuntu:~/test$ ls -l
  total 4
  lrwxrwxrwx 1 zhu zhu  10 Jul  1 18:51 text -> text-7.txt
  -rw-rw-r-- 1 zhu zhu 208 Jul  1 18:20 text-7.txt
  
  # 注：text文件的“lrwxrwxrwx”的第一个字母是'l'，表示该文件是一个符号链接，“text -> text-7.txt”表示链接指向
  ```

# 四、I/O重定向

I/O：即输入输出

标准输入：程序在运行过程中一般从标准输入获取输入，默认情况下标准输入与键盘相关联。

标准输出：程序在运行过程中将运行结果发送到标准输出，默认情况下标准输出与显示器屏幕相关联，不会保存为磁盘文件。

标准错误：程序运行过程中产生的错误信息发送到标准错误，默认情况下标准错误与显示器屏幕相关联。

I/O重定向：改变输入的来源（不从键盘输入）和输出的位置（不输出到屏幕）。

## 1. 标准输出重定向 

重定向操作符：`>`

```bash
# 命令示例
zhu@ubuntu:~$ ls -l > ls-output.txt
# 将命令`ls -l`的输出结果存储到ls-output.txt文件中
# 如果ls-output.txt文件不存在，会新建该文件
# 如果ls-output.txt文件已存在，会覆盖已有文件内容
```

将输出追加到文件末尾（不覆盖已有内容），使用重定向操作符：`>>`

## 2. 标准错误重定向

不存在专门的重定向操作符。

标准错误重定向需要使用文件描述符，shell 使用文件描述符0、1、2 分别表示标准输入、标准输出、标准错误。

```bash
# 命令示例
zhu@ubuntu:~$ ls -l /test 2> ls-error.txt
# 注：文件描述符2紧靠在重定向操作符之前，将标准错误重定向到ls-error.txt文件
```

将标准输出和标准错误重定向到一个文件中：

```bash
# 旧版本
ls -l /test > ls-output.txt 2>&1
# 注：标准错误的重定向操作必须在标准输出重定向之后

# 新版本
ls -l /test &> ls-output.txt
```

丢弃不需要的输出结果：将输出结果重定向到 `/dev/null`  的特殊文件

```bash
ls -l /test 2> /dev/null
```

## 3. 标准输入重定向

标准输入重定向：从磁盘文件中获取输入。

标准输入重定向操作符：`<`



# 五、查看文件内容

## less：查看文本文件

```bash
# 命令格式
$ less file_name
```

`less` 是一个分页程序，如果文件内容多于一页可以配合以下命令前后翻看文本内容：

| 命令                        | 操作                 |
| --------------------------- | -------------------- |
| 上翻页键（Page Up）         | 查看上一页           |
| 下翻页键（Page Down）或空格 | 查看下一页           |
| 上方向键                    | 向后一行             |
| 下方向键                    | 向前一行             |
| G                           | 移动到文本文件末尾   |
| g                           | 移动到文本文件开头   |
| /characters                 | 向前搜索指定的字符串 |
| n                           | 重复上一次搜索       |
| q                           | **退出 less 命令**   |
| h                           | 显示帮助信息         |

## cat：拼接文件并在标准输出上打印

读取一个或多个文件，拼接后显示在标准输出。

cat 命令常用于显示短文本文件。

```bash
# 命令格式
$ cat file...
# note：With no file, or file is -, read standard input
# ctrl+d读取标准输入结束
```

> 扩展：可以利用 `cat` 命令创建小的文本文件
>
> ```bash
> # cat读取标准输入，通过将标准输出重定向到text.txt文件，从而将标准输入的内容存储到文本文件中
> $ cat > text.txt
> It's been a long day without you my friend
> And I'll tell you all about it when I see you again
> We've come a long way from where we began
> Oh I'll tell you all about it when I see you again
> When I see you again
> $ cat text.txt
> It's been a long day without you my friend
> And I'll tell you all about it when I see you again
> We've come a long way from where we began
> Oh I'll tell you all about it when I see you again
> When I see you again
> ```

## grep：查询与模式匹配的行

grep：global regular expression print，在文本文件中搜索与指定的正则表达式匹配的文本，将包含匹配项的文本行输出到标准输出。

```shell
Usage: grep [OPTION]... PATTERN [FILE]...
Search for PATTERN in each FILE.
Example: grep -i 'hello world' menu.h main.c

# 常用选项：
-i, --ignore-case         忽略字母大小写（默认区分大小写）
-v		正常情况下，grep会输出与正则表达式匹配的文本行，该选项输出没有匹配的文本行
-c		输出匹配的数量，不再输出文本行
-l		输出包含匹配项的文件名，不再输出文本行
-n		在包含匹配项的文本行之前加上行号
```

### 正则表达式







利用通配符，可以构建出复杂的文件名匹配条件。

常用的通配符及其含义如下所示：

| 通配符        | 含义                                         |
| ------------- | -------------------------------------------- |
| *             | 匹配任意多个字符                             |
| ？            | 匹配任意单个字符                             |
| [characters]  | 匹配字符集合characters中的任意单个字符       |
| [!characters] | 匹配不属于字符集合characters中的任意单个字符 |
| [[:class:]]   | 匹配字符类class中的任意单个字符              |

> 不能匹配以点号开头的文件（即隐藏文件），如果想匹配此类文件，可以使用模式`.[!.]`

常用的字符类及其含义如下所示：

| 字符类    | 含义                     |
| --------- | ------------------------ |
| [:alnum:] | 匹配任意单个字母数字字符 |
| [:alpha:] | 匹配任意单个字母         |
| [:digit:] | 匹配任意单个数字         |
| [:lower:] | 匹配任意单个小写字母     |
| [:upper:] | 匹配任意单个大写字母     |





## head：输出文件的开头部分

```shell
Usage: head [OPTION]... [FILE]...

# 常用选项：
-n		指定打印的行数（默认打印10行）
```

## tail：输出文件的结尾部分

```shell
Usage: tail [OPTION]... [FILE]...
Print the last 10 lines of each FILE to standard output.

# 常用选项：
-f		实时查看文件
-n		指定打印的行数
```



# 六、文件编辑

## Vi/Vim：文本编辑器

> VIM ：Vi IMproved  的缩写

+ vi/vim 命令存在三种模式：
  + 命令模式（启动时处于）
  + 插入模式（命令模式下按下 `i` 进入插入模式，插入模式下按 `Esc` 键可以退出插入模式并返回命令模式）
  + 底行命令模式（命令模式下输入 `:` 直接进入，插入模式下按 `Esc` 键先退出到命令模式下再）

  > 如果在 Vi 中“迷路”，搞不清当前所处的模式，连按两次 `Esc` 键就可以返回命令模式。

+ 保存对文件的改动

  + 方式一：底行命令模式下输入 `w` ，然后按 `Enter` 键，保存当前编辑文件，但并不退出，继续等待用户输入命令；
  + 方式二：底行命令模式下输入 `w newfile`，将当前文件的内容保存到指定的 newfile 中，而原有文件保持不变，若 newfile 是一个已存在的文件，则在显示窗口的状态行给出提示信息：File exists (use ! to override)，此时，若用户希望用当前内容替换 newfile 中原有内容，可使用命令 : `w! newfile`；
  + 方式三：底行命令模式下输入 `wq` ，先保存文件，然后退出。

+ 撤销更改

  命令 `u`

+ 关闭打开的文件

  + 方式一：底行命令模式下输入 `q` ，如果文件没有被修改，直接退出；如果文件被修改了，在显示窗口的最末行显示如下信息：`No write since last change (use ! to overrides)` 提示用户该文件被修改后没有保存，然后并不退出，继续等待用户命令；
  + 方式二：连按两次大写字母 `Z` ，若当前编辑的文件被修改过，则保存该文件后退出；若当前编辑的文件没被修改过，则直接退出；
  + 方式三：底行命令模式下输入 `q!` ，放弃所作修改而直接退出；
  + 方式四：底行命令模式下输入 `wq` ，先保存文件，然后退出。

+ 命令模式下光标移动

  | 光标移动                       | 命令                                              |
  | ------------------------------ | ------------------------------------------------- |
  | 向右移动一个字符               | `l` 或右方向键                                    |
  | 向左移动一个字符               | `h` 或左方向键                                    |
  | 向下移动一行                   | `j` 或下方向键                                    |
  | 向上移动一行                   | `k` 或上方向键                                    |
  | **移动到当前行行首**           | 数字 0                                            |
  | **移动到当前行行尾**           | `$`                                               |
  | **移动到最后一行**             | 方式一：`G ` <br />方式二：`shift + g`            |
  | **移动到第一行**               | 方式一：输入 `:0` 或 `:1`，回车<br />方式二：`gg` |
  | 移动到指定的第 n 行            | `nG`                                              |
  | 移动到下一个单词开头或标点符号 | w（大写忽略标点符号）                             |
  | 移动到上一个单词开头或标点符号 | b                                                 |

  > 注：许多命令的前面都可以加上数字前缀，通过数字可以指定命令执行的次数。

+ 文本编辑

  + 在当前光标之后追加文本：命令模式下输入 `a`（`i`命令无法做到）
  + 在已有的两行之间插入一个空行并进入插入模式：`o`（在当前行之下新建一行）、`O`（在当前行之上新建一行）

+ 文本删除

  | 删除内容                     | 命令 |
  | ---------------------------- | ---- |
  | 删除当前字符                 | x    |
  | 删除当前字符和之后的两个字符 | 3x   |
  | 当前行                       | dd   |
  | 当前行和接下来的4行          | 5dd  |
  | 从光标处一直到行尾           | d$   |
  | 从光标处一直到行首           | d0   |
  | 从当前行一直到文件末尾       | dG   |
  | 从当前行一直到第20行         | d20G |

+ 剪切文本

  使用 `d` 命令，被删除的文本会被复制到粘贴缓冲区，可以使用 `p` 命令将粘贴缓冲区中的内容粘贴到光标之前或之后；

+ 复制文本

  使用 `y` 命令，使用方式和用于剪切文本的 d 命令差不多；

+ 粘贴文本

  使用 `p` 命令；

+ 合并两行

  无法通过将光标移至行尾，删除行尾字符来合并当前行与下一行；

  使用命令 `J`

+ 文本搜索和替换

  | 操作                                                         | 命令                       |
  | ------------------------------------------------------------ | -------------------------- |
  | 行内搜索某个字符并将光标移动至指定字符的下一次出现处（输入分号可以重复先前的搜索） | `fa`（搜索字符a）          |
  | 全文搜索指定单词并将光标移动至指定单词的下一次出现处（使用 n 命令重复搜索上一次指定的字符串）（也可以指定正则表达式） | `/待搜索字符串`            |
  | 在若干行或整个文件范围内执行搜索和替换操作                   | `:%s/搜索模式/替换文本/gc` |
  
  >   `:%s/搜索模式/替换文本/g` 解释：
  >
  > + % ：指定操作的行范围，此处`%`表示整个文本；可以使用 `1,5` 指定行范围，表示从第1行到第5行；如果未指定行范围，仅对当前行执行操作；
  > + g ：表示对行中所有搜索到的字符串执行替换操作，如果不指定，仅替换每行搜索到的第一个字符串
  > + c ：在替换的时候由用户进行确认，如果不指定，不需要经过用户确认；
  >   + y ：执行替换；
  >   + n ：跳过本次替换；
  >   + a ：执行所有替换；
  >   + q ：退出替换；



# 七、进程

每个进程都被分配了一个进程 ID（Process ID, PID）。

## ps：查看进程

> ps 命令显示的是运行 ps 时刻系统内进程的快照，没有动态显示的功能

```bash
Description: ps - report a snapshot of the current processes.
Usage: ps [options]

# 常用的两组选项组合如下：
	ps -ef
	ps aux (没有前置连字符，与-ef不同的是，该选项组合的作用：To see every process on the system using BSD syntax)

# 常用选项
-e		显示所有的进程，与 -A 选项功能相同
-f		输出格式控制，使用完整的格式显示进程信息
```

```bash
[shengwen@localhost ~]$ ps -ef | grep ps
UID         PID   PPID  C STIME TTY          TIME CMD
root       1199      1  0 Jul16 ?        00:00:00 /usr/sbin/cupsd -l
shengwen   2981   2503  0 Jul16 tty2     00:00:02 /usr/libexec/tracker-miner-apps
shengwen   6726   3160  0 04:45 pts/0    00:00:00 ps -ef
shengwen   6727   3160  0 04:45 pts/0    00:00:00 grep --color=auto ps
```

```bash
[shengwen@localhost ~]$ ps aux | grep ps
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root       1199  0.0  0.2 133560  9936 ?        Ss   Jul16   0:00 /usr/sbin/cupsd -l
shengwen   2981  0.0  0.6 769848 25696 tty2     SNl+ Jul16   0:02 /usr/libexec/tracker-miner-apps
shengwen   6664  0.0  0.0  57184  3820 pts/0    R+   04:40   0:00 ps aux
shengwen   6665  0.0  0.0  12108  1068 pts/0    R+   04:40   0:00 grep --color=auto ps

# 输出信息解释：
USER	用户ID，该进程的属主
%CPU	CPU占用率
%MEM	内存占用率
VSZ		虚拟内存大小
RSS		占用的RAM大小
START	进程启动时间
```

## top：动态查看进程

top 命令可以动态更新系统进程的信息。

top 命令的输出结果分为两部分：上部分显示系统的总体状态信息，下部分显示按CPU活动排序的进程列表。

```shell
[shengwen@localhost ~]$ top
top - 04:49:23 up  5:45,  1 user,  load average: 0.00, 0.03, 0.02
Tasks: 261 total,   1 running, 260 sleeping,   0 stopped,   0 zombie
%Cpu(s):  1.4 us,  2.9 sy,  0.0 ni, 94.2 id,  0.0 wa,  1.4 hi,  0.2 si,  0.0 st
MiB Mem :   3757.6 total,   1918.3 free,   1136.7 used,    702.5 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   2370.9 avail Mem 

   PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND   
  2580 shengwen  20   0 3129176 267424 109472 S   9.0   7.0   2:32.65 gnome-sh+ 
  3155 shengwen  20   0  531184  42116  30444 S   2.0   1.1   0:23.35 gnome-te+ 
  2752 root      20   0  204224  32100  10200 S   0.3   0.8   0:51.83 sssd_kcm  
  2978 shengwen  20   0  569824  40240  33240 S   0.3   1.0   0:35.66 vmtoolsd  
  3995 root      20   0       0      0      0 I   0.3   0.0   0:00.53 kworker/+ 
  6346 root      20   0       0      0      0 I   0.3   0.0   0:00.22 kworker/+ 
  6785 shengwen  20   0   64012   4944   4080 R   0.3   0.1   0:00.26 top       
     1 root      20   0  179356  14036   9152 S   0.0   0.4   0:05.90 systemd   
     2 root      20   0       0      0      0 S   0.0   0.0   0:00.02 kthreadd  
     3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp    
     4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_par_+ 
     6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/+ 
```

![](image/top命令输出信息解释.png)

top 命令可以搭配键盘使用，常用的键盘命令如下：

+ **按进程的CPU使用率排序**

  打开大写键盘的情况下，直接按P键；未打开大写键盘的情况下，Shift+P键

+ **按照内存使用率排序**

  打开大写键盘的情况下，直接按M键，打开大写键盘的情况下，直接按M键



# 八、权限

普通用户切换到 root 用户：

+ 方式一：输入 `su`，然后按照提示输入 root 密码；
+ 方式二：输入 `sudo su`，然后按照提示输入 root 密码（一般不需要输入密码就能切换到 root 用户）
+ 方式三：输入 `su root`，然后按照提示输入 root 密码

root 用户切换到普通用户：

+ 方式一：输入 `exit`
+ 方式二：`ctrl + d`
+ 方式三：输入 `su 用户名`



# 九、磁盘

## 1. df：查看磁盘使用情况

df，即 disk free

| 命令常用选项 | 描述                                 | 示例                                                     |
| ------------ | ------------------------------------ | -------------------------------------------------------- |
| -h           | --human-readable，使用人类可读的格式 | 存储空间大小默认显示为104857600，使用该选项后显示为 100G |
| -T           | 显示文件系统类型                     |                                                          |

| 输出结果   | 描述                 | 备注   |
| ---------- | -------------------- | ------ |
| Filesystem | 文件系统名称         |        |
| Size       | 文件系统磁盘空间大小 |        |
| Used       | 使用空间大小         |        |
| Avail      | 可用空闲空间大小     |        |
| Use%       | 使用空间的的百分比   |        |
| Mounted on | 文件系统的挂载点     | 挂载： |



# 其他

## 1. 管道操作符

`|` ：将一个命令的标准输出传给另一个命令的标准输入

```bash
Usage: command_1 | command_2

Example:
[shengwen@localhost ~]$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  test  Videos
[shengwen@localhost ~]$ ls | grep 'D'
Desktop
Documents
Downloads
```





















