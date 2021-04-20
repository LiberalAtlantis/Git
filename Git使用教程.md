# Git学习笔记

## 缩略语

集中化的版本控制系统（Centralized Version Control Systems，简称 CVCS）

## 基础知识

Git 有三种状态，你的文件可能处于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。
•已修改：表示修改了文件，但还没保存到数据库中。
•已暂存：表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
•已提交：表示数据已经安全地保存在本地数据库中。

这会让我们的 Git 项目拥有三个阶段：工作区、暂存区以及 Git 目录。
工作区是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
暂存区是一个文件，保存了下次将要提交的文件列表信息，一般在 Git 仓库目录中。 按照 Git 的术语叫做“索引”，不过一般说法还是叫“暂存区”。
Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，复制的就是这里的数据。

基本的 Git 工作流程如下：
1.在工作区中修改文件。
2.将你想要下次提交的更改选择性地暂存，这样只会将更改的部分添加到暂存区。
3.提交更新，找到暂存区的文件，将快照永久性存储到 Git 目录。
如果 Git 目录中保存着特定版本的文件，就属于 已提交状态。 如果文件已修改并放入暂存区，就属于 已暂存状态。 如果自上次检出后，作了修改但还没有放到暂存区域，就是 已修改状态。

## Git安装

### Linux

以 Fedora 为例，如果你在使用它（或与之紧密相关的基于 RPM 的发行版，如 RHEL 或 CentOS），你可以使用 dnf：

~~~bash
\$ sudo dnf install git-all
~~~

如果你在基于 Debian 的发行版上，如 Ubuntu，请使用 apt：

~~~bash
\$ sudo apt install git-all
~~~

