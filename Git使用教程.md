# Git学习笔记

## 1 缩略语

集中化的版本控制系统（Centralized Version Control Systems，简称 CVCS）

## 2 基础知识

Git 有三种状态，用户的文件可能处于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。

•已修改：表示修改了文件，但还没保存到数据库中。

•已暂存：表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

•已提交：表示数据已经安全地保存在本地数据库中。

这会让我们的 Git 项目拥有三个阶段：工作区、暂存区以及 Git 目录。

工作区是对项目的某个版本独立提取出来的内容。这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供用户使用或修改。

暂存区是一个文件，保存了下次将要提交的文件列表信息，一般在 Git 仓库目录中。按照 Git 的术语叫做“索引”，不过一般说法还是叫“暂存区”。

Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。这是 Git 中最重要的部分，从其它计算机克隆仓库时，复制的就是这里的数据。

基本的 Git 工作流程如下：

1.在工作区中修改文件。

2.将想要下次提交的更改选择性地暂存，这样只会将更改的部分添加到暂存区。

3.提交更新，找到暂存区的文件，将快照永久性存储到 Git 目录。

如果 Git 目录中保存着特定版本的文件，就属于已提交状态。如果文件已修改并放入暂存区，就属于已暂存状态。如果自上次检出后，作了修改但还没有放到暂存区域，就是已修改状态。

## 3 Git安装

### 3.1 Linux

以 Fedora 为例，如果用户在使用它（或与之紧密相关的基于 RPM 的发行版，如 RHEL 或 CentOS），可以使用 dnf ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo dnf install git-all
~~~

如果在基于 Debian 的发行版上，如 Ubuntu，请使用 apt ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo apt install git-all
~~~

