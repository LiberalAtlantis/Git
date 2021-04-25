# Git简明教程

## 1 Git下载

### 1.0 概述

可以根据主机系统选择对应系统版本下载安装。

下载地址
[https://git-scm.com/downloads]

### 1.1 Linux

以 Fedora 为例，如果你在使用它（或与之紧密相关的基于 RPM 的发行版，如 RHEL 或 CentOS），你可以使用 dnf ：

~~~bash
$ sudo dnf install git-all
~~~

如果你在基于 Debian 的发行版上，如 Ubuntu，请使用 apt ：

~~~bash
$ sudo apt install git-all
~~~

下载地址
[https://git-scm.com/download/linux]

### 1.2 macOS

在 Mac 上安装 Git 有多种方式。 最简单的方法是安装 Xcode Command Line Tools 。 Mavericks （10.9） 或更高版本的系统中，在 Terminal 里尝试首次运行 git 命令即可。

~~~bash
$ git --version
~~~

下载地址
[https://git-scm.com/download/mac]

### 1.3 Windows

在 Windows 上安装 Git 也有几种安装方法。 官方版本可以在 Git 官方网站下载。 打开 [https://git-scm.com/download/win] ，下载会自动开始。 要注意这是一个名为 Git for Windows 的项目（也叫做 msysGit），和 Git 是分别独立的项目；更多信息请访问 [http://msysgit.github.io/] 。

要进行自动安装，你可以使用 Git Chocolatey 包。 注意 Chocolatey 包是由社区维护的。

另一个简单的方法是安装 GitHub Desktop 。 该安装程序包含图形化和命令行版本的 Git 。 它也能支持 Powerbash ，提供了稳定的凭证缓存和健全的换行设置。稍后我们会对这方面有更多了解，现在只要一句话就够了，这些都是你所需要的。 你可以在 GitHub for Windows 网站下载，网址为 GitHub Desktop 网站。

### 1.4 从源码安装

#### 1.4.1 编译环境

##### 1.4.1.1 dnf （如 Fedora ）

安装 Git 依赖的库： autotools 、 curl 、 zlib 、 openssl 、 expat 和 libiconv 。

~~~bash
$ sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel openssl-devel perl-devel zlib-devel
~~~

为了添加文档的多种格式（doc、html、info），需要以下附加的依赖：

~~~bash
$ sudo dnf install asciidoc xmlto docbook2X
~~~

基于 RPM 的发行版（Fedora/RHEL/RHEL衍生版），需要 getopt 包 （它已经在基于 Debian 的发行版中预装了）：

~~~bash
$ sudo dnf install getopt
~~~

此外，如果使用 Fedora/RHEL/RHEL 衍生版，需要执行以下命令：

~~~bash
$ sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi
~~~

以此来解决二进制文件名的不同。

##### 1.4.1.2 apt （如基于 Debian 的系统）

安装 Git 依赖的库： autotools 、 curl 、 zlib 、 openssl 、 expat 和 libiconv 。

~~~bash
$ sudo apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev
~~~

为了添加文档的多种格式（doc、html、info），需要以下附加的依赖：

~~~bash
$ sudo apt-get install asciidoc xmlto docbook2x
~~~

注意：使用 RHEL 和 RHEL 衍生版，如 CentOS 和 Scientific Linux 的用户需要开启 EPEL 库以便下载 docbook2X 包。

基于 Debian 的发行版（Debian/Ubuntu/Ubuntu-derivatives），需要 install-info 包：

~~~bash
$ sudo apt-get install install-info
~~~

##### 1.4.1.3 获取源码

当安装好所有的必要依赖，可以从几个地方来取得最新发布版本的 tar 包。 

(1) Kernel.org 网站，网址为 [https://www.kernel.org/pub/software/scm/git]

(2) GitHub 网站上的镜像，网址为 [https://github.com/git/git/releases]

通常在 GitHub 上的是最新版本，但 kernel.org 上包含有文件下载签名，如果你想验证下载正确性的话会用到。

#### 1.4.2 编译并安装

~~~bash
$ tar -zxf git-2.8.0.tar.gz
$ cd git-2.8.0
$ make configure
$ ./configure --prefix=/usr
$ make all doc info
$ sudo make install install-doc install-html install-info
~~~

使用 Git 来获取 Git 的更新

~~~bash
$ git clone git://git.kernel.org/pub/scm/git/git.git
~~~

## 2 Git配置

### 2.1 概述

通过以下命令查看所有的配置以及它们所在的文件：

~~~bash
$ git config --list --show-origin
~~~

### 2.2 用户信息

安装完 Git 之后，要做的第一件事就是设置用户名和邮件地址。 它们会写入到每一次提交中，不可更改：

~~~bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
~~~

其中， "John Doe" 为用户名称， johndoe@example.com 为注册邮箱。

### 2.3 文本编辑器

如果你想使用不同的文本编辑器，例如 Emacs ，可以输入如下命令：

~~~bash
$ git config --global core.editor emacs
~~~

在 Windows 系统上，如果你想要使用别的文本编辑器，那么必须指定可执行文件的完整路径。 它可能随你的编辑器的打包方式而不同。

对于 Notepad++，一个流行的代码编辑器来说，你可能想要使用 32 位的版本， 因为在本书编写时 64 位的版本尚不支持所有的插件。 如果你在使用 32 位的 Windows 系统，或在 64 位系统上使用 64 位的编辑器，那么你需要输入如下命令：

~~~bash
$ git config --global core.editor "'C:/ProgramFiles/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
~~~

### 2.4 检查配置信息

使用 git config --list 命令来列出所有 Git 当时能找到的配置。

~~~bash
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
~~~

Git 会从不同的文件中读取同一个配置（例如：/etc/gitconfig与~/.gitconfig）。 在变量名重复的情况下，Git 会使用它找到的每一个变量的最后一个配置。

通过输入 git config `<`key`>`：来检查 Git 的某一项配置

~~~bash
$ git config user.name
John Doe
~~~

查询 Git 中变量的原始值，确定哪一个配置文件最后设置了该值：

~~~bash
$ git config --show-origin rerere.autoUpdate
file:/home/johndoe/.gitconfig   false
~~~

### 2.5 获取帮助

若你使用 Git 时需要获取帮助，有三种等价的方法可以找到 Git 命令的综合手册（manpage）：

~~~bash
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
~~~

获得 git config 命令的手册，执行

~~~bash
$ git help config
~~~

在 Freenode IRC 服务器 [https://freenode.net] 上的 #git 或 #github 频道可以寻求帮助。

如果只需要可用选项的快速参考，那么可以用 -h 选项获得更简明的 “help” 输出：

~~~bash
$ git add -h
~~~

~~~markdown
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

## 3 Git仓库