下载地址
[https://git-scm.com/download/linux]

### macOS

在 Mac 上安装 Git 有多种方式。 最简单的方法是安装 Xcode Command Line Tools。 Mavericks （10.9） 或更高版本的系统中，在 Terminal 里尝试首次运行 git命令即可。

~~~bash
\$ git --version
~~~

下载地址
[https://git-scm.com/download/mac]

### Windows

在 Windows 上安装 Git 也有几种安装方法。 官方版本可以在 Git 官方网站下载。 打开 [https://git-scm.com/download/win]，下载会自动开始。 要注意这是一个名为 Git for Windows 的项目（也叫做 msysGit），和 Git是分别独立的项目；更多信息请访问 [http://msysgit.github.io/]。
要进行自动安装，你可以使用 Git Chocolatey 包。 注意 Chocolatey 包是由社区维护的。
另一个简单的方法是安装 GitHub Desktop 。 该安装程序包含图形化和命令行版本的 Git 。 它也能支持Powerbash，提供了稳定的凭证缓存和健全的换行设置。稍后我们会对这方面有更多了解，现在只要一句话就够了，这些都是你所需要的。 你可以在 GitHub for Windows 网站下载，网址为 GitHub Desktop 网站。

### 从源码安装

如果你想从源码安装 Git，需要安装 Git 依赖的库：autotools、curl、zlib、openssl、expat 和 libiconv。 如果你的系统上有 dnf（如 Fedora）或者 apt（如基于 Debian 的系统），可以使用对应的命令来安装最少的依赖以便编译并安装 Git 的二进制版：

~~~bash
\$ sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel \  openssl-devel perl-devel zlib-devel
\$ sudo apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev \  gettext libz-dev libssl-dev
~~~

为了添加文档的多种格式（doc、html、info），需要以下附加的依赖：

~~~bash
\$ sudo dnf install asciidoc xmlto docbook2X
\$ sudo apt-get install asciidoc xmlto docbook2x
~~~

使用 RHEL 和 RHEL 衍生版，如 CentOS 和 Scientific Linux 的用户需要 开启 EPEL 库以便下载 docbook2X 包。

如果你使用基于 Debian 的发行版（Debian/Ubuntu/Ubuntu-derivatives），你也需要 install-info 包：

~~~bash
\$ sudo apt-get install install-info
~~~

如果你使用基于 RPM 的发行版（Fedora/RHEL/RHEL衍生版），你还需要 getopt 包 （它已经在基于 Debian的发行版中预装了）：

~~~bash
\$ sudo dnf install getopt
~~~

此外，如果你使用 Fedora/RHEL/RHEL衍生版，那么你需要执行以下命令：

~~~bash
\$ sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi
~~~

以此来解决二进制文件名的不同。

当你安装好所有的必要依赖，你可以继续从几个地方来取得最新发布版本的 tar 包。 你可以从 Kernel.org 网站获取，网址为 [https://www.kernel.org/pub/software/scm/git]， 或从 GitHub 网站上的镜像来获得，网址为[https://github.com/git/git/releases]。 通常在 GitHub 上的是最新版本，但 kernel.org 上包含有文件下载签名，如果你想验证下载正确性的话会用到。

编译并安装

~~~bash
\$ tar -zxf git-2.8.0.tar.gz
\$ cd git-2.8.0
\$ make configure
\$ ./configure --prefix=/usr
\$ make all doc info
\$ sudo make install install-doc install-html install-info
~~~

使用 Git 来获取 Git 的更新

~~~bash
\$ git clone git://git.kernel.org/pub/scm/git/git.git
~~~

## 初次运行

每台计算机上只需要配置一次，程序升级时会保留配置信息。 你可以在任何时候再次通过运行命令来修改它们。
Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。这些变量存储在三个不同的位置：
1./etc/gitconfig 文件: 包含系统上每一个用户及他们仓库的通用配置。如果在执行 git config 时带上 --system 选项，那么它就会读写该文件中的配置变量。（由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）
2.~/.gitconfig或 ~/.config/git/config 文件：只针对当前用户。 你可以传递 --global 选项让 Git读写此文件，这会对你系统上所有的仓库生效。
3.当前使用仓库的 Git 目录中的 config文件（即 .git/config）：针对该仓库。 你可以传递 --local选项让 Git 强制读写此文件，默认情况下用的就是它。（需要进入某个 Git 仓库中才能让该选项生效。）每一个级别会覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量。

在 Windows 系统中，Git 会查找 \$HOME目录下（一般情况下是 C:\Users\\$USER）的 .gitconfig文件。Git 同样也会寻找 /etc/gitconfig文件，但只限于 MSys 的根目录下，即安装 Git 时所选的目标位置。 如果你在 Windows 上使用 Git 2.x 以后的版本，那么还有一个系统级的配置文件，Windows XP 上在C:\Documents and Settings\All Users\Application Data\Git\config，Windows Vista 及更新的版本在 C:\ProgramData\Git\config。此文件只能以管理员权限通过 git config -f `'<'`file`'>'` 来修改。你可以通过以下命令查看所有的配置以及它们所在的文件：

~~~bash
\$ git config --list --show-origin
~~~

### 用户信息

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

~~~bash
\$ git config --global user.name "John Doe"
\$ git config --global user.email johndoe@example.com
~~~

如果使用了 --global 选项，那么该命令只需要运行一次，之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global选项的命令来配置。

### 文本编辑器

用户信息设置完毕，可以配置默认文本编辑器，当 Git 需要你输入信息时会调用它。 如果未配置，Git 会使用操作系统默认的文本编辑器。如果你想使用不同的文本编辑器，例如 Emacs，可以输入如下命令：
\$ git config --global core.editor emacs

在 Windows 系统上，如果你想要使用别的文本编辑器，那么必须指定可执行文件的完整路径。 它可能随你的编辑器的打包方式而不同。

对于 Notepad++，一个流行的代码编辑器来说，你可能想要使用 32 位的版本， 因为在本书编写时 64 位的版本尚不支持所有的插件。 如果你在使用 32 位的 Windows 系统，或在 64 位系统上使用 64 位的编辑器，那么你需要输入如下命令：

~~~bash
\$ git config --global core.editor "'C:/ProgramFiles/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
~~~

### 检查配置信息

使用 git config --list 命令来列出所有 Git 当时能找到的配置。

~~~bash
\$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
~~~

可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置（例如：/etc/gitconfig与~/.gitconfig）。 这种情况下，Git 会使用它找到的每一个变量的最后一个配置。

可以通过输入 git config `'<'`key`'>'`：来检查 Git 的某一项配置

~~~bash
\$ git config user.name
John Doe
~~~

由于 Git 会从多个文件中读取同一配置变量的不同值，因此你可能会在其中看到意料之外的值而不知道为什么。 此时，你可以查询 Git 中该变量的 原始值，它会告诉你哪一个配置文件最后设置了该值：
\$ git config --show-origin rerere.autoUpdate
file:/home/johndoe/.gitconfig   false

### 获取帮助

若你使用 Git 时需要获取帮助，有三种等价的方法可以找到 Git 命令的综合手册（manpage）：

~~~bash
\$ git help '<'verb'>'
\$ git '<'verb'>' --help
\$ man git-'<'verb'>'
~~~

例如，要想获得 git config命令的手册，执行
\$ git help config

如果你觉得手册或者本书的内容还不够用，你可以尝试在 Freenode IRC 服务器 [https://freenode.net] 上的 #git或 #github频道寻求帮助。
如果你不需要全面的手册，只需要可用选项的快速参考，那么可以用 -h选项获得更简明的 “help” 输出：
\$ git add -h
usage: git add [`'<'`options`'>'`] [--] `'<'`pathspec`'>'`...
-n, --dry-run         dry run
-v, --verbose         be verbose
-i, --interactive     interactive picking
-p, --patch           select hunks interactively
-e, --edit            edit current diff and apply
-f, --force           allow adding otherwise ignored files
-u, --update          update tracked files
--renormalize         renormalize EOL of tracked files (implies -u)
-N, --intent-to-add   record only the fact that the path will be addedlater
-A, --all             add changes from all tracked and untracked files
--ignore-removal      ignore paths removed in the working tree (sameas --no-all)
--refresh             don't add, only refresh the index
--ignore-errors       just skip files which cannot be added because oferrors
--ignore-missing      check if - even missing - files are ignored indry run
--chmod (+|-)x        override the executable bit of the listed files

## Git基础

### 获取Git仓库

通常有两种获取 Git 项目仓库的方式：
1.将尚未进行版本控制的本地目录转换为 Git 仓库；
2.从其它服务器 克隆一个已存在的 Git 仓库。
两种方式都会在你的本地机器上得到一个工作就绪的 Git 仓库。

#### 在已存在目录中初始化仓库

首先需要进入该项目目录中
在 Linux 上：

~~~bash
\$ cd /home/user/my_project
~~~

在 macOS 上：

~~~bash
\$ cd /Users/user/my_project
~~~

在 Windows 上：

~~~bash
\$ cd /c/user/my_project
~~~

初始化Git仓库，执行：
\$ git init
该命令将创建一个名为 .git的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是Git 仓库的骨干。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。

如果在一个已存在文件的文件夹（而非空文件夹）中进行版本控制，你应该开始追踪这些文件并进行初始提交。可以通过 git add命令来指定所需的文件来进行追踪，然后执行 git commit：

~~~bash
\$ git add *.c
\$ git add LICENSE
\$ git commit -m 'initial project version'
~~~

### 克隆现有的仓库

如果你想获得一份已经存在了的 Git 仓库的拷贝，比如说，你想为某个开源项目贡献自己的一份力，这时就要用到 git clone命令。Git 克隆的是该 Git 仓库服务器上的几乎所有数据，而不是仅仅复制完成你的工作所需要文件。 当你执行 git clone命令的时候，默认配置下远程 Git 仓库中的每一个文件的每一个版本都将被拉取下来。

克隆仓库的命令是 git clone `'<'`url`'>'`。比如，要克隆 Git 的链接库 libgit2，可以用下面的命令：

~~~bash
\$ git clone https://github.com/libgit2/libgit2
~~~

这会在当前目录下创建一个名为 “libgit2” 的目录，并在这个目录下初始化一个 .git文件夹， 从远程仓库拉取下所有数据放入 .git文件夹，然后从中读取最新版本的文件的拷贝。

如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以通过额外的参数指定新的目录名：

~~~bash
\$ git clone https://github.com/libgit2/libgit2 mylibgit
~~~

这会执行与上一条命令相同的操作，但目标目录名变为了 mylibgit。

Git 支持多种数据传输协议。 上面的例子使用的是 https://协议，不过你也可以使用 git://协议或者使用SSH 传输协议，比如 user@server:path/to/repo.git。

工作目录下的每一个文件都不外乎这两种状态：已跟踪或 未跟踪。 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后， 它们的状态可能是未修改，已修改或已放入暂存区。简而言之，已跟踪的文件就是 Git 已经知道的文件。
工作目录中除已跟踪文件外的其它所有文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有被放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态，因为 Git刚刚检出了它们， 而你尚未编辑过它们。

### 检查当前文件状态

用 git status命令查看哪些文件处于什么状态。如果在克隆仓库后立即使用此命令，会看到类似这样的输出：
\$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean

在项目下创建一个新的 README 文件
\$ echo 'My Project' '>' README
如果之前并不存在这个文件，使用 git status命令，你将看到一个新的未跟踪文件：
\$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:  
(use "git add ''<''file''>''..." to include in what will be committed)
READMEnothing added to commit but untracked files present (use "git add" totrack)

### 跟踪新文件

使用命令 git add开始跟踪一个文件。所以，要跟踪 README文件，运行：
\$ git add README
再运行 git status命令，会看到 README文件已被跟踪，并处于暂存状态：
\$ git statusOn branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:  
(use "git restore --staged ''<''file''>''..." to unstage)
new file:   README
只要在 Changes to be committed这行下面的，就说明是已暂存状态。git add命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。

### 暂存已修改的文件

如果你修改了一个名为 CONTRIBUTING.md的已被跟踪的文件，然后运行 git status命令，会看到下面内容：
\$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:  
(use "git reset HEAD '<'file'>'..." to unstage)
new file:   README
Changes not staged for commit:  
(use "git add '<'file'>'..." to update what will be committed)  
(use "git checkout -- '<'file'>'..." to discard changes in workingdirectory)
modified:   CONTRIBUTING.md
文件 CONTRIBUTING.md出现在 Changes not staged for commit这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。要暂存这次更新，需要运行 git add命令。
运行了 gitadd之后又作了修订的文件，需要重新运行 git add把最新版本重新暂存起来。

### 状态简览

git status命令的输出十分详细，但其用语有些繁琐。 Git 有一个选项可以缩短状态命令的输出，以简洁的方式查看更改。 使用 git status -s 命令或 git status --short 命令，将得到一种格式更为紧凑的输出。
\$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
新添加的未跟踪文件前面有 ?? 标记，新添加到暂存区中的文件前面有 A 标记，修改过的文件前面有 M 标记。输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态。例如，上面的状态报告显示： README文件在工作区已修改但尚未暂存，而 lib/simplegit.rb文件已修改且已暂存。 Rakefile文件已修，暂存后又作了修改，因此该文件的修改中既有已暂存的部分，又有未暂存的部分。

### 忽略文件

有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，可以创建一个名为 .gitignore的文件，列出要忽略的文件的模式。 来看一个实际的 .gitignore例子：
\$ cat .gitignore
*.[oa]
*~
第一行告诉 Git 忽略所有以 .o或 .a结尾的文件。
第二行告诉 Git 忽略所有名字以波浪符（~）结尾的文件，许多文本编辑软件（比如 Emacs）都用这样的文件名保存副本。

.gitignore 文件的格式规范如下：
•所有空行或者以 # 开头的行都会被 Git 忽略。
•可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。
•匹配模式可以以（/）开头防止递归。
•匹配模式可以以（/）结尾指定目录。
•要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反。
所谓的 glob 模式是指 bash 所使用的简化了的正则表达式。 星号（*）匹配零个或多个任意字符；[abc]匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）； 问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配（比如 [0-9]表示匹配所有 0 到 9 的数字）。 使用两个星号（**）表示匹配任意中间目录，比如 a/**/z可以匹配 a/z、 a/b/z或 a/b/c/z等。
我们再看一个 .gitignore文件的例子：

~~~.gitignore
# 忽略所有的 .a 文件
*.a
# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a
# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO
# 忽略任何目录下名为 build 的文件夹
build/
# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt
# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
~~~

GitHub 有一个十分详细的针对数十种项目及语言的 .gitignore文件列表， 你可以在[https://github.com/github/gitignore]找到它。
在最简单的情况下，一个仓库可能只根目录下有一个 .gitignore文件，它递归地应用到整个仓库中。 然而，子目录下也可以有额外的 .gitignore文件。子目录中的 .gitignore文件中的规则只作用于它所在的目录中。 （Linux 内核的源码库拥有 206 个 .gitignore文件。）

### 查看已暂存和未暂存的修改

如果 git status 命令的输出对于你来说过于简略，而你想知道具体修改了什么地方，可以用 git diff命令。
此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。也就是修改之后还没有暂存起来的变化内容。
若要查看已暂存的将要添加到下次提交里的内容，可以用 git diff --staged命令。
\$ git diff --staged
使用 git diff 来分析文件差异,也可以使用图形化的工具或外部 diff 工具来比较差异。可以使用 git difftool 命令来调用 emerge 或 vimdiff 等软件（包括商业软件）输出 diff 的分析结果。 使用 git difftool --tool-help 命令来看你的系统支持哪些 Git Diff 插件。
\$ git difftool --tool-help

### 提交更新

在提交之前，务必确认还有什么已修改或新建的文件还没有 git add 过， 否则提交的时候不会记录这些尚未暂存的变化。已修改但未暂存的文件只会保留在本地磁盘。所以，每次准备提交前，先用 git status 看下，你所需要的文件是不是都已暂存起来了，然后再运行提交命令git commit：
\$ git commit
更详细的内容修改提示可以用 -v 选项查看，这会将你所作更改的 diff 输出呈现在编辑器中，以便让你知道本次提交具体作出哪些修改。
另外，你也可以在 commit命令后添加 -m选项，将提交信息与命令放在同一行，如下所示：
\$ git commit -m "Story 182: Fix benchmarks for speed"

### 跳过使用暂存区域

Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤：

~~~bash
\$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:  
(use "git add '<'file'>'..." to update what will be committed)  
(use "git checkout -- '<'file'>'..." to discard changes in workingdirectory)
modified:   CONTRIBUTING.md
no changes added to commit (use "git add" and/or "git commit -a")

\$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
~~~

### 移除文件

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。可以用 git rm 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。
\$ git rm PROJECTS.md
如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 -f（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删尚未添加到快照的数据，这样的数据不能被 Git 恢复。

另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 为达到这一目的，使用 --cached 选项：
\$ git rm --cached README
git rm命令后面可以列出文件或者目录的名字，也可以使用 glob 模式。比如：
\$ git rm log/\*.log
注意到星号 *之前的反斜杠 \ ， 因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 bash 来帮忙展开。此命令删除 log/目录下扩展名为 .log的所有文件。 类似的比如：
\$ git rm \*~
该命令会删除所有名字以 ~ 结尾的文件。

### 移动文件

要在 Git 中对文件改名，可以输入如下命令：
\$ git mv file_from file_to
其实，运行 git mv就相当于运行了下面三条命令：

~~~bash
\$ mv README.md README
\$ git rm README.md
\$ git add README
~~~

有时候用其他工具批处理重命名的话，要记得在提交前删除旧的文件名，再添加新的文件名。

### 查看提交历史

我们使用一个非常简单的 “simplegit” 项目作为示例。 运行下面的命令获取该项目：

~~~bash
\$ git clone https://github.com/schacon/simplegit-progit
~~~

当你在此项目中运行 git log命令时，可以看到下面的输出：

~~~bash
\$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon '<'schacon@gee-mail.com'>'
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon '<'schacon@gee-mail.com'>'
Date:   Sat Mar 15 16:40:33 2008 -0700
    removed unnecessary test
commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon '<'schacon@gee-mail.com'>'
Date:   Sat Mar 15 10:31:28 2008 -0700
    first commit
~~~

不传入任何参数的默认情况下，git log 会按时间先后顺序列出所有的提交，最近的更新排在最上面。这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。
git log 比较有用的选项是 -p 或 --patch，它会显示每次提交所引入的差异（按补丁的格式输出）。你也可以限制显示的日志条目数量，例如使用 -2 选项来只显示最近的两次提交：
\$ git log -p -2
该选项除了显示基本信息之外，还附带了每次提交的变化。可以为 git log附带一系列的总结性选项。比如你想看到每次提交的简略统计信息，可以使用 --stat 选项：
\$ git log --stat
--stat 选项在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结。
另一个非常有用的选项是 --pretty 。 这个选项可以使用不同于默认格式的方式展示提交历史。这个选项有一些内建的子选项供你使用。比如 oneline 会将每个提交放在一行显示，在浏览大量的提交时非常有用。另外还有 short ，full 和 fuller 选项，它们展示信息的格式基本一致，但是详尽程度不一
\$ git log --pretty=oneline
最有意思的是 format，可以定制记录的显示格式。 这样的输出对后期提取分析格外有用——因为你知道输出的格式不会随着 Git 的更新而发生改变：
\$ git log --pretty=format:"%h - %an, %ar : %s"
git log --pretty=format 常用的选项
选项    说明
%H      提交的完整哈希值
%h      提交的简写哈希值
%T      树的完整哈希值
%t      树的简写哈希值
%P      父提交的完整哈希值
%p      父提交的简写哈希值
%an     作者名字
%ae     作者的电子邮件地址
%ad     作者修订日期（可以用 --date=选项 来定制格式）
%ar     作者修订日期，按多久以前的方式显示
%cn     提交者的名字
%ce     提交者的电子邮件地址
%cd     提交日期
%cr     提交日期（距今多长时间）
%s      提交说明
作者指的是实际作出修改的人，提交者指的是最后将此工作成果提交到仓库的人。
当 oneline 或 format 与另一个 log 选项 --graph 结合使用时尤其有用。 这个选项添加了一些 ASCII 字符串来形象地展示你的分支、合并历史：
\$ git log --pretty=format:"%h %s" --graph
git log 的常用选项
选项                说明
-p                  按补丁格式显示每个提交引入的差异。
--stat              显示每次提交的文件修改统计信息。
--shortstat         只显示 --stat 中最后的行数修改添加移除统计。
--name-only         仅在提交信息后显示已修改的文件清单。
--name-status       显示新增、修改、删除的文件清单。
--abbrev-commit     仅显示 SHA-1 校验和所有 40 个字符中的前几个字符。
--relative-date     使用较短的相对时间而不是完整格式显示日期（比如“2 weeks ago”）。
--graph             在日志旁以 ASCII 图形显示分支与合并历史。
--pretty            使用其他格式显示历史提交信息。可用的选项包括 oneline、short、full、fuller 和format（用来定义自己的格式）。
--oneline           --pretty=oneline --abbrev-commit合用的简写。

### 限制输出长度

可以使用类似 -'<'n'>' 的选项，其中的n可以是任何整数，表示仅显示最近的n条提交。不过实践中这个选项不是很常用，因为 Git 默认会将所有的输出传送到分页程序中，所以你一次只会看到一页的内容。
类似 --since 和 --until 这种按照时间作限制的选项很有用。例如，下面的命令会列出最近两周的所有提交：
\$ git log --since=2.weeks
该命令可用的格式十分丰富——可以是类似 "2008-01-15"的具体的某一天，也可以是类似 "2 years 1 day3 minutes ago"的相对日期。
还可以过滤出匹配指定条件的提交。用 --author选项显示指定作者的提交，用 --grep 选项搜索提交说明中的关键字。可以指定多个 --author 和 --grep 搜索条件，这样会只输出任意匹配 --author 模式和 --grep 模式的提交。然而，如果你添加了 --all-match 选项，则只会输出所有匹配--grep 模式的提交。
另一个非常有用的过滤器是 -S（俗称“pickaxe”选项），它接受一个字符串参数，并且只会显示那些添加或删除了该字符串的提交。假设你想找出添加或删除了对某一个特定函数的引用的提交，可以调用：
\$ git log -S function_name
最后一个很实用的 git log 选项是路径（path），如果只关心某些文件或者目录的历史提交，可以在 git log 选项的最后指定它们的路径。
限制 git log 输出的选项
选项                    说明
-'<'n'>'                    仅显示最近的 n 条提交。
--since, --after        仅显示指定时间之后的提交。
--until, --before       仅显示指定时间之前的提交。
--author                仅显示作者匹配指定字符串的提交。
--committer             仅显示提交者匹配指定字符串的提交。
--grep                  仅显示提交说明中包含指定字符串的提交。
-S                      仅显示添加或删除内容匹配指定字符串的提交。
如果要在 Git 源码库中查看 Junio Hamano 在 2008 年 10 月其间， 除了合并提交之外的哪一个提交修改了测试文件，可以使用下面的命令：
\$ git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" \   --before="2008-11-01" --no-merges -- t/

隐藏合并提交
按照你代码仓库的工作流程，记录中可能有为数不少的合并提交，它们所包含的信息通常并不多。为了避免显示的合并提交弄乱历史记录，可以为 log 加上 --no-merges 选项。

### 撤消操作

注意，有些撤消操作是不可逆的。这是在使用 Git 的过程中，会因为操作失误而导致之前的工作丢失的少有的几个地方之一。
运行带有 --amend 选项的提交命令来重新提交：
\$ git commit --amend
这个命令会将暂存区中的文件提交。如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令）， 那么快照会保持不变，而你所修改的只是提交信息。
例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：
\$ git commit -m 'initial commit'
\$ git add forgotten_file
\$ git commit --amend
最终你只会有一个提交——第二次提交将代替第一次提交的结果。

### 取消暂存的文件

~~~bash
\$ git add *
\$ git status
On branch masterChanges to be committed:
  (use "git reset HEAD '<'file'>'..." to unstage)
    renamed:    README.md -'>' README
    modified:   CONTRIBUTING.md

在 “Changes to be committed” 文字正下方，提示使用 git reset HEAD '<'file'>'...来取消暂存。所以，我们可以这样来取消暂存 CONTRIBUTING.md 文件：
\$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M   CONTRIBUTING.md
\$ git status
On branch masterChanges to be committed:
  (use "git reset HEAD '<'file'>'..." to unstage)
    renamed:    README.md -'>' README
Changes not staged for commit:
  (use "git add '<'file'>'..." to update what will be committed)
  (use "git checkout -- '<'file'>'..." to discard changes in workingdirectory)
    modified:   CONTRIBUTING.md
~~~

### 撤消对文件的修改

如果你并不想保留对 CONTRIBUTING.md文件的修改，通过如下命令方便地撤消修改——将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚把它放入工作目录时的样子）
\$ git checkout -- CONTRIBUTING.md
请务必记得 git checkout -- '<'file'>'是一个危险的命令。 你对那个文件在本地的任何修改都会消失——Git 会用最近提交的版本覆盖掉它。

## 远程仓库的使用

管理远程仓库包括了解如何添加远程仓库、移除无效的远程仓库、管理不同的远程分支并定义它们是否被跟踪等等。

远程仓库可以在你的本地主机上
你完全可以在一个“远程”仓库上工作，而实际上它在你本地的主机上。 词语“远程”未必表示仓库在网络或互联网上的其它位置，而只是表示它在别处。 在这样的远程仓库上工作，仍然需要和其它远程仓库上一样的标准推送、拉取和抓取操作。

### 查看远程仓库

如果想查看你已经配置的远程仓库服务器，可以运行 git remote命令。它会列出你指定的每一个远程服务器的简写。如果你已经克隆了自己的仓库，那么至少应该能看到 origin ——这是 Git 给你克隆的仓库服务器的默认名字：

~~~bash
\$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.
\$ cd ticgit
\$ git remote
origin
~~~

你也可以指定选项 -v ，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。

~~~bash
\$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
~~~

如果你的远程仓库不止一个，该命令会将它们全部列出。 例如，与几个协作者合作的，拥有多个远程仓库的仓库看起来像下面这样：

~~~bash
\$ cd grit
\$ git remote -v
bakkdoor  https://github.com/bakkdoor/grit (fetch)
bakkdoor  https://github.com/bakkdoor/grit (push)
cho45     https://github.com/cho45/grit (fetch)
cho45     https://github.com/cho45/grit (push)
defunkt   https://github.com/defunkt/grit (fetch)
defunkt   https://github.com/defunkt/grit (push)
koke      git://github.com/koke/grit.git (fetch)
koke      git://github.com/koke/grit.git (push)
origin    git@github.com:mojombo/grit.git (fetch)
origin    git@github.com:mojombo/grit.git (push)
~~~

### 添加远程仓库

运行 git remote add '<'shortname'>' '<'url'>' 添加一个新的远程 Git 仓库，同时指定一个方便使用的简写：

~~~bash
\$ git remote
origin
\$ git remote add pb https://github.com/paulboone/ticgit
\$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
pb  https://github.com/paulboone/ticgit (fetch)
pb  https://github.com/paulboone/ticgit (push)
~~~

现在你可以在命令行中使用字符串 pb 来代替整个 URL。例如，如果你想拉取 Paul 的仓库中有但你没有的信息，可以运行 git fetch pb：

~~~bash
\$ git fetch pb
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 43 (delta 10), reused 31 (delta 5)
Unpacking objects: 100% (43/43), done.
From https://github.com/paulboone/ticgit
 * [new branch]      master     -'>' pb/master
 * [new branch]      ticgit     -'>' pb/ticgit
~~~

现在 Paul 的 master 分支可以在本地通过 pb/master访问到——你可以将它合并到自己的某个分支中，或者如果你想要查看它的话，可以检出一个指向该点的本地分支。

### 从远程仓库中抓取与拉取

从远程仓库中获得数据，可以执行：

~~~bash
\$ git fetch '<'remote'>'
~~~

这个命令会访问远程仓库，从中拉取所有你还没有的数据。执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。
如果你使用 clone 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。所以，git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作。必须注意 git fetch 命令只会将数据下载到你的本地仓库——它并不会自动合并或修改你当前的工作。当准备好时你必须手动将其合并入你的工作。
如果你的当前分支设置了跟踪远程分支（阅读下一节和 Git 分支了解更多信息），那么可以用 git pull 命令来自动抓取后合并该远程分支到当前分支。

### 推送到远程仓库

当你想分享项目时，必须将其推送到上游。这个命令很简单：git push '<'remote'>' '<'branch'>'
当你想要将 master分支推送到 origin服务器时（再次说明，克隆时通常会自动帮你设置好那两个名字），那么运行这个命令就可以将你所做的备份到服务器：
\$ git push origin master
只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。你必须先抓取他们的工作并将其合并进你的工作后才能推送。

### 查看某个远程仓库

如果想要查看某一个远程仓库的更多信息，可以使用 git remote show '<'remote'>'命令。如果想以一个特定的缩写名运行这个命令，例如 origin，会得到像下面类似的信息：

~~~bash
\$ git remote show origin
* remote origin
  Fetch URL: https://github.com/schacon/ticgit
  Push  URL: https://github.com/schacon/ticgit
  HEAD branch: master
  Remote branches:
    master                               tracked
    dev-branch                           tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
~~~

它同样会列出远程仓库的 URL 与跟踪分支的信息。 这些信息非常有用，它告诉你正处于 master分支，并且如果运行 git pull，就会抓取所有的远程引用，然后将远程 master分支合并到本地 master分支。它也会列出拉取到的所有远程引用。
如果你是 Git 的重度使用者，那么还可以通过 git remote show 看到更多的信息。
\$ git remote show
这个命令列出了当你在特定的分支上执行 git push会自动地推送到哪一个远程分支。 它也同样地列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了， 还有当你执行 git pull时哪些本地分支可以与它跟踪的远程分支自动合并。

### 远程仓库的重命名与移除

你可以运行 git remote rename 来修改一个远程仓库的简写名。例如，想要将 pb 重命名为 paul，可以用 git remote rename 这样做：

~~~bash
\$ git remote rename pb paul
\$ git remote
origin
paul
~~~

值得注意的是这同样也会修改你所有远程跟踪的分支名字。那些过去引用 pb/master的现在会引用paul/master。

如果因为一些原因想要移除一个远程仓库——你已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了——可以使用 git remote remove 或 git remote rm：

~~~bash
\$ git remote remove paul
\$ git remote
origin
~~~

一旦你使用这种方式删除了一个远程仓库，那么所有和这个远程仓库相关的远程跟踪分支以及配置信息也会一起被删除。

### 打标签

Git 可以给仓库历史中的某一个提交打上标签，以示重要。比较有代表性的是人们会使用这个功能来标记发布结点（ v1.0、 v2.0等等）。

#### 列出标签

在 Git 中列出已有的标签非常简单，只需要输入 git tag （可带上可选的 -l 选项 --list ）：

~~~bash
\$ git tag
v1.0
v2.0
~~~

你也可以按照特定的模式查找标签。例如，Git 自身的源代码仓库包含标签的数量超过500个。如果只对1.8.5
系列感兴趣，可以运行：

~~~bash
\$ git tag -l "v1.8.5*"
~~~

按照通配符列出标签需要 -l或 --list选项如果你只想要完整的标签列表，那么运行 git tag就会默认假定你想要一个列表，它会直接给你列出来， 此时的 -l或 --list是可选的。然而，如果你提供了一个匹配标签名的通配模式，那么 -l或 --list就是强制使用的。

#### 创建标签

Git 支持两种标签：轻量标签（lightweight）与附注标签（annotated）。
轻量标签很像一个不会改变的分支——它只是某个特定提交的引用。
附注标签是存储在 Git 数据库中的一个完整对象， 它们是可以被校验的，其中包含打标签者的名字、电子邮件地址、日期时间， 此外还有一个标签信息，并且可以使用 GNU Privacy Guard （GPG）签名并验证。通常会建议创建附注标签，这样你可以拥有以上所有信息。

#### 附注标签

在 Git 中创建附注标签十分简单。最简单的方式是当你在运行 tag 命令时指定 -a 选项：

~~~bash
\$ git tag -a v1.4 -m "my version 1.4"
\$ git tag
v0.1
v1.3
v1.4
~~~

-m选项指定了一条将会存储在标签中的信息。如果没有为附注标签指定一条信息，Git 会启动编辑器要求你输入信息。通过使用 git show 命令可以看到标签信息和与之对应的提交信息：

~~~bash
\$ git show v1.4
tag v1.4
Tagger: Ben Straub '<'ben@straub.cc'>'
Date:   Sat May 3 20:19:12 2014 -0700
my version 1.4
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon '<'schacon@gee-mail.com'>'
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
~~~

输出显示了打标签者的信息、打标签的日期时间、附注信息，然后显示具体的提交信息。

#### 轻量标签

轻量标签本质上是将提交校验和存储到一个文件中——没有保存任何其他信息。创建轻量标签，不需要使用 -a 、-s 或 -m 选项，只需要提供标签名字：

~~~bash
\$ git tag v1.4-lw
\$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
~~~

这时，如果在标签上运行 git show ，你不会看到额外的标签信息。命令只会显示出提交信息：

~~~bash
\$ git show v1.4-lw
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon '<'schacon@gee-mail.com'>'
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
~~~

#### 后期打标签

你也可以对过去的提交打标签。 假设提交历史是这样的：

~~~bash
\$ git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
4682c3261057305bdd616e23b64b0857d832627b added a todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme
~~~

假设在 v1.2 时你忘记给项目打标签，也就是在 “updated rakefile” 提交。 你可以在之后补上标签。 要在那个提交上打标签，你需要在命令的末尾指定提交的校验和（或部分校验和）：
\$ git tag -a v1.2 9fceb02
可以看到你已经在那次提交上打上标签了：

~~~bash
\$ git tag
v0.1
v1.2
v1.3
v1.4
v1.4-lw
v1.5
\$ git show v1.2
tag v1.2
Tagger: Scott Chacon '<'schacon@gee-mail.com'>'
Date:   Mon Feb 9 15:32:16 2009 -0800
version 1.2
commit 9fceb02d0ae598e95dc970b74767f19372d61af8
Author: Magnus Chacon '<'mchacon@gee-mail.com'>'
Date:   Sun Apr 27 20:43:35 2008 -0700
    updated rakefile...
~~~

#### 共享标签

默认情况下，git push命令并不会传送标签到远程仓库服务器上。在创建完标签后你必须显式地推送标签到共享服务器上。 这个过程就像共享远程分支一样——你可以运行 git push origin '<'tagname'>'。

~~~bash
\$ git push origin v1.5
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.5 -'>' v1.5
~~~

如果想要一次性推送很多标签，也可以使用带有 --tags 选项的 git push 命令。这将会把所有不在远程仓库服务器上的标签全部传送到那里。

~~~bash
\$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)To git@github.com:schacon/simplegit.git
 * [new tag]         v1.4 -'>' v1.4
 * [new tag]         v1.4-lw -'>' v1.4-lw
~~~

git push 推送两种标签使用 git push '<'remote'>' --tags 推送标签并不会区分轻量标签和附注标签， 没有简单的选项能够让你只选择推送一种标签。

#### 删除标签

要删除掉你本地仓库上的标签，可以使用命令 git tag -d '<'tagname'>'。
例如，可以使用以下命令删除一个轻量标签：

~~~bash
\$ git tag -d v1.4-lw
Deleted tag 'v1.4-lw' (was e7d5add)
~~~

注意上述命令并不会从任何远程仓库中移除这个标签，你必须用 git push '<'remote'>':refs/tags/'<'tagname'>'来更新你的远程仓库：
第一种变体是

~~~bash
\$ git push '<'remote'>' :refs/tags/'<'tagname'>'：
~~~

~~~bash
\$ git push origin :refs/tags/v1.4-lw
To /git@github.com:schacon/simplegit.git
 - [deleted]         v1.4-lw
~~~

第二种更直观的删除远程标签的方式是：

~~~bash
\$ git push origin --delete '<'tagname'>'
~~~

#### 检出标签

如果你想查看某个标签所指向的文件版本，可以使用 git checkout 命令， 虽然这会使你的仓库处于“分离头指针（detached HEAD）”的状态——这个状态有些不好的副作用。

~~~bash
\$ git checkout 2.0.0
Note: checking out '2.0.0'.
You are in 'detached HEAD' state. You can look around, make experimentalchanges and commit them, and you can discard any commits you make in thisstate without impacting any branches by performing another checkout.
If you want to create a new branch to retain commits you create, you maydo so (now or later) by using -b with the checkout command again. Example:
  git checkout -b '<'new-branch'>'
HEAD is now at 99ada87... Merge pull request #89 from schacon/appendix-final
\$ git checkout 2.0-beta-0.1
Previous HEAD position was 99ada87... Merge pull request #89 fromschacon/appendix-final
HEAD is now at df3f601... add atlas.json and cover image
~~~

在“分离头指针”状态下，如果你做了某些更改然后提交它们，标签不会发生变化，但你的新提交将不属于任何分支，并且将无法访问，除非通过确切的提交哈希才能访问。 因此，如果你需要进行更改，比如你要修复旧版本中的错误，那么通常需要创建一个新分支：

~~~bash
\$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
~~~

如果在这之后又进行了一次提交，version2分支就会因为这个改动向前移动，此时它就会和v2.0.0标签稍微有些不同，这时就要当心了。

### Git别名

如果不想每次都输入完整的 Git 命令，可以通过 gitconfig 文件来轻松地为每一个命令设置一个别名。这里有一些例子你可以试试：

~~~bash
\$ git config --global alias.co checkout
\$ git config --global alias.br branch
\$ git config --global alias.ci commit
\$ git config --global alias.st status
~~~

这意味着，当要输入 git commit时，只需要输入 git ci。
为了解决取消暂存文件的易用性问题，可以向 Git 中添加你自己的取消暂存别名：

~~~bash
\$ git config --global alias.unstage 'reset HEAD --'
~~~

这会使下面的两个命令等价：

~~~bash
\$ git unstage fileA
\$ git reset HEAD -- fileA
~~~

这样看起来更清楚一些。 通常也会添加一个 last 命令，像这样：

~~~bash
\$ git config --global alias.last 'log -1 HEAD'
~~~

这样，可以轻松地看到最后一次提交：

~~~bash
\$ git last
commit 66938dae3329c7aebe598c2246a8e6af90d04646
Author: Josh Goebel '<'dreamer3@example.com'>'
Date:   Tue Aug 26 19:48:51 2008 +0800
    test for current head
    Signed-off-by: Scott Chacon '<'schacon@example.com'>'
~~~

可以看出，Git 只是简单地将别名替换为对应的命令。 然而，你可能想要执行外部命令，而不是一个 Git 子命令。 如果是那样的话，可以在命令前面加入  !符号。 如果你自己要写一些与 Git 仓库协作的工具的话，那会很有用。 我们现在演示将 git visual定义为 gitk的别名：

~~~bash
\$ git config --global alias.visual '!gitk'
~~~

## Git分支

### 分支简介

在进行提交操作时，Git 会保存一个提交对象（commit object）。 该提交对象会包含一个指向暂存内容快照的指针。 但不仅仅是这样，该提交对象还包含了作者的姓名和邮箱、提交时输入的信息以及指向它的父对象的指针。首次提交产生的提交对象没有父对象，普通提交操作产生的提交对象有一个父对象， 而由多个分支合并产生的提交对象有多个父对象.
当使用 git commit进行提交操作时，Git 会先计算每一个子目录（本例中只有项目根目录）的校验和，然后在 Git 仓库中这些校验和保存为树对象。 随后，Git 便会创建一个提交对象，它除了包含上面提到的那些信息外，还包含指向这个树对象（项目根目录）的指针。 如此一来，Git 就可以在需要的时候重现此次保存的快照。
现在，Git 仓库中有五个对象：三个 blob 对象（保存着文件快照）、一个 树对象 （记录着目录结构和 blob 对象索引）以及一个 提交对象（包含着指向前述树对象的指针和所有提交信息）。
Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 master 。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 master分支会在每次提交时自动向前移动。
Git 的 master分支并不是一个特殊分支。它就跟其它分支完全没有区别。 之所以几乎每一个仓库都有 master 分支，是因为 git init命令默认创建它，并且大多数人都懒得去改动它。

### 分支创建

比如，创建一个 testing 分支， 你需要使用 git branch命令：

~~~bash
\$ git branch testing
~~~

Git 有一个名为 HEAD 的特殊指针，指示当前在哪一个分支上。git branch 命令仅仅创建一个新分支，并不会自动切换到新分支中去。

可以简单地使用 git log 命令查看各个分支当前所指的对象。 提供这一功能的参数是 --decorate。

~~~bash
\$ git log --oneline --decorate
f30ab (HEAD -'>' master, testing) add feature #32 - ability to add newformats to the central interface
34ac2 Fixed bug #1328 - stack overflow under certain conditions
98ca9 The initial commit of my project
~~~

### 分支切换

要切换到一个已存在的分支，你需要使用 git checkout 命令。我们现在切换到新创建的 testing 分支去：

~~~bash
\$ git checkout testing
~~~

这样 HEAD就指向 testing 分支了。

切换回 master分支

~~~bash
\$ git checkout master
~~~

在切换分支时，一定要注意你工作目录里的文件会被改变。 如果是切换到一个较旧的分支，你的工作目录会恢复到该分支最后一次提交时的样子。 如果 Git 不能干净利落地完成这个任务，它将禁止切换分支。

可以简单地使用 git log 命令查看分叉历史。 运行 git log --oneline --decorate --graph--all ，它会输出你的提交历史、各个分支的指向以及项目的分支分叉情况。

~~~bash
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the
* 34ac2 fixed bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
~~~

### 分支的新建与合并

#### 新建分支

假设你正在你的项目上工作，并且在 master分支上已经有了一些提交。
现在，你已经决定要解决你的公司使用的问题追踪系统中的 #53 问题。 想要新建一个分支并同时切换到那个分支上，你可以运行一个带有 -b 参数的 git checkout 命令：

~~~bash
\$ git checkout -b iss53
Switched to a new branch "iss53"
~~~

它是下面两条命令的简写：

~~~bash
\$ git branch iss53
\$ git checkout iss53
~~~

你继续在 #53 问题上工作，并且做了一些提交。 在此过程中，iss53分支在不断的向前推进，因为你已经检出到该分支（也就是说，你的 HEAD 指针指向了 iss53 分支）

~~~bash
\$ vim index.html
\$ git commit -a -m 'added a new footer [issue 53]'
~~~

有了 Git 的帮助，你不必把这个紧急问题和 iss53 的修改混在一起，你也不需要花大力气来还原关于 53# 问题的修改，然后再添加关于这个紧急问题的修改，最后将这个修改提交到线上分支。 你所要做的仅仅是切换回 master 分支。
但是，在你这么做之前，要留意你的工作目录和暂存区里那些还没有被提交的修改，它可能会和你即将检出的分支产生冲突从而阻止 Git 切换到该分支。 最好的方法是，在你切换分支之前，保持好一个干净的状态。 有一些方法可以绕过这个问题（即，暂存（stashing） 和 修补提交（commit amending）），我们会在 贮藏与清理中 看到关于这两个命令的介绍。 现在，我们假设你已经把你的修改全部提交了，这时你可以切换回 master分支了：

~~~bash
\$ git checkout master
Switched to branch 'master'
~~~

这个时候，你的工作目录和你在开始 #53 问题之前一模一样。
请牢记：当你切换分支的时候，Git 会重置你的工作目录，使其看起来像回到了你在那个分支上最后一次提交的样子。
Git 会自动添加、删除、修改文件以确保此时你的工作目录和这个分支最后一次提交时的样子一模一样。
接下来，你要修复这个紧急问题。 我们来建立一个 hotfix分支，在该分支上工作直到问题解决：

~~~bash
\$ git checkout -b hotfix
Switched to a new branch 'hotfix'
\$ vim index.html$ git commit -a -m 'fixed the broken email address'
[hotfix 1fb7853] fixed the broken email address
 1 file changed, 2 insertions(+)
~~~

你可以运行你的测试，确保你的修改是正确的，然后将 hotfix分支合并回你的 master分支来部署到线上。你可以使用 git merge命令来达到上述目的：

~~~bash
\$ git checkout master
\$ git merge hotfix
Updating f42c576..3a0874cFast-forward index.html | 2 ++
 1 file changed, 2 insertions(+)
~~~

在合并的时候，你应该注意到了“快进（fast-forward）”这个词。 由于你想要合并的分支 hotfix 所指向的提交 C4 是你所在的提交 C2 的直接后继，因此 Git 会直接将指针向前移动。换句话说，当你试图合并两个分支时，如果顺着一个分支走下去能够到达另一个分支，那么 Git 在合并两者的时候，只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧——这就叫做 “快进（fast-forward）”。
关于这个紧急问题的解决方案发布之后，你准备回到被打断之前时的工作中。 然而，你应该先删除 hotfix 分支，因为你已经不再需要它了 —— master分支已经指向了同一个位置。 你可以使用带 -d 选项的 git branch 命令来删除分支：

~~~bash
\$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
~~~

现在你可以切换回你正在工作的分支继续你的工作，也就是针对 #53 问题的那个分支（iss53 分支）。

~~~bash
\$ git checkout iss53
Switched to branch "iss53"
\$ vim index.html
\$ git commit -a -m 'finished the new footer [issue 53]'
[iss53 ad82d7a] finished the new footer [issue 53]
1 file changed, 1 insertion(+)
~~~

你在 hotfix 分支上所做的工作并没有包含到 iss53 分支中。 如果你需要拉取 hotfix 所做的修改，你可以使用 git merge master 命令将 master分支合并入 iss53 分支，或者你也可以等到 iss53 分支完成其使命，再将其合并回 master分支。

#### 分支的合并

假设你已经修正了 #53 问题，并且打算将你的工作合并入 master 分支。 为此，你需要合并 iss53 分支到master 分支，这和之前你合并 hotfix 分支所做的工作差不多。 你只需要检出到你想合并入的分支，然后运行 git merge 命令：

~~~bash
\$ git checkout master
Switched to branch 'master'
\$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
~~~

修改合并之后，就不再需要 iss53 分支了。 现在你可以在任务追踪系统中关闭此项任务，并删除这个分支。

~~~bash
\$ git branch -d iss53
~~~

#### 遇到冲突时的分支合并

如果你在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，Git 就没法干净的合并它们。如果你对 #53 问题的修改和有关 hotfix 分支的修改都涉及到同一个文件的同一处，在合并它们的时候就会产生合并冲突：

~~~bash
\$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
~~~

此时 Git 做了合并，但是没有自动地创建一个新的合并提交。 Git 会暂停下来，等待你去解决合并产生的冲突。 你可以在合并冲突后的任意时刻使用 git status 命令来查看那些因包含合并冲突而处于未合并（unmerged）状态的文件：

~~~bash
\$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
Unmerged paths:
  (use "git add '<'file'>'..." to mark resolution)
    both modified:      index.html
no changes added to commit (use "git add" and/or "git commit -a")
~~~

任何因包含合并冲突而有待解决的文件，都会以未合并状态标识出来。 Git 会在有冲突的文件中加入标准的冲突解决标记，这样你可以打开这些包含冲突的文件然后手动解决冲突。 出现冲突的文件会包含一些特殊区段，看起来像下面这个样子：

~~~bash
'<''<''<''<''<''<''<' HEAD:index.html
'<'div id="footer"'>'contact : email.support@github.com'<'/div'>'
=======
'<'div id="footer"'>'
 please contact us at support@github.com
'<'/div'>'
'>''>''>''>''>''>''>' iss53:index.html
~~~

这表示 HEAD 所指示的版本（也就是你的 master 分支所在的位置，因为你在运行 merge 命令的时候已经检出到了这个分支）在这个区段的上半部分（=======的上半部分），而 iss53 分支所指示的版本在 ======= 的下半部分。
为了解决冲突，你必须选择使用由 =======分割的两部分中的一个，或者你也可以自行合并这些内容。 例如，你可以通过把这段内容换成下面的样子来解决冲突：

~~~bash
'<'div id="footer"'>'
please contact us at email.support@github.com
'<'/div'>'
~~~

上述的冲突解决方案仅保留了其中一个分支的修改，并且 '<''<''<''<''<''<''<' , ======= , 和 '>''>''>''>''>''>''>' 这些行被完全删除了。在你解决了所有文件里的冲突之后，对每个文件使用 git add 命令来将其标记为冲突已解决。 一旦暂存这些原本有冲突的文件，Git 就会将它们标记为冲突已解决。
如果你想使用图形化工具来解决冲突，你可以运行 git mergetool ，该命令会为你启动一个合适的可视化合并工具，并带领你一步一步解决这些冲突：

~~~bash
\$ git mergetool
This message is displayed because 'merge.tool' is not configured.
See 'git mergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse
diffmerge ecmerge p4merge araxis bc3 codecompare vimdiff emerge
Merging:
index.html
Normal merge conflict for 'index.html':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (opendiff):
~~~

如果你想使用除默认工具（在这里 Git 使用 opendiff 做为默认的合并工具，因为作者在 Mac 上运行该程序）外的其他合并工具，你可以在 “下列工具中（one of the following tools）” 这句后面看到所有支持的合并工具。 然后输入你喜欢的工具名字就可以了。

等你退出合并工具之后，Git 会询问刚才的合并是否成功。 如果你回答是，Git 会暂存那些文件以表明冲突已解决： 你可以再次运行 git status 来确认所有的合并冲突都已被解决：

~~~bash
\$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
Changes to be committed:
    modified:   index.html79
~~~

如果你对结果感到满意，并且确定之前有冲突的的文件都已经暂存了，这时你可以输入 git commit 来完成合并提交。 默认情况下提交信息看起来像下面这个样子：

~~~bash
Merge branch 'iss53'
Conflicts:
    index.html
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#   .git/MERGE_HEAD# and try again.
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master# All conflicts fixed but you are still merging.
#
# Changes to be committed:
#   modified:   index.html
#
~~~

### 分支管理

git branch命令不只是可以创建与删除分支。如果不加任何参数运行它，会得到当前所有分支的一个列表：

~~~bash
\$ git branch
  iss53
* master
  testing
~~~

注意 master分支前的 *字符：它代表现在检出的那一个分支（也就是说，当前 HEAD指针所指向的分支）。这意味着如果在这时候提交，master分支将会随着新的工作向前移动。 如果需要查看每一个分支的最后一次提交，可以运行 git branch -v命令：

~~~bash
\$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
~~~

--merged 与 --no-merged 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。 如果要查看哪些分支已经合并到当前分支，可以运行 git branch --merged：

~~~bash
$ git branch --merged
  iss53
* master
~~~

因为之前已经合并了 iss53分支，所以现在看到它在列表中。 在这个列表中分支名字前没有 *号的分支通常可以使用 git branch -d删除掉；你已经将它们的工作整合到了另一个分支，所以并不会失去任何东西。

查看所有包含未合并工作的分支，可以运行 git branch --no-merged：

~~~bash
\$ git branch --no-merged
  testing
~~~

这里显示了其他分支。 因为它包含了还未合并的工作，尝试使用 git branch -d 命令删除它时会失败：

~~~bash
\$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.
~~~

如果真的想要删除分支并丢掉那些工作，如同帮助信息里所指出的，可以使用 -D 选项强制删除它。

选项 --merged 和 --no-merged 会在没有给定提交或分支名作为参数时，分别列出已合并或未合并到当前分支的分支。可以通过附加的参数来查看其它分支的合并状态而不必检出它们。 例如，尚未合并到 master 分支的有哪些：

~~~bash
\$ git checkout testing
\$ git branch --no-merged master
  topicA
  featureB81
~~~