下载地址
[https://git-scm.com/download/linux]

### 3.2 macOS

在 Mac 上安装 Git 有多种方式。最简单的方法是安装 Xcode Command Line Tools 。Mavericks （10.9） 或更高版本的系统中，在 Terminal 里尝试首次运行 git 命令即可。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git --version
~~~

下载地址
[https://git-scm.com/download/mac]

### 3.3 Windows

在 Windows 上安装 Git 也有几种安装方法。 官方版本可以在 Git 官方网站下载。打开 [https://git-scm.com/download/win] ，下载会自动开始。要注意这是一个名为 Git for Windows 的项目（也叫做 msysGit），和 Git是分别独立的项目；更多信息请访问 [http://msysgit.github.io/] 。

要进行自动安装，可以使用 Git Chocolatey 包。注意 Chocolatey 包是由社区维护的。

另一个简单的方法是安装 GitHub Desktop 。该安装程序包含图形化和命令行版本的 Git 。它也能支持Powerbash，提供了稳定的凭证缓存和健全的换行设置。稍后会对这方面有更多了解，现在只要一句话就够了，这些都是所需要的。可以在 GitHub for Windows 网站下载，网址为 GitHub Desktop 网站。

### 3.4 从源码安装

如果从源码安装 Git，需要安装 Git 依赖的库：autotools、curl、zlib、openssl、expat 和 libiconv 。如果系统上有 dnf（如 Fedora）或者 apt（如基于 Debian 的系统），可以使用对应的命令来安装最少的依赖以便编译并安装 Git 的二进制版：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel \  openssl-devel perl-devel zlib-devel

MasterChief@DESKTOP MINGW64 ~
$ sudo apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev \  gettext libz-dev libssl-dev
~~~

为了添加文档的多种格式（doc、html、info），需要以下附加的依赖：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo dnf install asciidoc xmlto docbook2X

MasterChief@DESKTOP MINGW64 ~
$ sudo apt-get install asciidoc xmlto docbook2x
~~~

使用 RHEL 和 RHEL 衍生版，如 CentOS 和 Scientific Linux 的用户需要开启 EPEL 库以便下载 docbook2X 包。

如果使用基于 Debian 的发行版（Debian/Ubuntu/Ubuntu-derivatives），也需要 install-info 包：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo apt-get install install-info
~~~

如果使用基于 RPM 的发行版（Fedora/RHEL/RHEL衍生版），还需要 getopt 包 （它已经在基于 Debian 的发行版中预装了）：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo dnf install getopt
~~~

此外，如果使用 Fedora/RHEL/RHEL 衍生版，那么需要执行以下命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi
~~~

以此来解决二进制文件名的不同。

当安装好所有的必要依赖，可以继续从几个地方来取得最新发布版本的 tar 包。可以从 Kernel.org 网站获取，网址为 [https://www.kernel.org/pub/software/scm/git] ，或从 GitHub 网站上的镜像来获得，网址为 [https://github.com/git/git/releases] 。通常在 GitHub 上的是最新版本，但 kernel.org 上包含有文件下载签名，如果想验证下载正确性的话会用到。

编译并安装

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ tar -zxf git-2.8.0.tar.gz

MasterChief@DESKTOP MINGW64 ~
$ cd git-2.8.0

MasterChief@DESKTOP MINGW64 ~
$ make configure

MasterChief@DESKTOP MINGW64 ~
$ ./configure --prefix=/usr

MasterChief@DESKTOP MINGW64 ~
$ make all doc info

MasterChief@DESKTOP MINGW64 ~
$ sudo make install install-doc install-html install-info
~~~

使用 Git 来获取 Git 的更新

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone git://git.kernel.org/pub/scm/git/git.git
~~~

## 4 初次运行

每台计算机上只需要配置一次，程序升级时会保留配置信息。可以在任何时候再次通过运行命令来修改它们。

Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。这些变量存储在三个不同的位置：

1./etc/gitconfig 文件: 包含系统上每一个用户及他们仓库的通用配置。如果在执行 git config 时带上 --system 选项，那么它就会读写该文件中的配置变量。（由于它是系统配置文件，因此用户需要管理员或超级用户权限来修改它。）

2.~/.gitconfig或 ~/.config/git/config 文件：只针对当前用户。可以传递 --global 选项让 Git 读写此文件，这会对系统上所有的仓库生效。

3.当前使用仓库的 Git 目录中的 config 文件（即 .git/config ）：针对该仓库。可以传递 --local 选项让 Git 强制读写此文件，默认情况下用的就是它。（需要进入某个 Git 仓库中才能让该选项生效。）每一个级别会覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量。

在 Windows 系统中，Git 会查找 $HOME 目录下（一般情况下是 C:\Users\$USER ）的 .gitconfig 文件。Git 同样也会寻找 /etc/gitconfig 文件，但只限于 MSys 的根目录下，即安装 Git 时所选的目标位置。 如果在 Windows 上使用 Git 2.x 以后的版本，那么还有一个系统级的配置文件，Windows XP 上在 C:\Documents and Settings\All Users\Application Data\Git\config ，Windows Vista 及更新的版本在 C:\ProgramData\Git\config 。此文件只能以管理员权限通过 git config -f `<`file`>` 来修改。可以通过以下命令查看所有的配置以及它们所在的文件：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --list --show-origin
~~~

### 4.1 用户信息

安装完 Git 之后，要做的第一件事就是设置用户名和邮件地址。这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到每一次提交中，不可更改：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --global user.name "John Doe"

MasterChief@DESKTOP MINGW64 ~
$ git config --global user.email johndoe@example.com
~~~

如果使用了 --global 选项，那么该命令只需要运行一次，之后无论在该系统上做任何事情，Git 都会使用那些信息。当想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global 选项的命令来配置。

### 4.2 配置远程仓库

Git 服务器都会选择使用 SSH 公钥来进行授权。系统中的每个用户都必须提供一个公钥用于授权，没有的话就要生成一个。生成公钥的过程在所有操作系统上都差不多。 首先先确认一下是否已经有一个公钥了。SSH 公钥默认储存在账户的主目录下的 ~/.ssh 目录。进去看看：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cd ~/.ssh/

MasterChief@DESKTOP MINGW64 ~/.ssh
$ ls
id_rsa  id_rsa.pub  known_hosts
~~~

有 .pub 后缀的文件就是公钥，另一个文件则是密钥。假如没有这些文件，或者没有 .ssh 目录，可以用 ssh-keygen 来创建。

~~~bash
MasterChief@DESKTOP MINGW64 ~/.ssh
$ ssh-keygen
GGenerating public/private rsa key pair.
Enter file in which to save the key (/c/Users/MasterChief/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/MasterChief/.ssh/id_rsa.
Your public key has been saved in /Users/MasterChief/.ssh/id_rsa.public.
The key fingerprint is:
......
~~~

或者生成带有指定注释的KEY：

~~~bash
MasterChief@DESKTOP MINGW64 ~/.ssh
$ ssh-keygen -t rsa -C "youremail@mail.com"
......
~~~

在 Git 远程服务器上增加该密钥，即可连接到远程服务器。

### 4.3 文本编辑器

用户信息设置完毕，可以配置默认文本编辑器，当 Git 需要输入信息时会调用它。 如果未配置，Git 会使用操作系统默认的文本编辑器。如果想使用不同的文本编辑器，例如 Emacs ，可以输入如下命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --global core.editor emacs
~~~

在 Windows 系统上，如果想要使用别的文本编辑器，那么必须指定可执行文件的完整路径。它可能随编辑器的打包方式而不同。

对于 Notepad++，一个流行的代码编辑器来说，可能想要使用 32 位的版本， 因为 64 位的版本可能尚不支持所有的插件。如果在使用 32 位的 Windows 系统，或在 64 位系统上使用 64 位的编辑器，那么需要输入如下命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --global core.editor "'C:/ProgramFiles/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
~~~

### 4.4 检查配置信息

使用 git config --list 命令来列出所有 Git 当时能找到的配置。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
~~~

可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置（例如：/etc/gitconfig与~/.gitconfig）。这种情况下，Git 会使用它找到的每一个变量的最后一个配置。

可以通过输入 git config 'key' 来检查 Git 的某一项配置

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config user.name
John Doe
~~~

由于 Git 会从多个文件中读取同一配置变量的不同值，因此用户可能会在其中看到意料之外的值而不知道为什么。此时，可以查询 Git 中该变量的原始值，它会告诉用户哪一个配置文件最后设置了该值：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --show-origin rerere.autoUpdate
file:/home/johndoe/.gitconfig false
~~~

### 4.5 获取帮助

若使用 Git 时需要获取帮助，有三种等价的方法可以找到 Git 命令的综合手册（manpage）：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git help <verb>

MasterChief@DESKTOP MINGW64 ~
$ git <verb> --help

MasterChief@DESKTOP MINGW64 ~
$ man git-<verb>
~~~

例如，要想获得 git config 命令的手册，执行

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git help config
~~~

如果觉得手册或者本书的内容还不够用，可以尝试在 Freenode IRC 服务器 [https://freenode.net] 上的 #git 或 #github 频道寻求帮助。

如果不需要全面的手册，只需要可用选项的快速参考，那么可以用 -h 选项获得更简明的 “help” 输出：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git add -h
~~~

~~~bash
usage: git add [<options>] [--] <pathspec>...
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
~~~

## 5 Git基础

如果只想通过阅读一章来学习 Git，那么本章将是不二选择。本章涵盖了在使用 Git 完成各种工作时将会用到的各种基本命令。在学习完本章之后，应该能够配置并初始化一个仓库（repository）、开始或停止跟踪（track）文件、暂存（stage）或提交（commit）更改。本章也将演示如何配置 Git 来忽略指定的文件和文件模式、如何迅速而简单地撤销错误操作、如何浏览项目的历史版本以及不同提交（commits）之间的差异、如何向远程仓库推送（push）以及如何从远程仓库拉取（pull）文件。

### 5.1 获取Git仓库

通常有两种获取 Git 项目仓库的方式：

1.将尚未进行版本控制的本地目录转换为 Git 仓库；

2.从其它服务器克隆一个已存在的 Git 仓库。

两种方式都会在的本地机器上得到一个工作就绪的 Git 仓库。

#### 5.1.1 在已存在目录中初始化仓库

首先需要进入该项目目录中
在 Linux 上：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cd /home/user/my_project
~~~

在 macOS 上：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cd /Users/user/my_project
~~~

在 Windows 上：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cd /c/user/my_project
~~~

初始化Git仓库，执行：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git init
~~~

该命令将创建一个名为 .git 的子目录，这个子目录含有初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。但是，在这个时候，我们仅仅是做了一个初始化的操作，项目里的文件还没有被跟踪。

如果在一个已存在文件的文件夹（而非空文件夹）中进行版本控制，应该开始追踪这些文件并进行初始提交。可以通过 git add 命令来指定所需的文件来进行追踪，然后执行 git commit ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git add *.c

MasterChief@DESKTOP MINGW64 ~
$ git add LICENSE

MasterChief@DESKTOP MINGW64 ~
$ git commit -m 'initial project version'
~~~

### 5.2 克隆现有的仓库

如果用户想获得一份已经存在了的 Git 仓库的拷贝，比如说，用户想为某个开源项目贡献自己的一份力，这时就要用到 git clone 命令。 Git 克隆的是该 Git 仓库服务器上的几乎所有数据，而不是仅仅复制完成用户的工作所需要文件。 当用户执行 git clone 命令的时候，默认配置下远程 Git 仓库中的每一个文件的每一个版本都将被拉取下来。

克隆仓库的命令是 git clone < url > 。比如，要克隆 Git 的链接库 libgit2 ，可以用下面的命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone https://github.com/libgit2/libgit2
~~~

这会在当前目录下创建一个名为 “libgit2” 的目录，并在这个目录下初始化一个 .git 文件夹， 从远程仓库拉取下所有数据放入 .git 文件夹，然后从中读取最新版本的文件的拷贝。

如果用户想在克隆远程仓库的时候，自定义本地仓库的名字，用户可以通过额外的参数指定新的目录名：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone https://github.com/libgit2/libgit2 mylibgit
~~~

这会执行与上一条命令相同的操作，但目标目录名变为了 mylibgit 。

Git 支持多种数据传输协议。 上面的例子使用的是 https 协议 ，不过用户也可以使用 git 协议 或者使用 SSH 传输协议 ，比如 user@server:path/to/repo.git 。

工作目录下的每一个文件都不外乎这两种状态：已跟踪或未跟踪。已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能是未修改，已修改或已放入暂存区。简而言之，已跟踪的文件就是 Git 已经知道的文件。

工作目录中除已跟踪文件外的其它所有文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有被放入暂存区。初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态，因为 Git 刚刚检出了它们，而用户尚未编辑过它们。

### 5.3 检查当前文件状态

用 git status 命令查看哪些文件处于什么状态。如果在克隆仓库后立即使用此命令，会看到类似这样的输出：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
~~~

在项目下创建一个新的 README 文件

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ echo 'My Project' > README
~~~

如果之前并不存在这个文件，使用 git status 命令，将看到一个新的未跟踪文件：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:  
(use "git add <file>..." to include in what will be committed)
READMEnothing added to commit but untracked files present (use "git add" totrack)
~~~

### 5.4 跟踪新文件

使用命令 git add 开始跟踪一个文件。所以，要跟踪 README 文件，运行：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git add README
~~~

再运行 git status 命令，会看到 README 文件已被跟踪，并处于暂存状态：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:  
(use "git restore --staged <file>..." to unstage)
new file:   README
~~~

只要在 Changes to be committed 这行下面的，就说明是已暂存状态。 git add 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。

### 5.5 暂存已修改的文件

如果用户修改了一个名为 CONTRIBUTING.md 的已被跟踪的文件，然后运行 git status 命令，会看到下面内容：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:  
(use "git reset HEAD <file>..." to unstage)
new file:   README
Changes not staged for commit:  
(use "git add <file>..." to update what will be committed)  
(use "git checkout -- <file>..." to discard changes in workingdirectory)
modified:   CONTRIBUTING.md
~~~

文件 CONTRIBUTING.md 出现在 Changes not staged for commit 这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。要暂存这次更新，需要运行 git add 命令。

运行了 git add 之后又作了修订的文件，需要重新运行 git add 把最新版本重新暂存起来。

### 5.6 状态简览

git status 命令的输出十分详细，但其用语有些繁琐。 Git 有一个选项可以缩短状态命令的输出，以简洁的方式查看更改。使用 git status -s 命令或 git status --short 命令，将得到一种格式更为紧凑的输出。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
~~~

新添加的未跟踪文件前面有 ?? 标记，新添加到暂存区中的文件前面有 A 标记，修改过的文件前面有 M 标记。输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态。例如，上面的状态报告显示： README 文件在工作区已修改但尚未暂存，而 lib/simplegit.rb 文件已修改且已暂存。 Rakefile 文件已修，暂存后又作了修改，因此该文件的修改中既有已暂存的部分，又有未暂存的部分。

### 5.7 忽略文件

有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。这种情况下，可以创建一个名为 .gitignore 的文件，列出要忽略的文件的模式。来看一个实际的 .gitignore 例子：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cat .gitignore
*.[oa]
*~
~~~

第一行告诉 Git 忽略所有以 .o 或 .a 结尾的文件。

第二行告诉 Git 忽略所有名字以波浪符（~）结尾的文件，许多文本编辑软件（比如 Emacs）都用这样的文件名保存副本。

**.gitignore 文件的格式规范如下：**

•所有空行或者以 # 开头的行都会被 Git 忽略。

•可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。

•匹配模式可以以（/）开头防止递归。

•匹配模式可以以（/）结尾指定目录。

•要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反。

所谓的 glob 模式是指 bash 所使用的简化了的正则表达式。星号（\*）匹配零个或多个任意字符； \[abc\] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a ，要么匹配一个 b ，要么匹配一个 c ）； 问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配（比如 \[0-9\] 表示匹配所有 0 到 9 的数字）。 使用两个星号（\*\*） 表示匹配任意中间目录，比如 a/\*\*/z可以匹配 a/z 、 a/b/z 或 a/b/c/z 等。

我们再看一个 .gitignore 文件的例子：

~~~bash
# 忽略所有的 .a 文件
*.a
# 但跟踪所有的 lib.a，即便用户在前面忽略了 .a 文件
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

GitHub 有一个十分详细的针对数十种项目及语言的 .gitignore 文件列表，可以在 [https://github.com/github/gitignore] 找到它。
在最简单的情况下，一个仓库可能只根目录下有一个 .gitignore 文件，它递归地应用到整个仓库中。然而，子目录下也可以有额外的 .gitignore 文件。子目录中的 .gitignore 文件中的规则只作用于它所在的目录中。（ Linux 内核的源码库拥有 206 个 .gitignore 文件。）

### 5.8 查看已暂存和未暂存的修改

如果 git status 命令的输出对于用户来说过于简略，而用户想知道具体修改了什么地方，可以用 git diff 命令。

此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。也就是修改之后还没有暂存起来的变化内容。

若要查看已暂存的将要添加到下次提交里的内容，可以用 git diff --staged 命令。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git diff --staged
~~~

使用 git diff 来分析文件差异,也可以使用图形化的工具或外部 diff 工具来比较差异。可以使用 git difftool 命令来调用 emerge 或 vimdiff 等软件（包括商业软件）输出 diff 的分析结果。使用 git difftool --tool-help 命令来看系统支持哪些 Git Diff 插件。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git difftool --tool-help
~~~

### 5.9 提交更新

在提交之前，务必确认还有什么已修改或新建的文件还没有 git add 过， 否则提交的时候不会记录这些尚未暂存的变化。已修改但未暂存的文件只会保留在本地磁盘。所以，每次准备提交前，先用 git status 看下，所需要的文件是不是都已暂存起来了，然后再运行提交命令 git commit ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git commit
~~~

更详细的内容修改提示可以用 -v 选项查看，这会将用户所作更改的 diff 输出呈现在编辑器中，以便让用户知道本次提交具体作出哪些修改。

另外，也可以在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行，如下所示：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git commit -m "Story 182: Fix benchmarks for speed"
~~~

### 5.10 跳过使用暂存区域

Git 提供了一个跳过使用暂存区域的方式，只要在提交的时候，给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:  
(use "git add <file>..." to update what will be committed)  
(use "git checkout -- <file>..." to discard changes in workingdirectory)
modified:   CONTRIBUTING.md
no changes added to commit (use "git add" and/or "git commit -a")

MasterChief@DESKTOP MINGW64 ~
$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
~~~

### 5.11 移除文件

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。可以用 git rm 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git rm PROJECTS.md
~~~

如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 -f （译注：即 force 的首字母）。这是一种安全特性，用于防止误删尚未添加到快照的数据，这样的数据不能被 Git 恢复。

另外一种情况是，把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。换句话说，用户想让文件保留在磁盘，但是并不想让 Git 继续跟踪。为达到这一目的，可以使用 --cached 选项：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git rm --cached README
~~~

git rm 命令后面可以列出文件或者目录的名字，也可以使用 glob 模式。比如：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git rm log/\*.log
~~~

注意到星号 * 之前的反斜杠 \ ，因为 Git 有它自己的文件模式扩展匹配方式，所以我们不用 bash 来帮忙展开。此命令删除 log/ 目录下扩展名为 .log 的所有文件。类似的比如：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git rm \*~
~~~

该命令会删除所有名字以 ~ 结尾的文件。

### 5.12 移动文件

要在 Git 中对文件改名，可以输入如下命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git mv file_from file_to
~~~

其实，运行 git mv 就相当于运行了下面三条命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ mv README.md README

MasterChief@DESKTOP MINGW64 ~
$ git rm README.md

MasterChief@DESKTOP MINGW64 ~
$ git add README
~~~

有时候用其他工具批处理重命名的话，要记得在提交前删除旧的文件名，再添加新的文件名。

### 5.13 查看提交历史

在提交了若干更新，又或者克隆了某个项目之后，用户也许想回顾下提交历史。完成这个任务最简单而又有效的工具是 git log 命令。

我们使用一个非常简单的 “simplegit” 项目作为示例。运行下面的命令获取该项目：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone https://github.com/schacon/simplegit-progit
~~~

当用户在此项目中运行 git log 命令时，可以看到下面的输出：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700
    removed unnecessary test
commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700
    first commit
~~~

不传入任何参数的默认情况下， git log 会按时间先后顺序列出所有的提交，最近的更新排在最上面。这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

git log 比较有用的选项是 -p 或 --patch ，它会显示每次提交所引入的差异（按补丁的格式输出）。用户也可以限制显示的日志条目数量，例如使用 -2 选项来只显示最近的两次提交：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.platform  =   Gem::Platform::RUBY
     s.name      =   "simplegit"
-    s.version   =   "0.1.0"
+    s.version   =   "0.1.1"
     s.author    =   "Scott Chacon"
     s.email     =   "schacon@gee-mail.com"
     s.summary   =   "A simple gem for using Git in Ruby code."
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700
    removed unnecessary test
diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
     end
 end
-
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
~~~

该选项除了显示基本信息之外，还附带了每次提交的变化。可以为 git log 附带一系列的总结性选项。比如用户想看到每次提交的简略统计信息，可以使用 --stat 选项：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log --stat
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
 Rakefile | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700
    removed unnecessary test
 lib/simplegit.rb | 5 -----
 1 file changed, 5 deletions(-)
commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700
    first commit
 README           |  6 ++++++
 Rakefile         | 23 +++++++++++++++++++++++
 lib/simplegit.rb | 25 +++++++++++++++++++++++++
 3 files changed, 54 insertions(+)
~~~

--stat 选项在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结。

另一个非常有用的选项是 --pretty 。 这个选项可以使用不同于默认格式的方式展示提交历史。这个选项有一些内建的子选项供用户使用。比如 oneline 会将每个提交放在一行显示，在浏览大量的提交时非常有用。另外还有 short ，full 和 fuller 选项，它们展示信息的格式基本一致，但是详尽程度不一。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
~~~

最有意思的是 format ，可以定制记录的显示格式。这样的输出对后期提取分析格外有用——因为用户知道输出的格式不会随着 Git 的更新而发生改变：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : changed the version number
085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
a11bef0 - Scott Chacon, 6 years ago : first commit
~~~

git log --pretty=format 常用的选项列出了 format 接受的常用格式占位符的写法及其代表的意义。

git log --pretty=format 常用的选项
| 选项 | 说明 |
| :-----| :----- |
| %H | 提交的完整哈希值 |
| %h | 提交的简写哈希值 |
| %T | 树的完整哈希值 |
| %t | 树的简写哈希值 |
| %P | 父提交的完整哈希值 |
| %p | 父提交的简写哈希值 |
| %an | 作者名字 |
| %ae | 作者的电子邮件地址 |
| %ad | 作者修订日期（可以用 --date= 选项来定制格式） |
| %ar | 作者修订日期，按多久以前的方式显示 |
| %cn | 提交者的名字 |
| %ce | 提交者的电子邮件地址 |
| %cd | 提交日期 |
| %cr | 提交日期（距今多长时间） |
| %s | 提交说明 |

作者指的是实际作出修改的人，提交者指的是最后将此工作成果提交到仓库的人。

当 oneline 或 format 与另一个 log 选项 --graph 结合使用时尤其有用。这个选项添加了一些 ASCII 字符串来形象地展示用户的分支、合并历史：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 ignore errors from SIGCHLD on trap
*  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
|\
| * 420eac9 Added a method for getting the current branch.
* | 30e367c timeout code and tests
* | 5a09431 add timeout protection to grit
* | e1193f8 support for heads with slashes in them
|/
* d6016bc require time for xmlschema
*  11d191e Merge branch 'defunkt' into local
~~~

以上只是简单介绍了一些 git log 命令支持的选项。 git log 的常用选项列出了我们目前涉及到的和没涉及到的选项，以及它们是如何影响 log 命令的输出的：

git log 的常用选项
| 选项 | 说明 |
| :-----| :----- |
| -p | 按补丁格式显示每个提交引入的差异。 |
| --stat | 显示每次提交的文件修改统计信息。 |
| --shortstat | 只显示 --stat 中最后的行数修改添加移除统计。 |
| --name-only | 仅在提交信息后显示已修改的文件清单。 |
| --name-status | 显示新增、修改、删除的文件清单。 |
| --abbrev-commit | 仅显示 SHA-1 校验和所有 40 个字符中的前几个字符。 |
| --relative-date | 使用较短的相对时间而不是完整格式显示日期（比如“2 weeks ago”）。 |
| --graph | 在日志旁以 ASCII 图形显示分支与合并历史。 |
| --pretty | 使用其他格式显示历史提交信息。可用的选项包括 oneline、short、full、fuller 和format（用来定义自己的格式）。 |
| --oneline | --pretty=oneline --abbrev-commit合用的简写。 |

### 5.14 限制输出长度

除了定制输出格式的选项之外，git log 还有许多非常实用的限制输出长度的选项，也就是只输出一部分的提交。可以使用类似 -< n > 的选项，其中的 n 可以是任何整数，表示仅显示最近的n条提交。不过实践中这个选项不是很常用，因为 Git 默认会将所有的输出传送到分页程序中，所以用户一次只会看到一页的内容。

类似 --since 和 --until 这种按照时间作限制的选项很有用。 例如，下面的命令会列出最近两周的所有提交：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log --since=2.weeks
~~~

该命令可用的格式十分丰富——可以是类似 "2008-01-15" 的具体的某一天，也可以是类似 "2 years 1 day 3 minutes ago" 的相对日期。

还可以过滤出匹配指定条件的提交。用 --author 选项显示指定作者的提交，用 --grep 选项搜索提交说明中的关键字。

可以指定多个 --author 和 --grep 搜索条件，这样会只输出任意匹配 --author 模式和 --grep 模式的提交。然而，如果用户添加了 --all-match 选项，则只会输出所有匹配 --grep 模式的提交。

另一个非常有用的过滤器是 -S （俗称“pickaxe”选项），它接受一个字符串参数，并且只会显示那些添加或删除了该字符串的提交。假设用户想找出添加或删除了对某一个特定函数的引用的提交，可以调用：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log -S function_name
~~~

最后一个很实用的 git log 选项是路径（path），如果只关心某些文件或者目录的历史提交，可以在 git log 选项的最后指定它们的路径。因为是放在最后位置上的选项，所以用两个短划线（--）隔开之前的选项和后面限定的路径名。

在 限制 git log 输出的选项 中列出了常用的选项

限制 git log 输出的选项
| 选项 | 说明 |
| :-----| :----- |
| -< n > | 仅显示最近的 n 条提交。 |
| --since, --after | 仅显示指定时间之后的提交。 |
| --until, --before | 仅显示指定时间之前的提交。 |
| --author | 仅显示作者匹配指定字符串的提交。 |
| --committer | 仅显示提交者匹配指定字符串的提交。 |
| --grep | 仅显示提交说明中包含指定字符串的提交。 |
| -S | 仅显示添加或删除内容匹配指定字符串的提交。 |

如果要在 Git 源码库中查看 Junio Hamano 在 2008 年 10 月其间， 除了合并提交之外的哪一个提交修改了测试文件，可以使用下面的命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" \   --before="2008-11-01" --no-merges -- t/
5610e3b - Fix testcase failure when extended attributes are in use
acd3b9e - Enhance hold_lock_file_for_{update,append}() API
f563754 - demonstrate breakage of detached checkout with symbolic link
HEAD
d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
51a94af - Fix "checkout --track -b newbranch" on detached HEAD
b0ad11e - pull: allow "git pull origin $something:$current_branch" into an
unborn branch
~~~

*隐藏合并提交*
*按照用户代码仓库的工作流程，记录中可能有为数不少的合并提交，它们所包含的信息通常并不多。为了避免显示的合并提交弄乱历史记录，可以为 log 加上 --no-merges 选项。*

### 5.15 撤消操作

注意，有些撤消操作是不可逆的。这是在使用 Git 的过程中，会因为操作失误而导致之前的工作丢失的少有的几个地方之一。

运行带有 --amend 选项的提交命令来重新提交：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git commit --amend
~~~

这个命令会将暂存区中的文件提交。如果自上次提交以来用户还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，而用户所修改的只是提交信息。

例如，用户提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git commit -m 'initial commit'

MasterChief@DESKTOP MINGW64 ~
$ git add forgotten_file

MasterChief@DESKTOP MINGW64 ~
$ git commit --amend
~~~

最终用户只会有一个提交——第二次提交将代替第一次提交的结果。

### 5.16 取消暂存的文件

如何操作暂存区和工作目录中已修改的文件。

这些命令在修改文件状态的同时，也会提示如何撤消操作。例如，已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却意外地输入 git add * 暂存了它们两个。如何只取消暂存两个中的一个呢？ git status 命令提示：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git add *

MasterChief@DESKTOP MINGW64 ~
$ git status
On branch masterChanges to be committed:
  (use "git reset HEAD <file>..." to unstage)
    renamed:    README.md -> README
    modified:   CONTRIBUTING.md
~~~

在 “Changes to be committed” 文字正下方，提示使用 git reset HEAD < file >... 来取消暂存。所以，我们可以这样来取消暂存 CONTRIBUTING.md 文件：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M   CONTRIBUTING.md

MasterChief@DESKTOP MINGW64 ~
$ git status
On branch masterChanges to be committed:
  (use "git reset HEAD <file>..." to unstage)
    renamed:    README.md -> README
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in workingdirectory)
    modified:   CONTRIBUTING.md
~~~

### 5.17 撤消对文件的修改

如果用户并不想保留对 CONTRIBUTING.md 文件的修改，通过如下命令方便地撤消修改——将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚把它放入工作目录时的样子）

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout -- CONTRIBUTING.md
~~~

请务必记得 git checkout -- < file > 是一个危险的命令。用户对那个文件在本地的任何修改都会消失—— Git 会用最近提交的版本覆盖掉它。

## 6 远程仓库的使用

远程仓库是指托管在因特网或其他网络中的用户项目的版本库。用户可以有几个远程仓库，通常有些仓库对只读，有些则可以读写。与他人协作涉及管理远程仓库以及根据需要推送或拉取数据。
管理远程仓库包括了解如何添加远程仓库、移除无效的远程仓库、管理不同的远程分支并定义它们是否被跟踪等等。

*远程仓库可以在用户的本地主机上*
*用户完全可以在一个“远程”仓库上工作，而实际上它在用户本地的主机上。词语“远程”未必表示仓库在网络或互联网上的其它位置，而只是表示它在别处。在这样的远程仓库上工作，仍然需要和其它远程仓库上一样的标准推送、拉取和抓取操作。*

### 6.1 Linux查看远程仓库

如果用户想查看已经配置的远程仓库服务器，可以运行 git remote 命令。它会列出用户指定的每一个远程服务器的简写。如果用户已经克隆了自己的仓库，那么至少应该能看到 origin ——这是 Git 给用户克隆的仓库服务器的默认名字：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.

MasterChief@DESKTOP MINGW64 ~
$ cd ticgit

MasterChief@DESKTOP MINGW64 ~
$ git remote
origin
~~~

用户也可以指定选项 -v ，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL 。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
~~~

如果用户的远程仓库不止一个，该命令会将它们全部列出。例如，与几个协作者合作的，拥有多个远程仓库的仓库看起来像下面这样：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cd grit

MasterChief@DESKTOP MINGW64 ~
$ git remote -v
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

### 6.2 添加远程仓库

运行 git remote add < shortname > < url > 添加一个新的远程 Git 仓库，同时指定一个方便使用的简写：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git remote
origin

MasterChief@DESKTOP MINGW64 ~
$ git remote add pb https://github.com/paulboone/ticgit

MasterChief@DESKTOP MINGW64 ~
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
pb  https://github.com/paulboone/ticgit (fetch)
pb  https://github.com/paulboone/ticgit (push)
~~~

现在用户可以在命令行中使用字符串 pb 来代替整个 URL 。例如，如果用户想拉取 Paul 的仓库中有但用户没有的信息，可以运行 git fetch pb ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git fetch pb
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 43 (delta 10), reused 31 (delta 5)
Unpacking objects: 100% (43/43), done.
From https://github.com/paulboone/ticgit
 * [new branch]      master     -> pb/master
 * [new branch]      ticgit     -> pb/ticgit
~~~

现在 Paul 的 master 分支可以在本地通过 pb/master 访问到——用户可以将它合并到自己的某个分支中，或者如果用户想要查看它的话，可以检出一个指向该点的本地分支。

### 6.3 从远程仓库中抓取与拉取

从远程仓库中获得数据，可以执行：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git fetch <remote>
~~~

这个命令会访问远程仓库，从中拉取所有用户还没有的数据。执行完成后，用户将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

如果用户使用 clone 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。所以， git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作。必须注意 git fetch 命令只会将数据下载到用户的本地仓库——它并不会自动合并或修改用户当前的工作。当准备好时用户必须手动将其合并入用户的工作。

如果用户的当前分支设置了跟踪远程分支（阅读下一节和 Git 分支了解更多信息），那么可以用 git pull 命令来自动抓取后合并该远程分支到当前分支。

### 6.4 推送到远程仓库

当用户想分享项目时，必须将其推送到上游。这个命令很简单： git push < remote > < branch > 。

当用户想要将 master 分支推送到 origin 服务器时（再次说明，克隆时通常会自动帮用户设置好那两个名字），那么运行这个命令就可以将用户所做的备份到服务器：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git push origin master
~~~

只有当用户有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。当用户和其他人在同一时间克隆，他们先推送到上游然后用户再推送到上游，用户的推送就会毫无疑问地被拒绝。用户必须先抓取他们的工作并将其合并进用户的工作后才能推送。

### 6.5 查看某个远程仓库

如果想要查看某一个远程仓库的更多信息，可以使用 git remote show < remote > 命令。如果想以一个特定的缩写名运行这个命令，例如 origin ，会得到像下面类似的信息：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git remote show origin
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

它同样会列出远程仓库的 URL 与跟踪分支的信息。这些信息非常有用，它告诉用户正处于 master 分支，并且如果运行 git pull ，就会抓取所有的远程引用，然后将远程 master 分支合并到本地 master 分支。它也会列出拉取到的所有远程引用。

如果用户是 Git 的重度使用者，那么还可以通过 git remote show 看到更多的信息。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git remote show origin
* remote origin
  URL: https://github.com/my-org/complex-project
  Fetch URL: https://github.com/my-org/complex-project
  Push  URL: https://github.com/my-org/complex-project
  HEAD branch: master
  Remote branches:
    master                           tracked
    dev-branch                       tracked
    markdown-strip                   tracked
    issue-43                         new (next fetch will store in
remotes/origin)
    issue-45                         new (next fetch will store in
remotes/origin)
    refs/remotes/origin/issue-11     stale (use 'git remote prune' to
remove)
  Local branches configured for 'git pull':
    dev-branch merges with remote dev-branch
    master     merges with remote master
  Local refs configured for 'git push':
    dev-branch                     pushes to dev-branch
(up to date)
    markdown-strip                 pushes to markdown-strip
(up to date)
    master                         pushes to master
(up to date)
~~~

这个命令列出了当用户在特定的分支上执行 git push 会自动地推送到哪一个远程分支。它也同样地列出了哪些远程分支不在用户的本地，哪些远程分支已经从服务器上移除了，还有当用户执行 git pull 时哪些本地分支可以与它跟踪的远程分支自动合并。

### 6.6 远程仓库的重命名与移除

用户可以运行 git remote rename 来修改一个远程仓库的简写名。例如，想要将 pb 重命名为 paul ，可以用 git remote rename 这样做：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git remote rename pb paul

MasterChief@DESKTOP MINGW64 ~
$ git remote
origin
paul
~~~

值得注意的是这同样也会修改用户所有远程跟踪的分支名字。那些过去引用 pb/master 的现在会引用paul/master 。

如果因为一些原因想要移除一个远程仓库——用户已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了——可以使用 git remote remove 或 git remote rm ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git remote remove paul

MasterChief@DESKTOP MINGW64 ~
$ git remote
origin
~~~

一旦用户使用这种方式删除了一个远程仓库，那么所有和这个远程仓库相关的远程跟踪分支以及配置信息也会一起被删除。

### 6.7 打标签

Git 可以给仓库历史中的某一个提交打上标签，以示重要。比较有代表性的是人们会使用这个功能来标记发布结点（ v1.0、 v2.0等等）。

#### 6.7.1 列出标签

在 Git 中列出已有的标签非常简单，只需要输入 git tag （可带上可选的 -l 选项 --list ）：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git tag
v1.0
v2.0
~~~

这个命令以字母顺序列出标签，但是它们显示的顺序并不重要。

用户也可以按照特定的模式查找标签。例如，Git 自身的源代码仓库包含标签的数量超过500个。如果只对 1.8.5 系列感兴趣，可以运行：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git tag -l "v1.8.5*"
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
v1.8.5-rc3
v1.8.5.1
v1.8.5.2
v1.8.5.3
v1.8.5.4
v1.8.5.5
~~~

*按照通配符列出标签需要 -l 或 --list 选项*
*如果用户只想要完整的标签列表，那么运行 git tag 就会默认假定用户想要一个列表，它会直接给用户列出来，此时的 -l 或 --list 是可选的。 然而，如果用户提供了一个匹配标签名的通配模式，那么 -l 或 --list 就是强制使用的。*

#### 6.7.2 创建标签

Git 支持两种标签：轻量标签（ lightweight ）与附注标签（ annotated ）。

轻量标签很像一个不会改变的分支——它只是某个特定提交的引用。

附注标签是存储在 Git 数据库中的一个完整对象，它们是可以被校验的，其中包含打标签者的名字、电子邮件地址、日期时间，此外还有一个标签信息，并且可以使用 GNU Privacy Guard （GPG）签名并验证。通常会建议创建附注标签，这样用户可以拥有以上所有信息。

#### 6.7.3 附注标签

在 Git 中创建附注标签十分简单。最简单的方式是当用户在运行 tag 命令时指定 -a 选项：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git tag -a v1.4 -m "my version 1.4"

MasterChief@DESKTOP MINGW64 ~
$ git tag
v0.1
v1.3
v1.4
~~~

-m 选项指定了一条将会存储在标签中的信息。如果没有为附注标签指定一条信息，Git 会启动编辑器要求用户输入信息。通过使用 git show 命令可以看到标签信息和与之对应的提交信息：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git show v1.4
tag v1.4
Tagger: Ben Straub <ben@straub.cc>
Date:   Sat May 3 20:19:12 2014 -0700
my version 1.4
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
~~~

输出显示了打标签者的信息、打标签的日期时间、附注信息，然后显示具体的提交信息。

#### 6.7.4 轻量标签

轻量标签本质上是将提交校验和存储到一个文件中——没有保存任何其他信息。创建轻量标签，不需要使用 -a 、-s 或 -m 选项，只需要提供标签名字：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git tag v1.4-lw

MasterChief@DESKTOP MINGW64 ~
$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
~~~

这时，如果在标签上运行 git show ，用户不会看到额外的标签信息。命令只会显示出提交信息：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git show v1.4-lw
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
~~~

#### 6.7.5 后期打标签

用户也可以对过去的提交打标签。 假设提交历史是这样的：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log --pretty=oneline
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

假设在 v1.2 时用户忘记给项目打标签，也就是在 “updated rakefile” 提交。用户可以在之后补上标签。要在那个提交上打标签，用户需要在命令的末尾指定提交的校验和（或部分校验和）：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git tag -a v1.2 9fceb02
~~~

可以看到用户已经在那次提交上打上标签了：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git tag
v0.1
v1.2
v1.3
v1.4
v1.4-lw
v1.5

MasterChief@DESKTOP MINGW64 ~
$ git show v1.2
tag v1.2
Tagger: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Feb 9 15:32:16 2009 -0800
version 1.2
commit 9fceb02d0ae598e95dc970b74767f19372d61af8
Author: Magnus Chacon <mchacon@gee-mail.com>
Date:   Sun Apr 27 20:43:35 2008 -0700
    updated rakefile...
~~~

#### 6.7.6 共享标签

默认情况下， git push 命令并不会传送标签到远程仓库服务器上。在创建完标签后用户必须显式地推送标签到共享服务器上。这个过程就像共享远程分支一样——用户可以运行 git push origin < tagname >。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git push origin v1.5
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.5 -> v1.5
~~~

如果想要一次性推送很多标签，也可以使用带有 --tags 选项的 git push 命令。这将会把所有不在远程仓库服务器上的标签全部传送到那里。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)To git@github.com:schacon/simplegit.git
 * [new tag]         v1.4 -> v1.4
 * [new tag]         v1.4-lw -> v1.4-lw
~~~

*git push 推送两种标签*
*使用 git push < remote > --tags 推送标签并不会区分轻量标签和附注标签，没有简单的选项能够让用户只选择推送一种标签。*

#### 6.7.7 删除标签

要删除掉用户本地仓库上的标签，可以使用命令 git tag -d < tagname > 。
例如，可以使用以下命令删除一个轻量标签：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git tag -d v1.4-lw
Deleted tag 'v1.4-lw' (was e7d5add)
~~~

注意上述命令并不会从任何远程仓库中移除这个标签，用户必须用 git push < remote >:refs/tags/< tagname > 来更新用户的远程仓库：

第一种变体是 git push < remote >:refs/tags/< tagname > ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git push origin :refs/tags/v1.4-lw
To /git@github.com:schacon/simplegit.git
 - [deleted]         v1.4-lw
~~~

第二种更直观的删除远程标签的方式是：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git push origin --delete <tagname>
~~~

#### 6.7.8 检出标签

如果用户想查看某个标签所指向的文件版本，可以使用 git checkout 命令， 虽然这会使用户的仓库处于“分离头指针（detached HEAD）”的状态——这个状态有些不好的副作用。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout 2.0.0
Note: checking out '2.0.0'.
You are in 'detached HEAD' state. You can look around, make experimentalchanges and commit them, and you can discard any commits you make in thisstate without impacting any branches by performing another checkout.
If you want to create a new branch to retain commits you create, you maydo so (now or later) by using -b with the checkout command again. Example:
  git checkout -b <new-branch>
HEAD is now at 99ada87... Merge pull request #89 from schacon/appendix-final
$ git checkout 2.0-beta-0.1
Previous HEAD position was 99ada87... Merge pull request #89 fromschacon/appendix-final
HEAD is now at df3f601... add atlas.json and cover image
~~~

在“分离头指针”状态下，如果用户做了某些更改然后提交它们，标签不会发生变化，但用户的新提交将不属于任何分支，并且将无法访问，除非通过确切的提交哈希才能访问。 因此，如果用户需要进行更改，比如用户要修复旧版本中的错误，那么通常需要创建一个新分支：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
~~~

如果在这之后又进行了一次提交，version2 分支就会因为这个改动向前移动，此时它就会和v2.0.0标签稍微有些不同，这时就要当心了。

### 6.8 Git 别名

如果不想每次都输入完整的 Git 命令，可以通过 gitconfig 文件来轻松地为每一个命令设置一个别名。这里有一些例子用户可以试试：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --global alias.co checkout

MasterChief@DESKTOP MINGW64 ~
$ git config --global alias.br branch

MasterChief@DESKTOP MINGW64 ~
$ git config --global alias.ci commit

MasterChief@DESKTOP MINGW64 ~
$ git config --global alias.st status
~~~

这意味着，当要输入 git commit 时，只需要输入 git ci 。
为了解决取消暂存文件的易用性问题，可以向 Git 中添加用户自己的取消暂存别名：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --global alias.unstage 'reset HEAD --'
~~~

这会使下面的两个命令等价：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git unstage fileA

MasterChief@DESKTOP MINGW64 ~
$ git reset HEAD -- fileA
~~~

这样看起来更清楚一些。 通常也会添加一个 last 命令，像这样：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --global alias.last 'log -1 HEAD'
~~~

这样，可以轻松地看到最后一次提交：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git last
commit 66938dae3329c7aebe598c2246a8e6af90d04646
Author: Josh Goebel <dreamer3@example.com>
Date:   Tue Aug 26 19:48:51 2008 +0800
    test for current head
    Signed-off-by: Scott Chacon <schacon@example.com>
~~~

可以看出，Git 只是简单地将别名替换为对应的命令。 然而，用户可能想要执行外部命令，而不是一个 Git 子命令。 如果是那样的话，可以在命令前面加入 ! 符号。 如果用户自己要写一些与 Git 仓库协作的工具的话，那会很有用。 我们现在演示将 git visual 定义为 gitk 的别名：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git config --global alias.visual '!gitk'
~~~

## 7 Git分支

### 7.1 分支简介

在进行提交操作时， Git 会保存一个提交对象（ commit object ）。该提交对象会包含一个指向暂存内容快照的指针。但不仅仅是这样，该提交对象还包含了作者的姓名和邮箱、提交时输入的信息以及指向它的父对象的指针。首次提交产生的提交对象没有父对象，普通提交操作产生的提交对象有一个父对象，而由多个分支合并产生的提交对象有多个父对象。

当使用 git commit 进行提交操作时， Git 会先计算每一个子目录（本例中只有项目根目录）的校验和，然后在 Git 仓库中这些校验和保存为树对象。随后， Git 便会创建一个提交对象，它除了包含上面提到的那些信息外，还包含指向这个树对象（项目根目录）的指针。如此一来， Git 就可以在需要的时候重现此次保存的快照。

现在， Git 仓库中有五个对象：三个 blob 对象（保存着文件快照）、一个树对象（记录着目录结构和 blob 对象索引）以及一个提交对象（包含着指向前述树对象的指针和所有提交信息）。

Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 master 。在多次提交操作之后，用户其实已经有一个指向最后那个提交对象的 master 分支。 master 分支会在每次提交时自动向前移动。

Git 的 master 分支并不是一个特殊分支。它就跟其它分支完全没有区别。之所以几乎每一个仓库都有 master 分支，是因为 git init 命令默认创建它，并且大多数人都懒得去改动它。

### 7.2 分支创建

比如，创建一个 testing 分支， 用户需要使用 git branch 命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch testing
~~~

Git 有一个名为 HEAD 的特殊指针，指示当前在哪一个分支上。 git branch 命令仅仅创建一个新分支，并不会自动切换到新分支中去。

可以简单地使用 git log 命令查看各个分支当前所指的对象。提供这一功能的参数是 --decorate 。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log --oneline --decorate
f30ab (HEAD -> master, testing) add feature #32 - ability to add newformats to the central interface
34ac2 Fixed bug #1328 - stack overflow under certain conditions
98ca9 The initial commit of my project
~~~

### 7.3 分支切换

要切换到一个已存在的分支，用户需要使用 git checkout 命令。我们现在切换到新创建的 testing 分支去：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout testing
~~~

这样 HEAD就指向 testing 分支了。

切换回 master 分支

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout master
~~~

在切换分支时，一定要注意用户工作目录里的文件会被改变。如果是切换到一个较旧的分支，用户的工作目录会恢复到该分支最后一次提交时的样子。如果 Git 不能干净利落地完成这个任务，它将禁止切换分支。

可以简单地使用 git log 命令查看分叉历史。运行 git log --oneline --decorate --graph--all ，会输出用户的提交历史、各个分支的指向以及项目的分支分叉情况。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the
* 34ac2 fixed bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
~~~

### 7.4 分支的新建与合并

#### 7.4.1 新建分支

假设用户正在项目上工作，并且在 master 分支上已经有了一些提交。

现在，用户已经决定要解决公司使用的问题追踪系统中的 #53 问题。想要新建一个分支并同时切换到那个分支上，用户可以运行一个带有 -b 参数的 git checkout 命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout -b iss53
Switched to a new branch "iss53"
~~~

它是下面两条命令的简写：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch iss53

MasterChief@DESKTOP MINGW64 ~
$ git checkout iss53
~~~

用户继续在 #53 问题上工作，并且做了一些提交。在此过程中， iss53 分支在不断的向前推进，因为用户已经检出到该分支（也就是说，用户的 HEAD 指针指向了 iss53 分支）

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ vim index.html

MasterChief@DESKTOP MINGW64 ~
$ git commit -a -m 'added a new footer [issue 53]'
~~~

有了 Git 的帮助，用户不必把这个紧急问题和 iss53 的修改混在一起，用户也不需要花大力气来还原关于 53# 问题的修改，然后再添加关于这个紧急问题的修改，最后将这个修改提交到线上分支。用户所要做的仅仅是切换回 master 分支。

但是，在用户这么做之前，要留意用户的工作目录和暂存区里那些还没有被提交的修改，它可能会和用户即将检出的分支产生冲突从而阻止 Git 切换到该分支。最好的方法是，在用户切换分支之前，保持好一个干净的状态。有一些方法可以绕过这个问题（即，暂存（ stashing ） 和 修补提交（ commit amending ）），我们会在 贮藏与清理中 看到关于这两个命令的介绍。 现在，我们假设用户已经把用户的修改全部提交了，这时用户可以切换回 master 分支了：

~~~bash
$ git checkout master
Switched to branch 'master'
~~~

这个时候，用户的工作目录和用户在开始 #53 问题之前一模一样。

请牢记：当用户切换分支的时候， Git 会重置用户的工作目录，使其看起来像回到了用户在那个分支上最后一次提交的样子。

Git 会自动添加、删除、修改文件以确保此时用户的工作目录和这个分支最后一次提交时的样子一模一样。

接下来，用户要修复这个紧急问题。我们来建立一个 hotfix 分支，在该分支上工作直到问题解决：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout -b hotfix
Switched to a new branch 'hotfix'

MasterChief@DESKTOP MINGW64 ~
$ vim index.html

MasterChief@DESKTOP MINGW64 ~
$ git commit -a -m 'fixed the broken email address'
[hotfix 1fb7853] fixed the broken email address
 1 file changed, 2 insertions(+)
~~~

用户可以运行用户的测试，确保用户的修改是正确的，然后将 hotfix 分支合并回用户的 master 分支来部署到线上。用户可以使用 git merge 命令来达到上述目的：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout master

MasterChief@DESKTOP MINGW64 ~
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
~~~

在合并的时候，用户应该注意到了“快进（ fast-forward ）”这个词。由于用户想要合并的分支 hotfix 所指向的提交 C4 是用户所在的提交 C2 的直接后继，因此 Git 会直接将指针向前移动。换句话说，当用户试图合并两个分支时，如果顺着一个分支走下去能够到达另一个分支，那么 Git 在合并两者的时候，只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧——这就叫做 “快进（ fast-forward ）”。

关于这个紧急问题的解决方案发布之后，用户准备回到被打断之前时的工作中。然而，用户应该先删除 hotfix 分支，因为用户已经不再需要它了—— master 分支已经指向了同一个位置。用户可以使用带 -d 选项的 git branch 命令来删除分支：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
~~~

现在用户可以切换回用户正在工作的分支继续用户的工作，也就是针对 #53 问题的那个分支（ iss53 分支 ）。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout iss53
Switched to branch "iss53"

MasterChief@DESKTOP MINGW64 ~
$ vim index.html

MasterChief@DESKTOP MINGW64 ~
$ git commit -a -m 'finished the new footer [issue 53]'
[iss53 ad82d7a] finished the new footer [issue 53]
1 file changed, 1 insertion(+)
~~~

用户在 hotfix 分支上所做的工作并没有包含到 iss53 分支中。如果用户需要拉取 hotfix 所做的修改，用户可以使用 git merge master 命令将 master 分支合并入 iss53 分支，或者用户也可以等到 iss53 分支完成其使命，再将其合并回 master 分支。

#### 7.4.2 分支的合并

假设用户已经修正了 #53 问题，并且打算将用户的工作合并入 master 分支。为此，用户需要合并 iss53 分支到 master 分支，这和之前用户合并 hotfix 分支所做的工作差不多。用户只需要检出到用户想合并入的分支，然后运行 git merge 命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout master
Switched to branch 'master'

MasterChief@DESKTOP MINGW64 ~
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
~~~

修改合并之后，就不再需要 iss53 分支了。现在用户可以在任务追踪系统中关闭此项任务，并删除这个分支。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch -d iss53
~~~

#### 7.4.3 遇到冲突时的分支合并

如果用户在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改， Git 就没法干净的合并它们。如果用户对 #53 问题的修改和有关 hotfix 分支的修改都涉及到同一个文件的同一处，在合并它们的时候就会产生合并冲突：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
~~~

此时 Git 做了合并，但是没有自动地创建一个新的合并提交。 Git 会暂停下来，等待用户去解决合并产生的冲突。用户可以在合并冲突后的任意时刻使用 git status 命令来查看那些因包含合并冲突而处于未合并（ unmerged ）状态的文件：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
Unmerged paths:
  (use "git add <file>..." to mark resolution)
    both modified:      index.html
no changes added to commit (use "git add" and/or "git commit -a")
~~~

任何因包含合并冲突而有待解决的文件，都会以未合并状态标识出来。 Git 会在有冲突的文件中加入标准的冲突解决标记，这样用户可以打开这些包含冲突的文件然后手动解决冲突。出现冲突的文件会包含一些特殊区段，看起来像下面这个样子：

~~~bash
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
~~~

这表示 HEAD 所指示的版本（也就是用户的 master 分支所在的位置，因为用户在运行 merge 命令的时候已经检出到了这个分支）在这个区段的上半部分（=======的上半部分），而 iss53 分支所指示的版本在 ======= 的下半部分。

为了解决冲突，用户必须选择使用由 ======= 分割的两部分中的一个，或者用户也可以自行合并这些内容。 例如，用户可以通过把这段内容换成下面的样子来解决冲突：

~~~bash
<div id="footer">
please contact us at email.support@github.com
</div>
~~~

上述的冲突解决方案仅保留了其中一个分支的修改，并且 <<<<<<< , ======= , 和 >>>>>>> 这些行被完全删除了。在用户解决了所有文件里的冲突之后，对每个文件使用 git add 命令来将其标记为冲突已解决。一旦暂存这些原本有冲突的文件， Git 就会将它们标记为冲突已解决。

如果用户想使用图形化工具来解决冲突，用户可以运行 git mergetool ，该命令会为用户启动一个合适的可视化合并工具，并带领用户一步一步解决这些冲突：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git mergetool
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

如果用户想使用除默认工具（在这里 Git 使用 opendiff 做为默认的合并工具，因为作者在 Mac 上运行该程序）外的其他合并工具，用户可以在 “下列工具中（one of the following tools）” 这句后面看到所有支持的合并工具。然后输入用户喜欢的工具名字就可以了。

等用户退出合并工具之后， Git 会询问刚才的合并是否成功。如果用户回答是， Git 会暂存那些文件以表明冲突已解决：用户可以再次运行 git status 来确认所有的合并冲突都已被解决：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
Changes to be committed:
    modified:   index.html79
~~~

如果用户对结果感到满意，并且确定之前有冲突的的文件都已经暂存了，这时用户可以输入 git commit 来完成合并提交。默认情况下提交信息看起来像下面这个样子：

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

### 7.5 分支管理

git branch 命令不只是可以创建与删除分支。如果不加任何参数运行它，会得到当前所有分支的一个列表：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch
  iss53
* master
  testing
~~~

注意 master 分支前的 * 字符：它代表现在检出的那一个分支（也就是说，当前 HEAD 指针所指向的分支）。这意味着如果在这时候提交， master 分支将会随着新的工作向前移动。如果需要查看每一个分支的最后一次提交，可以运行 git branch -v 命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
~~~

--merged 与 --no-merged 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。如果要查看哪些分支已经合并到当前分支，可以运行 git branch --merged ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch --merged
  iss53
* master
~~~

因为之前已经合并了 iss53 分支，所以现在看到它在列表中。在这个列表中分支名字前没有 * 号的分支通常可以使用 git branch -d 删除掉；用户已经将它们的工作整合到了另一个分支，所以并不会失去任何东西。

查看所有包含未合并工作的分支，可以运行 git branch --no-merged ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch --no-merged
  testing
~~~

这里显示了其他分支。因为它包含了还未合并的工作，尝试使用 git branch -d 命令删除它时会失败：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.
~~~

如果真的想要删除分支并丢掉那些工作，如同帮助信息里所指出的，可以使用 -D 选项强制删除它。

选项 --merged 和 --no-merged 会在没有给定提交或分支名作为参数时，分别列出已合并或未合并到当前分支的分支。可以通过附加的参数来查看其它分支的合并状态而不必检出它们。例如，尚未合并到 master 分支的有哪些：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout testing

MasterChief@DESKTOP MINGW64 ~
$ git branch --no-merged master
  topicA
  featureB81
~~~

### 7.6 分支开发工作流

#### 7.6.1 长期分支

在整个项目开发周期的不同阶段，可以同时拥有多个开放的分支；可以定期地把某些主题分支合并入其他分支中。

许多使用 Git 的开发者都喜欢使用这种方式来工作，比如只在 master 分支上保留完全稳定的代码——有可能仅仅是已经发布或即将发布的代码。他们还有一些名为 develop 或者 next 的平行分支，被用来做后续开发或者测试稳定性——这些分支不必保持绝对稳定，但是一旦达到稳定状态，它们就可以被合并入 master 分支了。这样，在确保这些已完成的主题分支（短期分支，比如之前的 iss53 分支）能够通过所有测试，并且不会引入更多 bug 之后，就可以合并入主干分支中，等待下一次的发布。

稳定分支的指针总是在提交历史中落后一大截，而前沿分支的指针往往比较靠前。

使用多个长期分支的方法并非必要，但是这么做通常很有帮助，尤其是在一个非常庞大或者复杂的项目中工作时。

#### 7.6.2 主题分支

主题分支是一种短期分支，它被用来实现单一特性或其相关工作。

请牢记，当用户做这么多操作的时候，这些分支全部都存于本地。当用户新建和合并分支的时候，所有这一切都只发生在用户本地的 Git 版本库中——没有与服务器发生交互。

### 7.7 远程分支

远程引用是对远程仓库的引用（指针），包括分支、标签等等。用户可以通过 git ls-remote < remote > 来显式地获得远程引用的完整列表， 或者通过 git remote show < remote > 获得远程分支的更多信息。然而，一个更常见的做法是利用远程跟踪分支。

远程跟踪分支是远程分支状态的引用。它们是用户无法移动的本地引用。一旦用户进行了网络通信， Git 就会为用户移动它们以精确反映远程仓库的状态。请将它们看做书签，这样可以提醒用户该分支在远程仓库中的位置就是用户最后一次连接到它们的位置。

它们以 < remote >/< branch > 的形式命名。例如，如果用户想要看用户最后一次与远程仓库 origin 通信时 master 分支的状态，用户可以查看 origin/master 分支。用户与同事合作解决一个问题并且他们推送了一个 iss53 分支，用户可能有自己的本地 iss53 分支，然而在服务器上的分支会以 origin/iss53 来表示。假设用户的网络里有一个在 git.ourcompany.com 的 Git 服务器。如果用户从这里克隆， Git 的 clone 命令会为用户自动将其命名为 origin ，拉取它的所有数据，创建一个指向它的 master 分支的指针，并且在本地将其命名为 origin/master 。 Git 也会给用户一个与 origin 的 master 分支在指向同一个地方的本地 master 分支，这样用户就有工作的基础。

*“origin” 并无特殊含义*
*远程仓库名字 “origin” 与分支名字 “master” 一样，在 Git 中并没有任何特别的含义一样。同时 “master” 是当用户运行 git init 时默认的起始分支名字，原因仅仅是它的广泛使用， “origin” 是当用户运行 git clone 时默认的远程仓库名字。如果用户运行 git clone -o booyah ，那么用户默认的远程分支名字将会是 booyah/master。*

如果用户在本地的 master 分支做了一些工作，在同一段时间内有其他人推送提交到 git.ourcompany.com 并且更新了它的 master 分支，这就是说用户们的提交历史已走向不同的方向。即便这样，只要用户保持不与 origin 服务器连接（并拉取数据），用户的 origin/master 指针就不会移动。

如果要与给定的远程仓库同步数据，运行 git fetch < remote > 命令（在本例中为 git fetch origin ）。这个命令查找 “origin” 是哪一个服务器（在本例中，它是 git.ourcompany.com ），从中抓取本地没有的数据，并且更新本地数据库，移动 origin/master 指针到更新之后的位置。

为了演示有多个远程仓库与远程分支的情况，我们假定用户有另一个内部 Git 服务器，仅服务于用户的某个敏捷开发团队。这个服务器位于 git.team1.ourcompany.com 。用户可以运行 git remote add 命令添加一个新的远程仓库引用到当前的项目。将这个远程仓库命名为 teamone ，将其作为完整 URL 的缩写。
现在，可以运行 git fetch teamone 来抓取远程仓库 teamone 有而本地没有的数据。因为那台服务器上现有的数据是 origin 服务器上的一个子集，所以 Git 并不会抓取数据而是会设置远程跟踪分支 teamone/master 指向 teamone 的 master 分支。

#### 7.7.1 推送

当用户想要公开分享一个分支时，需要将其推送到有写入权限的远程仓库上。本地的分支并不会自动与远程仓库同步——用户必须显式地推送想要分享的分支。这样，用户就可以把不愿意分享的内容放到私人分支上，而将需要和别人协作的内容推送到公开分支。

如果希望和别人一起在名为 serverfix 的分支上工作，用户可以像推送第一个分支那样推送它。运行 git push < remote > < branch > :

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git push origin serverfix
Counting objects: 24, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (24/24), 1.91 KiB | 0 bytes/s, done.
Total 24 (delta 2), reused 0 (delta 0)
To https://github.com/schacon/simplegit
 * [new branch]      serverfix -> serverfix
~~~

Git 自动将 serverfix 分支名字展开为 refs/heads/serverfix:refs/heads/serverfix ，那意味着，“推送本地的 serverfix 分支来更新远程仓库上的 serverfix 分支”。用户也可以运行 git push origin serverfix:serverfix ，它会做同样的事——也就是说“推送本地的 serverfix 分支，将其作为远程仓库的 serverfix 分支” 可以通过这种格式来推送本地分支到一个命名不相同的远程分支。如果并不想让远程仓库上的分支叫做 serverfix ，可以运行 git pushorigin serverfix:awesomebranch 来将本地的 serverfix 分支推送到远程仓库上的 awesomebranch 分支。

如果用户正在使用 HTTPS URL 来推送， Git 服务器会询问用户名与密码。默认情况下它会在终端中提示服务器是否允许用户进行推送。如果不想在每一次推送时都输入用户名与密码，用户可以设置一个 “credential cache” 。最简单的方式就是将其保存在内存中几分钟，可以简单地运行 git config --globalcredential.helper cache 来设置它。

下一次其他协作者从服务器上抓取数据时，他们会在本地生成一个远程分支 origin/serverfix ，指向服务器的 serverfix 分支的引用：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git fetch origin
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/schacon/simplegit
 * [new branch]      serverfix    -> origin/serverfix
~~~

要特别注意的一点是当抓取到新的远程跟踪分支时，本地不会自动生成一份可编辑的副本（拷贝）。换一句话说，这种情况下，不会有一个新的 serverfix 分支——只有一个不可以修改的 origin/serverfix 指针。可以运行 git merge origin/serverfix 将这些工作合并到当前所在的分支。如果想要在自己的 serverfix 分支上工作，可以将其建立在远程跟踪分支之上：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
~~~

这会给用户一个用于工作的本地分支，并且起点位于 origin/serverfix 。

#### 7.7.2 跟踪分支

从一个远程跟踪分支检出一个本地分支会自动创建所谓的“跟踪分支”（它跟踪的分支叫做“上游分支”）。跟踪分支是与远程分支有直接关系的本地分支。如果在一个跟踪分支上输入 git pull ， Git 能自动地识别去哪个服务器上抓取、合并到哪个分支。

当克隆一个仓库时，它通常会自动地创建一个跟踪 origin/master 的 master 分支。如果用户愿意的话可以设置其他的跟踪分支，或是一个在其他远程仓库上的跟踪分支，又或者不跟踪 master 分支。最简单的实例就是像之前看到的那样，运行 git checkout -b < branch > < remote >/< branch > 。 这是一个十分常用的操作所以 Git 提供了 --track 快捷方式：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout --track origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
~~~

如果用户尝试检出的分支 (a) 不存在且 (b) 刚好只有一个名字与之匹配的远程分支，那么 Git 就会为用户创建一个跟踪分支：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
~~~

如果想要将本地分支与远程分支设置为不同的名字，用户可以轻松地使用上一个命令增加一个不同名字的本地分支：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout -b sf origin/serverfix
Branch sf set up to track remote branch serverfix from origin.
Switched to a new branch 'sf'
~~~

现在，本地分支 sf 会自动从 origin/serverfix 拉取。

*设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支，可以在任意时间使用 -u 或 --set-upstream-to 选项运行 git branch 来显式地设置。*

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
~~~

当设置好跟踪分支后，可以通过简写 @{upstream} 或 @{u} 来引用它的上游分支。所以在 master 分支时并且它正在跟踪 origin/master 时，如果愿意的话可以使用 git merge @{u} 来取代 git merge origin/master 。

如果想要查看设置的所有跟踪分支，可以使用 git branch的 -vv 选项。 这会将所有的本地分支列出来并且包含更多的信息，如每一个分支正在跟踪哪个远程分支与本地分支是否是领先、落后或是都有。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
  master    1ae2a45 [origin/master] deploying index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] thisshould do it
  testing   5ea463a trying something new
~~~

可以看到 iss53 分支正在跟踪 origin/iss53 并且 “ahead” 是 2 ，意味着本地有两个提交还没有推送到服务器上。也能看到 master 分支正在跟踪 origin/master 分支并且是最新的。接下来可以看到 serverfix 分支正在跟踪 teamone 服务器上的 server-fix-good 分支并且领先 3 落后 1 ，意味着服务器上有一次提交还没有合并入同时本地有三次提交还没有推送。最后看到 testing 分支并没有跟踪任何远程分支。

需要重点注意的一点是这些数字的值来自于用户从每个服务器上最后一次抓取的数据。这个命令并没有连接服务器，它只会告诉用户关于本地缓存的服务器数据。如果想要统计最新的领先与落后数字，需要在运行此命令前抓取所有的远程仓库。可以像这样做：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git fetch --all; git branch -vv
~~~

#### 7.7.3 拉取

当 git fetch 命令从服务器上抓取本地没有的数据时，它并不会修改工作目录中的内容。它只会获取数据然后让用户自己合并。 git pull 在大多数情况下的含义是一个 git fetch 紧接着一个git merge 命令。如果有一个设置好的跟踪分支，不管是显式地设置还是通过 clone 或 checkout 命令为用户创建的，git pull 都会查找当前分支所跟踪的服务器与分支，从服务器上抓取数据然后尝试合并入那个远程分支。

由于 git pull 的魔法经常令人困惑所以通常单独显式地使用 fetch 与 merge 命令会更好一些。

#### 7.7.4 删除远程分支

假设用户已经通过远程分支做完所有的工作了，可以运行带有 --delete 选项的 git push 命令来删除一个远程分支。 如果想要从服务器上删除 serverfix 分支，运行下面的命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted]         serverfix
~~~

基本上这个命令做的只是从服务器上移除这个指针。 Git 服务器通常会保留数据一段时间直到垃圾回收运行，所以如果不小心删除掉了，通常是很容易恢复的。

### 7.8 变基

在 Git 中整合来自不同分支的修改主要有两种方法： merge 以及 rebase 。

#### 7.8.1 变基的基本操作

整合分支最容易的方法是 merge 命令。它会把两个分支的最新快照（ C3 和 C4 ）以及二者最近的共同祖先（ C2 ）进行三方合并，合并的结果是生成一个新的快照（并提交）。还有一种方法：用户可以提取在 C4 中引入的补丁和修改，然后在 C3 的基础上应用一次。在 Git 中，这种操作就叫做 变基（ rebase ）。用户可以使用 rebase 命令将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。

检出 experiment 分支，然后将它变基到 master 分支上：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout experiment

MasterChief@DESKTOP MINGW64 ~
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: added staged command
~~~

原理是首先找到这两个分支（即当前分支 experiment 、变基操作的目标基底分支 master ）的最近共同祖先 C2 ，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底 C3 ，最后以此将之前另存为临时文件的修改依序应用。

回到 master 分支，进行一次快进合并。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout master

MasterChief@DESKTOP MINGW64 ~
$ git merge experiment
~~~

这两种整合方法的最终结果没有任何区别，但是变基使得提交历史更加整洁。在查看一个经过变基的分支的历史记录时会发现，尽管实际的开发工作是并行的，但它们看上去就像是串行的一样，提交历史是一条直线没有分叉。

一般我们这样做的目的是为了确保在向远程分支推送时能保持提交历史的整洁——例如向某个其他人维护的项目贡献代码时。在这种情况下，用户首先在自己的分支里进行开发，当开发完成时用户需要先将用户的代码变基到 origin/master 上，然后再向主项目提交修改。这样的话，该项目的维护者就不再需要进行整合工作，只需要快进合并便可。

#### 7.8.2 更有趣的变基例子

在对两个分支进行变基时，所生成的“重放”并不一定要在目标分支上应用，用户也可以指定另外的一个分支进行应用。

用户创建了一个主题分支 server ，为服务端添加了一些功能，提交了 C3 和 C4。然后从 C3 上创建了主题分支 client ，为客户端添加了一些功能，提交了 C8 和 C9 。最后，用户回到 server 分支，又提交了 C10 。

假设用户希望将 client 中的修改合并到主分支并发布，但暂时并不想合并 server 中的修改，因为它们还需要经过更全面的测试。这时，用户就可以使用 git rebase 命令的 --onto 选项，选中在 client 分支里但不在 server 分支里的修改（即 C8 和 C9 ），将它们在 master 分支上重放：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git rebase --onto master server client
~~~

以上命令的意思是：“取出 client 分支，找出它从 server 分支分歧之后的补丁， 然后把这些补丁在 master 分支上重放一遍，让 client 看起来像直接基于 master 修改一样”。

现在可以快进合并 master 分支了。（如图 快进合并 master 分支，使之包含来自 client 分支的修改）：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout master

MasterChief@DESKTOP MINGW64 ~
$ git merge client
~~~

接下来用户决定将 server 分支中的修改也整合进来。 使用 git rebase < basebranch > < topicbranch > 命令可以直接将主题分支（即本例中的 server ）变基到目标分支（即 master ）上。这样做能省去用户先切换到 server 分支，再对其执行变基命令的多个步骤。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git rebase master server
~~~

然后就可以快进合并主分支 master 了：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git checkout master

MasterChief@DESKTOP MINGW64 ~
$ git merge server
~~~

至此， client 和 server 分支中的修改都已经整合到主分支里了，用户可以删除这两个分支，最终提交历史会变成图最终的提交历史中的样子：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git branch -d client

MasterChief@DESKTOP MINGW64 ~
$ git branch -d server
~~~

#### 7.8.3 变基的风险

如果提交存在于用户的仓库之外，而别人可能基于这些提交进行开发，那么不要执行变基。

如果用户遵循这条金科玉律，就不会出差错。

变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交。如果用户已经将提交推送至某个仓库，而其他人也已经从该仓库拉取提交并进行了后续工作，此时，如果用户用 git rebase 命令重新整理了提交并再次推送，用户的同伴因此将不得不再次将他们手头的工作与用户的提交进行整合，如果接下来用户还要拉取并整合他们修改过的提交，事情就会变得一团糟。

#### 7.8.4 用变基解决变基

使用 git pull --rebase 命令而不是直接 git pull 。又或者用户可以自己手动完成这个过程，先 git fetch ，再 git rebase teamone/master 。如果用户习惯使用 git pull ，同时又希望默认使用选项 --rebase ，用户可以执行这条语句 git config --global pull.rebase true 来更改 pull.rebase 的默认配置。

如果用户只对不会离开用户电脑的提交执行变基，那就不会有事。如果用户对已经推送过的提交执行变基，但别人没有基于它的提交，那么也不会有事。如果用户对已经推送至共用仓库的提交上执行变基命令，并因此丢失了一些别人的开发所基于的提交，那用户就有大麻烦了，用户的同事也会因此鄙视用户。如果用户或用户的同事在某些情形下决意要这么做，请一定要通知每个人执行 git pull --rebase 命令，这样尽管不能避免伤痛，但能有所缓解。

#### 7.8.5 变基 vs 合并

总的原则是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作，这样，用户才能享受到两种方式带来的便利。

## 8 服务器上的 Git

到目前为止，用户应该已经可以使用 Git 来完成日常工作。然而，为了使用 Git 协作功能，用户还需要有远程的 Git 仓库。尽管在技术上用户可以从个人仓库进行推送（push）和拉取（pull）来修改内容，但不鼓励使用这种方法，因为很容易弄混其他人的进度。此外，用户希望合作者们即使在电脑未联机时亦能存取仓库 —— 拥有一个更可靠的公用仓库十分有用。因此，与他人合作的最佳方法即是建立一个用户与合作者们都有权利访问，且可从那里推送和拉取资料的共用仓库。

架设一台 Git 服务器并不难。首先，选择用户希望服务器使用的通讯协议。在本章第一节将介绍可用的协议以及各自优缺点。下面一节将解释使用那些协议的典型设置及如何在用户的服务器上运行。最后，如果用户不介意托管用户的代码在其他人的服务器，且不想经历设置与维护自己服务器的麻烦，可以试试我们介绍的几个仓库托管服务。

如果用户对架设自己的服务器没兴趣，可以跳到本章最后一节去看看如何申请一个代码托管服务的帐户然后继续下一章，我们会在那里讨论分散式源码控制环境的林林总总。

一个远程仓库通常只是一个裸仓库（bare repository） —— 即一个没有当前工作目录的仓库。因为该仓库仅仅作为合作媒介，不需要从磁盘检查快照；存放的只有 Git 的资料。简单的说，裸仓库就是用户工程目录内的 .git 子目录内容，不包含其他资料。

### 8.1 协议

Git 可以使用四种不同的协议来传输资料：本地协议（Local），HTTP 协议，SSH（Secure Shell）协议及 Git 协议。

#### 8.1.1 本地协议

最基本的就是*本地协议（Local protocol）*，其中的远程版本库就是同一主机上的另一个目录。常见于团队每一个成员都对一个共享的文件系统（例如一个挂载的 NFS）拥有访问权，或者比较少见的多人共用同一台电脑的情况。后者并不理想，因为所有代码版本库如果长存于同一台电脑，更可能发生灾难性的损失。

如果使用共享文件系统，就可以从本地版本库克隆（clone）、推送（push）以及拉取（pull）。 像这样去克隆一个版本库或者增加一个远程到现有的项目中，使用版本库路径作为 URL。 例如，克隆一个本地版本库，可以执行如下的命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone /srv/git/project.git
~~~

或你可以执行这个命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone file:///srv/git/project.git
~~~

如果在 URL 开头明确的指定 file:// ，那么 Git 的行为会略有不同。如果仅是指定路径， Git 会尝试使用硬链接（hard link）或直接复制所需要的文件。如果指定 file:// ， Git 会触发平时用于网路传输资料的进程，那样传输效率会更低。指定 file:// 的主要目的是取得一个没有外部参考（extraneous references） 或对象（object）的干净版本库副本——通常是在从其他版本控制系统导入后或一些类似情况需要这么做 （关于维护任务可参见 Git 内部原理）。在此我们将使用普通路径，因为这样通常更快。

要增加一个本地版本库到现有的 Git 项目，可以执行如下的命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git remote add local_proj /srv/git/project.git
~~~

然后，就可以通过新的远程仓库名 local_proj 像在网络上一样从远端版本库推送和拉取更新了。

##### 8.1.1.1 优点

基于文件系统的版本库的优点是简单，并且直接使用了现有的文件权限和网络访问权限。如果团队已经有共享文件系统，建立版本库会十分容易。只需要像设置其他共享目录一样，把一个裸版本库的副本放到大家都可以访问的路径，并设置好读/写的权限，就可以了，我们会在 在服务器上搭建 Git 讨论如何导出一个裸版本库。

这也是快速从别人的工作目录中拉取更新的方法。如果用户和别人一起合作一个项目，他想让用户从版本库中拉取更新时，运行类似 git pull /home/john/project 的命令比推送到服务器再抓取回来简单多了。

##### 8.1.1.2 缺点

这种方法的缺点是，通常共享文件系统比较难配置，并且比起基本的网络连接访问，这不方便从多个位置访问。如果用户想从家里推送内容，必须先挂载一个远程磁盘，相比网络连接的访问方式，配置不方便，速度也慢。

值得一提的是，如果用户使用的是类似于共享挂载的文件系统时，这个方法不一定是最快的。访问本地版本库的速度与用户访问数据的速度是一样的。在同一个服务器上，如果允许 Git 访问本地硬盘，一般的通过 NFS 访问版本库要比通过 SSH 访问慢。

最终，这个协议并不保护仓库避免意外的损坏。每一个用户都有“远程”目录的完整 shell 权限，没有方法可以阻止他们修改或删除 Git 内部文件和损坏仓库。

#### 8.1.2 HTTP 协议

Git 通过 HTTP 通信有两种模式。在 Git 1.6.6 版本之前只有一个方式可用，十分简单并且通常是只读模式的。Git 1.6.6 版本引入了一种新的、更智能的协议，让 Git 可以像通过 SSH 那样智能的协商和传输数据。之后几年，这个新的 HTTP 协议因为其简单、智能变的十分流行。新版本的 HTTP 协议一般被称为 智能 HTTP 协议，旧版本的一般被称为 哑 HTTP 协议。

##### 8.1.2.1 智能 HTTP 协议

智能 HTTP 的运行方式和 SSH 及 Git 协议类似，只是运行在标准的 HTTP/S 端口上并且可以使用各种 HTTP 验证机制，这意味着使用起来会比 SSH 协议简单的多，比如可以使用 HTTP 协议的用户名/密码授权，免去设置SSH 公钥。

智能 HTTP 协议或许已经是最流行的使用 Git 的方式了，它即支持像 git:// 协议一样设置匿名服务，也可以像 SSH 协议一样提供传输时的授权和加密。而且只用一个 URL 就可以都做到，省去了为不同的需求设置不同的URL。如果要推送到一个需要授权的服务器上（一般来讲都需要），服务器会提示输入用户名和密码。从服务器获取数据时也一样。

事实上，类似 GitHub 的服务，在网页上看到的 URL（比如 [https://github.com/schacon/simplegit] ），和在克隆、推送（如果有权限）时使用的是一样的。

##### 8.1.2.2 哑（Dumb） HTTP 协议

如果服务器没有提供智能 HTTP 协议的服务，Git 客户端会尝试使用更简单的“哑” HTTP 协议。哑 HTTP 协议里 web 服务器仅把裸版本库当作普通文件来对待，提供文件服务。哑 HTTP 协议的优美之处在于设置起来简单。基本上，只需要把一个裸版本库放在 HTTP 根目录，设置一个叫做 post-update 的挂钩就可以了（见 Git钩子）。此时，只要能访问 web 服务器上你的版本库，就可以克隆版本库。下面是设置从 HTTP 访问版本库的方法：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cd /var/www/htdocs/

MasterChief@DESKTOP MINGW64 ~
$ git clone --bare /path/to/git_project gitproject.git

MasterChief@DESKTOP MINGW64 ~
$ cd gitproject.git

MasterChief@DESKTOP MINGW64 ~
$ mv hooks/post-update.sample hooks/post-update

MasterChief@DESKTOP MINGW64 ~
$ chmod a+x hooks/post-update
~~~

这样就可以了。Git 自带的 post-update 挂钩会默认执行合适的命令（git pdate-server-info），来确保通过 HTTP 的获取和克隆操作正常工作。这条命令会在用户通过 SSH 向版本库推送之后被执行；然后别人就可以通过类似下面的命令来克隆：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone https://example.com/gitproject.git
~~~

这里我们用了 Apache 里设置了常用的路径 /var/www/htdocs，不过用户可以使用任何静态 Web 服务器 —— 只需要把裸版本库放到正确的目录下就可以。Git 的数据是以基本的静态文件形式提供的（详情见 Git 内部原理）。

通常的，会在可以提供读/写的智能 HTTP 服务和简单的只读的哑 HTTP 服务之间选一个。极少会将二者混合提供服务。

##### 8.1.2.3 优点

我们将只关注智能 HTTP 协议的优点。

不同的访问方式只需要一个 URL 以及服务器只在需要授权时提示输入授权信息，这两个简便性让终端用户使用Git 变得非常简单。相比 SSH 协议，可以使用用户名/密码授权是一个很大的优势，这样用户就不必须在使用 Git 之前先在本地生成 SSH 密钥对再把公钥上传到服务器。对非资深的使用者，或者系统上缺少 SSH 相关程序的使用者， HTTP 协议的可用性是主要的优势。与 SSH 协议类似， HTTP 协议也非常快和高效。

用户也可以在 HTTPS 协议上提供只读版本库的服务，如此你在传输数据的时候就可以加密数据；或者，你甚至可以让客户端使用指定的 SSL 证书。

另一个好处是 HTTPS 协议被广泛使用，一般的企业防火墙都会允许这些端口的数据通过。

##### 8.1.2.4 缺点

在一些服务器上，架设 HTTPS 协议的服务端会比 SSH 协议的棘手一些。除了这一点，用其他协议提供 Git 服务与智能 HTTP 协议相比就几乎没有优势了。

如果你在 HTTP 上使用需授权的推送，管理凭证会比使用 SSH 密钥认证麻烦一些。然而，你可以选择使用凭证存储工具，比如 macOS 的 Keychain 或者 Windows 的凭证管理器。参考凭证存储如何安全地保存 HTTP 密码。

#### 8.1.3 SSH 协议

架设 Git 服务器时常用 SSH 协议作为传输协议。因为大多数环境下服务器已经支持通过 SSH 访问 —— 即使没有也很容易架设。SSH 协议也是一个验证授权的网络协议；并且，因为其普遍性，架设和使用都很容易。

通过 SSH 协议克隆版本库，你可以指定一个 ssh:// 的 URL：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone ssh://[user@]server/project.git
~~~

或者使用一个简短的 scp 式的写法：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone [user@]server:project.git
~~~

在上面两种情况中，如果你不指定可选的用户名，那么 Git 会使用当前登录的用的名字。

##### 8.1.3.1 优势

用 SSH 协议的优势有很多。首先，SSH 架设相对简单 —— SSH 守护进程很常见，多数管理员都有使用经验，并且多数操作系统都包含了它及相关的管理工具。其次，通过 SSH 访问是安全的 —— 所有传输数据都要经过授权和加密。最后，与 HTTPS 协议、Git 协议及本地协议一样，SSH 协议很高效，在传输前也会尽量压缩数据。

##### 8.1.3.2 缺点

SSH 协议的缺点在于它不支持匿名访问 Git 仓库。如果你使用 SSH，那么即便只是读取数据，使用者也 必须通过 SSH 访问你的主机，这使得 SSH 协议不利于开源的项目，毕竟人们可能只想把你的仓库克隆下来查看。如果你只在公司网络使用，SSH 协议可能是你唯一要用到的协议。如果你要同时提供匿名只读访问和 SSH 协议，那么你除了为自己推送架设 SSH 服务以外，还得架设一个可以让其他人访问的服务。

#### 8.1.4 Git 协议

最后是 Git 协议。这是包含在 Git 里的一个特殊的守护进程；它监听在一个特定的端口（9418），类似于 SSH 服务，但是访问无需任何授权。要让版本库支持 Git 协议，需要先创建一个 git-daemon-export-ok 文件 —— 它是 Git 协议守护进程为这个版本库提供服务的必要条件 —— 但是除此之外没有任何安全措施。要么谁都可以克隆这个版本库，要么谁也不能。这意味着，通常不能通过 Git 协议推送。由于没有授权机制，一旦你开放推送操作，意味着网络上知道这个项目 URL 的人都可以向项目推送数据。不用说，极少会有人这么做。

##### 8.1.4.1 优点

目前， Git 协议是 Git 使用的网络传输协议里最快的。如果你的项目有很大的访问量，或者你的项目很庞大并且不需要为写进行用户授权，架设 Git 守护进程来提供服务是不错的选择。它使用与 SSH 相同的数据传输机制，但是省去了加密和授权的开销。

##### 8.1.4.2 缺点

Git 协议缺点是缺乏授权机制。把 Git 协议作为访问项目版本库的唯一手段是不可取的。一般的做法里，会同时提供 SSH 或者 HTTPS 协议的访问服务，只让少数几个开发者有推送（写）权限，其他人通过 git:// 访问只有读权限。 Git 协议也许也是最难架设的。它要求有自己的守护进程，这就要配置 xinetd、systemd 或者其他的程序，这些工作并不简单。它还要求防火墙开放 9418 端口，但是企业防火墙一般不会开放这个非标准端口。而大型的企业防火墙通常会封锁这个端口。

### 8.2 在服务器上搭建 Git

*这里将要演示在 Linux 服务器上进行一次基本且简化的安装所需的命令与步骤，当然在 macOS 或 Windows 服务器上同样可以运行这些服务。事实上，在你的计算机基础架构中建立一个生产环境服务器，将不可避免的使用到不同的安全措施与操作系统工具。但是，希望你能从本节中获得一些必要的知识。*

在开始架设 Git 服务器前，需要把现有仓库导出为裸仓库——即一个不包含当前工作目录的仓库。这通常是很简单的。为了通过克隆你的仓库来创建一个新的裸仓库，你需要在克隆命令后加上 --bare 选项。按照惯例，裸仓库的目录名以 .git 结尾，就像这样：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone --bare my_project my_project.git
Cloning into bare repository 'my_project.git'...
done.
~~~

现在，你的 my_project.git 目录中应该有 Git 目录的副本了。

整体上效果大致相当于

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cp -Rf my_project/.git my_project.git
~~~

虽然在配置文件中有若干不同，但是对于你的目的来说，这两种方式都是一样的。它只取出 Git 仓库自身，不要工作目录，然后特别为它单独创建一个目录。

#### 8.2.1 把裸仓库放到服务器上

既然你有了裸仓库的副本，剩下要做的就是把裸仓库放到服务器上并设置你的协议。假设一个域名为 git.example.com 的服务器已经架设好，并可以通过 SSH 连接，你想把所有的 Git 仓库放在 /srv/git 目录下。假设服务器上存在 /srv/git/ 目录，你可以通过以下命令复制你的裸仓库来创建一个新仓库：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ scp -r my_project.git user@git.example.com:/srv/git
~~~

此时，其他可通过 SSH 读取此服务器上 /srv/git 目录的用户，可运行以下命令来克隆你的仓库。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone user@git.example.com:/srv/git/my_project.git
~~~

如果一个用户，通过使用 SSH 连接到一个服务器，并且其对 /srv/git/my_project.git 目录拥有可写权限，那么他将自动拥有推送权限。

如果到该项目目录中运行 git init 命令，并加上 --shared 选项，那么 Git 会自动修改该仓库目录的组权限为可写。注意，运行此命令的工程中不会摧毁任何提交、引用等内容。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ ssh user@git.example.com

MasterChief@DESKTOP MINGW64 ~
$ cd /srv/git/my_project.git

MasterChief@DESKTOP MINGW64 ~
$ git init --bare --shared
~~~

由此可见，根据现有的 Git 仓库创建一个裸仓库，然后把它放上你和协作者都有 SSH 访问权的服务器是多么容易。现在你们已经准备好在同一项目上展开合作了。

值得注意的是，这的确是架设一个几个人拥有连接权的 Git 服务的全部 —— 只要在服务器上加入可以用 SSH 登录的帐号，然后把裸仓库放在大家都有读写权限的地方。你已经准备好了一切，无需更多。

下面的几节中，会介绍如何扩展到更复杂的设定。这些内容包含如何避免为每一个用户建立一个账户，给仓库添加公共读取权限，架设网页界面等等。然而，请记住这一点，如果只是和几个人在一个私有项目上合作的话，仅仅是一个 SSH 服务器和裸仓库就足够了。

#### 8.2.2 小型安装

如果设备较少或者你只想在小型开发团队里尝试 Git ，那么一切都很简单。架设 Git 服务最复杂的地方在于用户管理。如果需要仓库对特定的用户可读，而给另一部分用户读写权限，那么访问和许可安排就会比较困难。

##### 8.2.2.1 SSH 连接

如果你有一台所有开发者都可以用 SSH 连接的服务器，架设你的第一个仓库就十分简单了，因为你几乎什么都不用做（正如我们上一节所说的）。如果你想在你的仓库上设置更复杂的访问控制权限，只要使用服务器操作系统的普通的文件系统权限就行了。

如果需要团队里的每个人都对仓库有写权限，又不能给每个人在服务器上建立账户，那么提供 SSH 连接就是唯一的选择了。我们假设用来共享仓库的服务器已经安装了 SSH 服务，而且你通过它访问服务器。

有几个方法可以使你给团队每个成员提供访问权。第一个就是给团队里的每个人创建账号，这种方法很直接但也很麻烦。或许你不会想要为每个人运行一次 adduser（或者 useradd）并且设置临时密码。

第二个办法是在主机上建立一个 git 账户，让每个需要写权限的人发送一个 SSH 公钥，然后将其加入 git 账户的~/.ssh/authorized_keys 文件。这样一来，所有人都将通过 git 账户访问主机。这一点也不会影响提交的数据——访问主机用的身份不会影响提交对象的提交者信息。

另一个办法是让 SSH 服务器通过某个 LDAP 服务，或者其他已经设定好的集中授权机制，来进行授权。只要每个用户可以获得主机的 shell 访问权限，任何 SSH 授权机制你都可视为是有效的。

#### 8.2.3 生成 SSH 公钥

许多 Git 服务器都使用 SSH 公钥进行认证。为了向 Git 服务器提供 SSH 公钥，如果某系统用户尚未拥有密钥，必须事先为其生成一份。这个过程在所有操作系统上都是相似的。首先，你需要确认自己是否已经拥有密钥。默认情况下，用户的 SSH 密钥存储在其 ~/.ssh 目录下。进入该目录并列出其中内容，你便可以快速确认自己是否已拥有密钥：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cd ~/.ssh
$ ls
authorized_keys2  id_dsa       known_hosts
config            id_dsa.pub
~~~

寻找一对以 id_dsa 或 id_rsa 命名的文件，其中一个带有 .pub 扩展名。 .pub 文件是你的公钥，另一个则是与之对应的私钥。如果找不到这样的文件（或者根本没有 .ssh 目录），你可以通过运行 ssh-keygen 程序来创建它们。在 Linux/macOS 系统中， ssh-keygen 随 SSH 软件包提供；在 Windows 上，该程序包含于 MSysGit 软件包中。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ ssh-keygen -o
Generating public/private rsa key pair.
Enter file in which to save the key (/home/schacon/.ssh/id_rsa):
Created directory '/home/schacon/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/schacon/.ssh/id_rsa.
Your public key has been saved in /home/schacon/.ssh/id_rsa.pub.
The key fingerprint is:
d0:82:24:8e:d7:f1:bb:9b:33:53:96:93:49:da:9b:e3 schacon@mylaptop.local
~~~

首先 ssh-keygen 会确认密钥的存储位置（默认是 .ssh/id_rsa），然后它会要求你输入两次密钥口令。如果你不想在使用密钥时输入口令，将其留空即可。然而，如果你使用了密码，那么请确保添加了 -o 选项，它会以比默认格式更能抗暴力破解的格式保存私钥。也可以用 ssh-agent 工具来避免每次都要输入密码。现在，进行了上述操作的用户需要将各自的公钥发送给任意一个 Git 服务器管理员（假设服务器正在使用基于公钥的 SSH 验证设置）。他们所要做的就是复制各自的 .pub 文件内容，并将其通过邮件发送。公钥看起来是这样的：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAklOUpkDHrfHY17SbrmTIpNLTGK9Tjom/BWDSU
GPl+nafzlHDTYW7hdI4yZ5ew18JH4JW9jbhUFrviQzM7xlELEVf4h9lFX5QVkbPppSwg0cda3
Pbv7kOdJ/MTyBlWXFCR+HAo3FXRitBqxiX1nKhXpHAZsMciLq8V6RjsNAQwdsdMFvSlVK/7XA
t3FaoJoAsncM1Q9x5+3V0Ww68/eIFmb1zuUFljQJKprrX88XypNDvjYNby6vw/Pb0rwert/En
mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+gw3r3+1nKatmIkjn2so1d01QraTlMqVSsbx
NrRFi9wrf+M7Q== schacon@mylaptop.local
~~~

#### 8.2.4 配置服务器

本例中，来看看如何配置服务器端的 SSH 访问。我们将使用 authorized_keys 方法来对用户进行认证。同时假设你使用的操作系统是标准的 Linux 发行版，比如 Ubuntu。首先，创建一个操作系统用户 git ，并为其建立一个 .ssh 目录。

*以下操作可通过 ssh-copy-id 命令自动完成，这样就不必手动复制并安装公钥了。*

首先，创建一个操作系统用户 git，并为其建立一个 .ssh 目录。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo adduser git

MasterChief@DESKTOP MINGW64 ~
$ su git

MasterChief@DESKTOP MINGW64 ~
$ cd

MasterChief@DESKTOP MINGW64 ~
$ mkdir .ssh && chmod 700 .ssh

MasterChief@DESKTOP MINGW64 ~
$ touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
~~~

接着，我们需要为系统用户 git 的 authorized_keys 文件添加一些开发者 SSH 公钥。假设我们已经获得了若干受信任的公钥，并将它们保存在临时文件中。与前文类似，这些公钥看起来是这样的：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cat /tmp/id_rsa.john.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCB007n/ww+ouN4gSLKssMxXnBOvf9LGt4L
ojG6rs6hPB09j9R/T17/x4lhJA0F3FR1rP6kYBRsWj2aThGw6HXLm9/5zytK6Ztg3RPKK+4k
Yjh6541NYsnEAZuXz0jTTyAUfrtU3Z5E003C4oxOj6H0rfIF1kKI9MAQLMdpGW1GYEIgS9Ez
Sdfd8AcCIicTDWbqLAcU4UpkaX8KyGlLwsNuuGztobF8m72ALC/nLF6JLtPofwFBlgc+myiv
O7TCUSBdLQlgMVOFq1I2uPWQOkOWQAHukEOmfjy2jctxSDBQ220ymjaNsHT4kgtZg2AYYgPq
dAv8JggJICUvax2T9va5 gsg-keypair
~~~

将这些公钥加入系统用户 git 的 .ssh 目录下 authorized_keys 文件的末尾：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cat /tmp/id_rsa.john.pub >> ~/.ssh/authorized_keys

MasterChief@DESKTOP MINGW64 ~
$ cat /tmp/id_rsa.josie.pub >> ~/.ssh/authorized_keys

MasterChief@DESKTOP MINGW64 ~
$ cat /tmp/id_rsa.jessica.pub >> ~/.ssh/authorized_keys
~~~

现在来为开发者新建一个空仓库。可以借助带 --bare 选项的 git init 命令来做到这一点，该命令在初始化仓库时不会创建工作目录：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cd /srv/git

MasterChief@DESKTOP MINGW64 ~
$ mkdir project.git

MasterChief@DESKTOP MINGW64 ~
$ cd project.git

MasterChief@DESKTOP MINGW64 ~
$ git init --bare
Initialized empty Git repository in /srv/git/project.git/
~~~

接着， John 、 Josie 或者 Jessica 中的任意一人可以将他们项目的最初版本推送到这个仓库中，他只需将此仓库设置为项目的远程仓库并向其推送分支。请注意，每添加一个新项目，都需要有人登录服务器取得 shell，并创建一个裸仓库。我们假定这个设置了 git 用户和 Git 仓库的服务器使用 gitserver 作为主机名。同时，假设该服务器运行在内网，并且你已在 DNS 配置中将 gitserver 指向此服务器。那么我们可以运行如下命令（假定 myproject 是已有项目且其中已包含文件）：

~~~bash
# on John's computer
MasterChief@DESKTOP MINGW64 ~
$ cd myproject

MasterChief@DESKTOP MINGW64 ~
$ git init

MasterChief@DESKTOP MINGW64 ~
$ git add .

MasterChief@DESKTOP MINGW64 ~
$ git commit -m 'initial commit'

MasterChief@DESKTOP MINGW64 ~
$ git remote add origin git@gitserver:/srv/git/project.git

MasterChief@DESKTOP MINGW64 ~
$ git push origin master
~~~

此时，其他开发者可以克隆此仓库，并推回各自的改动，步骤很简单：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone git@gitserver:/srv/git/project.git

MasterChief@DESKTOP MINGW64 ~
$ cd project

MasterChief@DESKTOP MINGW64 ~
$ vim README

MasterChief@DESKTOP MINGW64 ~
$ git commit -am 'fix for the README file'

MasterChief@DESKTOP MINGW64 ~
$ git push origin master
~~~

通过这种方法，你可以快速搭建一个具有读写权限、面向多个开发者的 Git 服务器。

需要注意的是，目前所有（获得授权的）开发者用户都能以系统用户 git 的身份登录服务器从而获得一个普通 shell 。 如果你想对此加以限制，则需要修改 /etc/passwd 文件中（ git 用户所对应）的 shell 值。

借助一个名为 git-shell 的受限 shell 工具，你可以方便地将用户 git 的活动限制在与 Git 相关的范围内。该工具随 Git 软件包一同提供。如果将 git-shell 设置为用户 git 的登录 shell（ login shell ），那么该用户便不能获得此服务器的普通 shell 访问权限。若要使用 git-shell ，需要用它替换掉 bash 或 csh ，使其成为该用户的登录 shell 。为进行上述操作，首先你必须确保 git-shell 的完整路径名已存在于 /etc/shells 文件中：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cat /etc/shells   # see if git-shell is already in there. If not...

MasterChief@DESKTOP MINGW64 ~
$ which git-shell   # make sure git-shell is installed on your system.

MasterChief@DESKTOP MINGW64 ~
$ sudo -e /etc/shells  # and add the path to git-shell from last command
~~~

现在你可以使用 chsh < username > -s < shell > 命令修改任一系统用户的 shell ：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo chsh git -s $(which git-shell)
~~~

这样，用户 git 就只能利用 SSH 连接对 Git 仓库进行推送和拉取操作，而不能登录机器并取得普通 shell 。如果试图登录，你会发现尝试被拒绝，像这样：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ ssh git@gitserver
fatal: Interactive git shell is not enabled.
hint: ~/git-shell-commands should exist and have read and execute access.
Connection to gitserver closed.
~~~

此时，用户仍可通过 SSH 端口转发来访问任何可达的 git 服务器。 如果你想要避免它，可编辑
authorized_keys 文件并在所有想要限制的公钥之前添加以下选项：

~~~bash
no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty
~~~

其结果如下：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cat ~/.ssh/authorized_keys
no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa
AAAAB3NzaC1yc2EAAAADAQABAAABAQCB007n/ww+ouN4gSLKssMxXnBOvf9LGt4LojG6rs6h
PB09j9R/T17/x4lhJA0F3FR1rP6kYBRsWj2aThGw6HXLm9/5zytK6Ztg3RPKK+4kYjh6541N
YsnEAZuXz0jTTyAUfrtU3Z5E003C4oxOj6H0rfIF1kKI9MAQLMdpGW1GYEIgS9EzSdfd8AcC
IicTDWbqLAcU4UpkaX8KyGlLwsNuuGztobF8m72ALC/nLF6JLtPofwFBlgc+myivO7TCUSBd
LQlgMVOFq1I2uPWQOkOWQAHukEOmfjy2jctxSDBQ220ymjaNsHT4kgtZg2AYYgPqdAv8JggJ
ICUvax2T9va5 gsg-keypair
no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa
AAAAB3NzaC1yc2EAAAADAQABAAABAQDEwENNMomTboYI+LJieaAY16qiXiH3wuvENhBG...
~~~

现在，网络相关的 Git 命令依然能够正常工作，但是开发者用户已经无法得到一个普通 shell 了。正如输出信息所提示的，你也可以在 git 用户的主目录下建立一个目录，来对 git-shell 命令进行一定程度的自定义。比如，你可以限制掉某些本应被服务器接受的 Git 命令，或者对刚才的 SSH 拒绝登录信息进行自定义，这样，当有开发者用户以类似方式尝试登录时，便会看到你的信息。 要了解更多有关自定义 shell 的信息，请运行 git help shell 。

#### 8.2.5 Git 守护进程

接下来将通过 “Git” 协议建立一个基于守护进程的仓库。对于快速且无需授权的 Git 数据访问，这是一个理想之选。请注意，因为其不包含授权服务，任何通过该协议管理的内容将在其网络上公开。

如果运行在防火墙之外的服务器上，它应该只对那些公开的只读项目服务。如果运行在防火墙之内的服务器上，它可用于支撑大量参与人员或自动系统（用于持续集成或编译的主机）只读访问的项目，这样可以省去逐一配置 SSH 公钥的麻烦。

无论何时，该 Git 协议都是相对容易设定的。通常，你只需要以守护进程的形式运行该命令：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git daemon --reuseaddr --base-path=/srv/git/ /srv/git/
~~~

--reuseaddr 选项允许服务器在无需等待旧连接超时的情况下重启，而 --base-path 选项允许用户在未完全指定路径的条件下克隆项目，结尾的路径将告诉 Git 守护进程从何处寻找仓库来导出。如果有防火墙正在运行，你需要开放端口 9418 的通信权限。

你可以通过许多方式将该进程以守护进程的方式运行，这主要取决于你所使用的操作系统。

由于在现代的 Linux 发行版中， systemd 是最常见的初始化系统，因此你可以用它来达到此目的。 只要在 /etc/systemd/system/git-daemon.service 中放一个文件即可，其内容如下：

~~~bash
[Unit]
Description=Start Git Daemon
[Service]
ExecStart=/usr/bin/git daemon --reuseaddr --base-path=/srv/git/ /srv/git/
Restart=always
RestartSec=500ms
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=git-daemon
User=git
Group=git
[Install]
WantedBy=multi-user.target
~~~

你可能会注意这里以 git 启动的 Git 驻留程序同时使用了 Group 和 User 权限。按需修改它并确保提供的用户在此系统上。此外，请确保 Git 二进制文件位于 /usr/bin/git ，必要时可修改此路径。

最后，你需要运行 systemctl enable git-daemon 以让它在系统启动时自动运行，这样也能让它通过 systemctl start git-daemon 启动，通过 systemctl stop git-daemon 停止。

在其他系统中，你可以使用 sysvinit 系统中的 xinetd 脚本，或者另外的方式来实现——只要你能够将其命令守护进程化并实现监控。

接下来，你需要告诉 Git 哪些仓库允许基于服务器的无授权访问。你可以在每个仓库下创建一个名为 git-daemon-export-ok 的文件来实现。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ cd /path/to/project.git

MasterChief@DESKTOP MINGW64 ~
$ touch git-daemon-export-ok
~~~

该文件将允许 Git 提供无需授权的项目访问服务。

#### 8.2.6 Smart HTTP

我们一般通过 SSH 进行授权访问，通过 git:// 进行无授权访问，但是还有一种协议可以同时实现以上两种方式的访问。设置 Smart HTTP 一般只需要在服务器上启用一个 Git 自带的名为 git-http-backend 的 CGI 脚本。该 CGI 脚本将会读取由 git fetch 或 git push 命令向 HTTP URL 发送的请求路径和头部信息，来判断该客户端是否支持 HTTP 通信（不低于1.6.6版本的客户端支持此特性）。如果 CGI 发现该客户端支持智能（Smart）模式，它将会以智能模式与它进行通信，否则它将会回落到哑（Dumb）模式下（因此它可以对某些老的客户端实现向下兼容）。

在完成以上简单的安装步骤后， 我们将用 Apache 来作为 CGI 服务器。 如果你没有安装 Apache ，你可以在 Linux 环境下执行如下或类似的命令来安装：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ sudo apt-get install apache2 apache2-utils

MasterChief@DESKTOP MINGW64 ~
$ a2enmod cgi alias env
~~~

该操作将会启用 mod_cgi ， mod_alias 和 mod_env 等 Apache 模块，这些模块都是使该功能正常工作所必须的。

你还需要将 /srv/git 的 Unix 用户组设置为 www-data ，这样 Web 服务器才能读写该仓库，因为 运行 CGI 脚本的 Apache 实例默认会以该用户的权限运行：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ chgrp -R www-data /srv/git
~~~

接下来我们要向 Apache 配置文件添加一些内容，来让 git-http-backend 作为 Web 服务器对 /git 路径请求的处理器。

~~~bash
SetEnv GIT_PROJECT_ROOT /srv/git
SetEnv GIT_HTTP_EXPORT_ALL
ScriptAlias /git/ /usr/lib/git-core/git-http-backend/
~~~

如果留空 GIT_HTTP_EXPORT_ALL 这个环境变量， Git 将只对无授权客户端提供带 git-daemon-export-ok 文件的版本库，就像 Git 守护进程一样。

最后，如果想让 Apache 允许 git-http-backend 请求并实现写入操作的授权验证，使用如下授权屏蔽配置即可：

~~~bash
<Files "git-http-backend">
    AuthType Basic
    AuthName "Git Access"
    AuthUserFile /srv/git/.htpasswd
    Require expr !(%{QUERY_STRING} -strmatch '*service=git-receive-pack*'
|| %{REQUEST_URI} =~ m#/git-receive-pack$#)
    Require valid-user
</Files>
~~~

这需要你创建一个包含所有合法用户密码的 .htpasswd 文件。以下是一个添加 “schacon” 用户到此文件的例子：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ htpasswd -c /srv/git/.htpasswd schacon
~~~

你可以通过许多方式添加 Apache 授权用户，选择使用其中一种方式即可。以上仅仅只是我们可以找到的最简单的一个例子。如果愿意的话，你也可以通过 SSL 运行它，以保证所有数据是在加密状态下进行传输的。

我们不想深入去讲解 Apache 配置文件，因为你可能会使用不同的 Web 服务器，或者可能有不同的授权需求。

它的主要原理是使用一个 Git 附带的，名为 git-http-backend 的 CGI 。它被引用来处理协商通过 HTTP 发送和接收的数据。它本身并不包含任何授权功能，但是授权功能可以在 Web 服务器层引用它时被轻松实现。你可以在任何所有可以处理 CGI 的 Web 服务器上办到这点，所以随便挑一个你最熟悉的 Web 服务器试手吧。

*欲了解更多的有关配置 Apache 授权访问的信息，请通过以下链接浏览 Apache 文档：*
*[https://httpd.apache.org/docs/current/howto/auth.html]*

#### 8.2.7 GitWeb

如果你对项目有读写权限或只读权限，你可能需要建立起一个基于网页的简易查看器。 Git 提供了一个叫做 GitWeb 的 CGI 脚本来做这项工作。

如果你想要查看 GitWeb 如何展示你的项目，并且在服务器上安装了轻量级 Web 服务器比如 lighttpd 或 webrick ， Git 提供了一个命令来让你启动一个临时的服务器。在 Linux 系统的电脑上， lighttpd 通常已经安装了，所以你只需要在项目目录里执行 git instaweb 命令即可。如果你使用 Mac 系统， Mac OS X Leopard 系统已经预安装了 Ruby ，所以 webrick 或许是你最好的选择。 如果不想使用 lighttpd 启动 instaweb 命令，你需要在执行时加入 --httpd 参数。

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git instaweb --httpd=webrick
[2009-02-21 10:02:21] INFO  WEBrick 1.3.1
[2009-02-21 10:02:21] INFO  ruby 1.8.6 (2008-03-03) [universal-darwin9.0]
~~~

这个命令启动了一个监听 1234 端口的 HTTP 服务器，并且自动打开了浏览器。这对你来说十分方便。 当你已经完成了工作并想关闭这个服务器，你可以执行同一个命令，并加上 --stop 选项：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git instaweb --httpd=webrick --stop
~~~

如果你现在想为你的团队或你托管的开源项目持续的运行这个页面，你需要通过普通的 Web 服务器来设置 CGI脚本。一些 Linux 发行版的软件库有 gitweb 包，可以通过 apt 或 dnf 来安装，你可以先试试。接下来我们来快速的了解一下如何手动安装 GitWeb 。首先，你需要获得 Git 的源代码，它包含了 GitWeb ，并可以生成自定义的 CGI 脚本：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone git://git.kernel.org/pub/scm/git/git.git

MasterChief@DESKTOP MINGW64 ~
$ cd git/

MasterChief@DESKTOP MINGW64 ~
$ make GITWEB_PROJECTROOT="/srv/git" prefix=/usr gitweb
    SUBDIR gitweb
    SUBDIR ../
make[2]: `GIT-VERSION-FILE' is up to date.
    GEN gitweb.cgi
    GEN static/gitweb.js

MasterChief@DESKTOP MINGW64 ~
$ sudo cp -Rf gitweb /var/www/
~~~

需要注意的是，你需要在命令中指定 GITWEB_PROJECTROOT 变量来让程序知道你的 Git 版本库的位置。现在，你需要在 Apache 中使用这个 CGI 脚本，你需要为此添加一个虚拟主机：

~~~bash
<VirtualHost *:80>
    ServerName gitserver
    DocumentRoot /var/www/gitweb
    <Directory /var/www/gitweb>
        Options +ExecCGI +FollowSymLinks +SymLinksIfOwnerMatch
        AllowOverride All
        order allow,deny
        Allow from all
        AddHandler cgi-script cgi
        DirectoryIndex gitweb.cgi
    </Directory>
</VirtualHost>
~~~

再次提醒，GitWeb 可以通过任何一个支持 CGI 或 Perl 的网络服务器架设；如果你需要的话，架设起来应该不会很困难。 现在，你可以访问 [http://gitserver/] 在线查看你的版本库。

#### 8.2.8 GitLab

虽然 GitWeb 相当简单。但如果你正在寻找一个更现代，功能更全的 Git 服务器，这里有几个开源的解决方案可供你选择安装。因为 GitLab 是其中最出名的一个，我们将它作为示例并讨论它的安装和使用。这比 GitWeb 要复杂的多并且需要更多的维护，但它的确是一个功能更全的选择。

##### 8.2.8.1 安装

GitLab 是一个数据库支持的 web 应用，所以相比于其他 git 服务器，它的安装过程涉及到更多的东西。幸运的是，这个过程有非常详细的文档说明和支持。

这里有一些可参考的方法帮你安装 GitLab 。为了更快速的启动和运行，你可以下载虚拟机镜像或者在
[https://bitnami.com/stack/gitlab] 上获取一键安装包，同时调整配置使之符合你特定的环境。 Bitnami 的一个优点在于它的登录界面（通过 alt+→ 键进入），它会告诉你安装好的 GitLab 的 IP 地址以及默认的用户名和密码。

无论如何，跟着 GitLab 社区版的 readme 文件一步步来，你可以在这里找到它 [https://gitlab.com/gitlab-org/gitlab-ce/tree/master] 。在这里你将会在主菜单中找到安装 GitLab 的帮助，一个可以在 Digital Ocean 上运行的虚拟机，以及 RPM 和 DEB 包（都是测试版）。这里还有 “非官方” 的引导让 GitLab 运行在非标准的操作系统和数据库上，一个全手动的安装脚本，以及许多其他的话题。

##### 8.2.8.2 管理

GitLab 的管理界面是通过网络进入的。将你的浏览器转到已经安装 GitLab 的 主机名或 IP 地址，然后以管理员身份登录即可。默认的用户名是 admin@local.host ，默认的密码是 5iveL!fe（你会得到类似 请登录后尽快更换密码 的提示）。登录后，点击主栏上方靠右位置的 “Admin area” 图标进行管理。

###### 8.2.8.2.1 使用者

GitLab 上的用户指的是对应协作者的帐号。用户帐号没有很多复杂的地方，主要是包含登录数据的用户信息集合。每一个用户账号都有一个命名空间 ，即该用户项目的逻辑集合。如果一个叫 jane 的用户拥有一个名称是 project 的项目，那么这个项目的 url 会是 [http://server/jane/project] 。

移除一个用户有两种方法。“屏蔽（Blocking）”一个用户阻止他登录 GitLab 实例，但是该用户命名空间下的所有数据仍然会被保存，并且仍可以通过该用户提交对应的登录邮箱链接回他的个人信息页。
而另一方面，“销毁（Destroying）” 一个用户，会彻底的将他从数据库和文件系统中移除。他命名空间下的所有项目和数据都会被删除，拥有的任何组也会被移除。这显然是一个更永久且更具破坏力的行为，所以很少用到这种方法。

###### 8.2.8.2.2 组

一个 GitLab 的组是一些项目的集合，连同关于多少用户可以访问这些项目的数据。每一个组都有一个项目命名空间（与用户一样），所以如果一个叫 training 的组拥有一个名称是 materials 的项目，那么这个项目的 url 会是 [http://server/training/materials] 。

每一个组都有许多用户与之关联，每一个用户对组中的项目以及组本身的权限都有级别区分。权限的范围从“访客”（仅能提问题和讨论） 到 “拥有者”（完全控制组、成员和项目）。权限的种类太多以至于难以在这里一一列举，不过在 GitLab 的管理界面上有帮助链接。

###### 8.2.8.2.3 项目

一个 GitLab 的项目相当于 git 的版本库。每一个项目都属于一个用户或者一个组的单个命名空间。如果这个项目属于一个用户，那么这个拥有者对所有可以获取这个项目的人拥有直接管理权；如果这个项目属于一个组，那么该组中用户级别的权限也会起作用。

每一个项目都有一个可视级别，控制着谁可以看到这个项目页面和仓库。如果一个项目是 私有 的，这个项目的拥有者必须明确授权从而使特定的用户可以访问。一个内部的项目可以被所有登录的人看到，而一个公开的项目则是对所有人可见的。注意，这种控制既包括 git fetch 的使用也包括对项目 web 用户界面的访问。

###### 8.2.8.2.4 钩子

GitLab 在项目和系统级别上都支持钩子程序。对任意级别，当有相关事件发生时，GitLab 的服务器会执行一个包含描述性 JSON 数据的 HTTP 请求。 这是自动化连接你的 git 版本库和 GitLab 实例到其他的开发工具，比如 CI 服务器，聊天室，或者部署工具的一个极好方法。

##### 8.2.8.3 基本用途

你想要在 GitLab 做的第一件事就是建立一个新项目。这通过点击工具栏上的 “+” 图标完成。你会被要求填写项目名称，也就是这个项目所属的命名空间，以及它的可视层级。绝大多数的设定并不是永久的，可以通过设置界面重新调整。点击 “Create Project”，你就完成了。

项目存在后，你可能会想将它与本地的 Git 版本库连接。每一个项目都可以通过 HTTPS 或者 SSH 连接，任意两者都可以被用来配置远程 Git。在项目主页的顶栏可以看到这个项目的 URLs。对于一个存在的本地版本库，这个命令将会向主机位置添加一个叫 gitlab 的远程仓库：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git remote add gitlab https://server/namespace/project.git
~~~

如果你的本地没有版本库的副本，你可以这样做：

~~~bash
MasterChief@DESKTOP MINGW64 ~
$ git clone https://server/namespace/project.git
~~~

web 用户界面提供了几个有用的获取版本库信息的网页。每一个项目的主页都显示了最近的活动，并且通过顶部的链接可以使你浏览项目文件以及提交日志。

##### 8.2.8.4 一起工作

在一个 GitLab 项目上一起工作的最简单方法就是赋予协作者对 git 版本库的直接 push 权限。你可以通过项目设定的 “Members（成员）” 部分向一个项目添加写作者，并且将这个新的协作者与一个访问级别关联（不同的访问级别在 组 中已简单讨论）。通过赋予一个协作者 “Developer（开发者）” 或者更高的访问级别，这个用户就可以毫无约束地直接向版本库或者向分支进行提交。

另外一个让合作更解耦的方法就是使用合并请求。 它的优点在于让任何能够看到这个项目的协作者在被管控的情况下对这个项目作出贡献。可以直接访问的协作者能够简单的创建一个分支，向这个分支进行提交，也可以开启一个向 master 或者其他任何一个分支的合并请求。对版本库没有推送权限的协作者则可以 “fork” 这个版本库（即创建属于自己的这个库的副本），向那个副本进行提交，然后从那个副本开启一个到主项目的合并请求。这个模型使得项目拥有者完全控制着向版本库的提交，以及什么时候允许加入陌生协作者的贡献。

在 GitLab 中合并请求和问题是一个长久讨论的主要部分。每一个合并请求都允许在提出改变的行进行讨论（它支持一个轻量级的代码审查），也允许对一个总体性话题进行讨论。两者都可以被分配给用户，或者组织到milestones（里程碑） 界面。

这个部分主要聚焦于在 GitLab 中与 Git 相关的特性，但是 GitLab 作为一个成熟的系统，它提供了许多其他产品来帮助你协同工作，例如项目 wiki 与系统维护工具。 GitLab 的一个优点在于，服务器设置和运行以后，你将很少需要调整配置文件或通过 SSH 连接服务器。绝大多数的管理和日常使用都可以在浏览器界面中完成。

##### 8.2.8.5 第三方托管的选择

如果不想设立自己的 Git 服务器，你可以选择将你的 Git 项目托管到一个外部专业的托管网站。这带来了一些好处：一个托管网站可以用来快速建立并开始项目，且无需进行服务器维护和监控工作。即使你在内部设立并且运行了自己的服务器，你仍然可以把你的开源代码托管在公共托管网站——这通常更有助于开源社区来发现和帮助你。

现在，有非常多的托管供你选择，每个选择都有不同的优缺点。欲查看最新列表，请浏览 Git 维基的
GitHosting 页面 [https://git.wiki.kernel.org/index.php/GitHosting] ，我们会在 GitHub 详细讲解 GitHub ，作为目前最大的 Git 托管平台，你很可能需要与托管在 GitHub 上的项目进行交互，而且你也很可能并不想去设立你自己的 Git 服务器。

##### 8.2.8.6 总结

你有多种远程存取 Git 仓库的选择便于与其他人合作或是分享你的工作。

运行你自己的服务器将有许多权限且允许你运行该服务于你自己的防火墙内，但如此通常需要耗费你大量的时间去设置与维护服务器。如果你放置你的资料于托管服务器内，可轻易的设置与维护；无论如何，你必须能够保存你的代码在其他服务器，且某些组织不允许此作法。这将直截了当的决定哪个作法或组合的方式较适合你或你的组织。

### 8.3 分布式 Git

你现在拥有了一个远程 Git 版本库，能为所有开发者共享代码提供服务，在一个本地工作流程下，你也已经熟悉了基本 Git 命令。你现在可以学习如何利用 Git 提供的一些分布式工作流程了。

这一章中，你将会学习如何作为贡献者或整合者，在一个分布式协作的环境中使用 Git。你会学习为一个项目成功地贡献代码，并接触一些最佳实践方式，让你和项目的维护者能轻松地完成这个过程。另外，你也会学到如何管理有很多开发者提交贡献的项目。


| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |
