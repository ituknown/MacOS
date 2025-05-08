# 前言

`HomeBrew` 是 Mac 下最常用的包管理器工具，类似于 CentOS 的 `yum` ，用起来很是方便。该包管理器可以说是程序员的专属利器，安装 IDE？应有尽有！

HomeBrew 官网是：[https://brew.sh](https://brew.sh)，另外，Github 仓库地址是：[https://github.com/Homebrew/brew](https://github.com/Homebrew/brew)。除此之外，官网还提供了一个使用手册：[https://docs.brew.sh](https://docs.brew.sh)。所以，有什么问题直接浏览着这三个网页即可寻找答案。

本文记录的是在使用过程中遇到的问题以及注意事项。

# 安装 Brew

在 [HomeBrew官网](https://brew.sh) 中提供了 brew 的安装命令：

```bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

或者执行如下命令：

```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

**注意：** 直接在命令终端运行该命令的话之后的所有的软件包默认都安装在 `/usr/local` 目录下，当然也是推荐的安装方式。如果想要改变之后软件包的安装目录可以在安装之后在环境变量中设置 `HOMEBREW_PREFIX` 来进行改变之后软件包的安装目录，示例：

```bash
export HOMEBREW_PREFIX = $(YouPath)
```

安装命令执行完成后可以执行如下命令验证是否安装成功：

```bash
$ brew help
```

# 基本使用

## 检查系统环境

在 brew 安装成功后，你可以使用 `doctor` 命令来检查你的系统环境：

```bash
$ brew doctor
```

如果你的系统存在潜在问题，就会列出警告信息。示例：

```
Please note that these warnings are just used to help the Homebrew maintainers
with debugging if you file an issue. If everything you use Homebrew for is
working fine: please don't worry or file an issue; just ignore this. Thanks!

Warning: A newer Command Line Tools release is available.
Update them from Software Update in System Preferences or
https://developer.apple.com/download/more/.


Warning: Unbrewed header files were found in /usr/local/include.
If you didn't put them there on purpose they could cause problems when
building Homebrew formulae, and may need to be deleted.

...
```

## 安装软件包

HomeBrew 提供了丰富的软件包，如果你想要安装软件包只需要执行如下命令即可：

```bash
$ brew install <package_name>
```

另外，如果你不知道或记不清你想要安装的软件包你也可以通过 `search` 命令进行查找软件包。

## 查找软件包

比如，我想要安装 `wget` 软件包，但是我又记不得全名称。这样，我就可以利用 `search` 命令进行搜索：

```bash
$ brew search wge

==> Formulae
brew-gem         liblwgeom        pwgen            sf-pwgen         wget ✔           wgetpaste
```

在 Formulae 信息中，就会列出所有符合条件的软件包。注意看 `wget` ，在其后有一个 `✔` 标识。标识该软件包是安装频率最高的软件包。

## 卸载软件包

卸载软件包很简单，仅仅需要执行如下命令即可：

```bash
$ brew uninstall <package_name>
```

## 列出安装的软件列表

时间久了，就会记不清在系统中使用 brew 到底安装了多少软件包。那我们就可以使用 `list` 命令列出所有安装的软件包：

```bash
$ brew list
```

## 查看 brew 配置

该信息很重要，因为你所有与 brew 有关的信息你都可以查看。命令是： `config` ：

```bash
$ brew config
```

下面是输出示例：

```
HOMEBREW_VERSION: 2.2.11
ORIGIN: https://github.com/Homebrew/brew
HEAD: fa8fe0fc396550c23156043cd07550fc6ae6a27a
Last commit: 13 days ago
Core tap ORIGIN: https://github.com/Homebrew/homebrew-core
Core tap HEAD: 6949fc6f36de1261dffbe37f3b2216cf903431ed
Core tap last commit: 3 hours ago
HOMEBREW_PREFIX: /usr/local
HOMEBREW_NO_AUTO_UPDATE: true
CPU: octa-core 64-bit kabylake
Homebrew Ruby: 2.6.3 => /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/bin/ruby
Clang: 11.0 build 1100
Git: 2.19.2 => /usr/local/bin/git
Curl: 7.64.1 => /usr/bin/curl
Java: 1.8.0_181, 1.7.0_80
macOS: 10.15-x86_64
CLT: 11.0.0.0.1.1567737322
Xcode: N/A
```

在输出的信息中列出了 `brew` 的默认安装目录： `HOMEBREW_PREFIX` 。所以，如果你想要修改该目录只需要在配置环境中设置一个环境变量即可。示例：

```bash
export HOMEBREW_PREFIX=/opt/local
```

## 禁止自动更新

你有没有发现，在使用中如果想要安装软件包就会更待很长一段时间，就如同卡顿一样。其根本原因是要检查 `brew` 是否需要更新，如果官网提供了一个新的版本就会自动请求更新，只有更新之后才会进行安装软件包。所以，这个感觉到这个设置很影响客户体验有木有😇😇😇😇。

**所以，我们需要将该设置取消掉。**

同样的，`brew` 有一个环境配置： `HOMEBREW_NO_AUTO_UPDATE` 。该配置用于是否取消自动更新。我们只需要在环境变量中设置其值为 `true` 即可：

```bash
export HOMEBREW_NO_AUTO_UPDATE=true
```

如果是在配置文件中修改的记得使其生效，可以执行命令： `source <YouConfig>` 。

现在再次执行 `config` 命令查看配置信息：

```bash
$ brew config

HOMEBREW_VERSION: 2.2.11
ORIGIN: https://github.com/Homebrew/brew
HEAD: fa8fe0fc396550c23156043cd07550fc6ae6a27a
Last commit: 13 days ago
Core tap ORIGIN: https://github.com/Homebrew/homebrew-core
Core tap HEAD: 6949fc6f36de1261dffbe37f3b2216cf903431ed
Core tap last commit: 3 hours ago
HOMEBREW_PREFIX: /usr/local
HOMEBREW_NO_AUTO_UPDATE: true
CPU: octa-core 64-bit kabylake
Homebrew Ruby: 2.6.3 => /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/bin/ruby
Clang: 11.0 build 1100
Git: 2.19.2 => /usr/local/bin/git
Curl: 7.64.1 => /usr/bin/curl
Java: 1.8.0_181, 1.7.0_80
macOS: 10.15-x86_64
CLT: 11.0.0.0.1.1567737322
Xcode: N/A
```

可以看到在输出的信息中多了 `HOMEBREW_NO_AUTO_UPDATE: true` ，表示其已生效。不信？你可以测试安装一个软件包试试！

## 禁止清除缓存

默认情况下，HomeBrew 每次安装新软件或者执行更新时就会自动执行 `brew cleanup` 命令进行清除废旧和过时的软件包。但是如果你特别变态不想去让他自动执行 `cleanup` 命令清除这些缓存，那你就可以设置 `HOMEBREW_NO_INSTALL_CLEANUP` 来进行它清除：

```bash
export HOMEBREW_NO_INSTALL_CLEANUP=true
```

| **注意** |
| :--- |
| 如果你设置了该配置，就表示之后 brew 不在自动清除废旧、过时的软件包。长时间会在系统磁盘中占用大量的空间。

所以，你需要不定时的手动执行 bren cleanup 命令进行清除！ |

## 手动更新

`brew` 安装的软件包我们也是要时常更新的，下面的命令是更新、升级以及清楚缓存命令，检查时常去运行下：

```bash
 brew update -v && brew upgrade -v && brew autoremove -v && brew cleanup -v
```

不过我更喜欢将上面的命令使用 alias 取个别名写到配置文件中：

```bash
alias brewu='brew update -v && brew upgrade -v && brew autoremove -v && brew cleanup -v'
```

这样以后只需要简单的执行如下命令就可以进行升级更新了：

```bash
$ brewu
```

# 清华源设置

默认情况下， `brew` 使用的是默认的 `Github` 源。所以在使用中不管是下载还是更新软件都特别慢，有时甚至卡老半天。

不过呢，国内也有相应的镜像源，用的最多的可能就是清华源和阿里云镜像了。配置这些源之后可以大大提高使用效率。怎么来形容配置源和没有配置源呢🤔？嗯，看下下面的比较：


用原有的镜像下载非常慢 => 🚶
替换源，更新速度变成    => 🚀


另外要说的一点是， `brew` 本身也可以按照 GUI 软件，比如开发者工具 IDE 等，不过需要按照 `brew-cask`，执行如下命令即可安装（根据需要安装）：

```bash
$ brew cask
```

现在我们就以清华源为例来更改本地源！

清华源地址是：[https://mirrors.tuna.tsinghua.edu.cn/](https://mirrors.tuna.tsinghua.edu.cn/)，你可以直接在搜索中进行搜索 `homebrew` 即可找到相应的源：

<div style="text-align: left;">
  <img src="https://ituknown.org/macos-media/homebrew/images-search.png" alt="images-search.png" width="650" />
</div>

我们需要更新这些源，其中 `homebrew-bottles` 与我们安装软件有关，所以推荐也进行设置。点击 `homebrew` 即可进入说明页面，在说明页面中也有说明如何使用，只需要按照文档说明执行命令即可：

<div style="text-align: left;">
  <img src="https://ituknown.org/macos-media/homebrew/use-helper.png" alt="use-helper.png" width="650" />
</div>

所以，笔者这里就替换 `brew` 、 `brew-core` 以及 `brew-cask` ，直接执行如下命令即可：

```bash
git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git
brew update
```

| **注意** |
| :--- |
| 如何你没有安装 `brew-cask` ，第三条命令就不需要执行了 |

等待命令执行完成后执行 `brew config` 即可查看配置的源信息，示例：

<div style="text-align: left;">
  <img src="https://ituknown.org/macos-media/homebrew/brew_config.png" alt="brew_config.png" width="650" />
</div>

现在就来更换 `homebrew-bottles` ，直接点击然后拷贝 URL 链接即可，链接如下：

```
https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles
```

替换  `bottles` 与我们当前使用的 `shell` 有关，所以需要执行如下命令进行查看：

```bash
$ echo $SHELL
```

根据版本不同，会输出2种结果：`/bin/zsh` 或 `/bin/bash`。

如果想要临时替换，直接在命令行中执行如下命令即可：

```bash
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/
```

如何想要永久替换则根据自己的 Shell 版本进行选择：

## `/bin/zsh` 版本

直接在命令行中执行如下命令即可：

```bash
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/' >> ~/.zshrc

source ~/.zshrc
```

## `/bin/bash` 版本


直接在命令行中执行如下命令即可：

```bash
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/' >> ~/.bash_profile

source ~/.bash_profile
```

# FAQ

`brew` 很好用，但是在使用过程中也少不了遇到各种问题。在这里就列举遇到的问题：
## `chown: /usr/local: Operation not permitted` 

出现该问题的通常都是 `Mac OS X 10.11` 及之后的版本吗，原因是 HomeBrew 默认指定的系统目录是 `/usr/local` 。而在 `Mac OS X 10.11` 之后的版本，对 `/usr/local` 等系统目录文件的读写是需要系统 `root` 全限的，以往的H omebrew 安装如果没有指定安装路径，会默认安装在这些需要系统 `root` 用户读写权限的目录下，导致有些指令需要添加 `sudo` 前缀来执行，比如升级 Homebrew 需要执行如下命令：

```bash
$ sudo brew update
```

所以，如果你不想每次都执行 `sudo` 命令那么就需要修改用于对 `/usr/local` 目录的全限，执行如下命令：

```bash
$ sudo chown -R $(whoami) /usr/local/*
```

其中 `$(whoami)` 表示的是当前系统登录用户，你可以直接在命令终端中执行 `whoami` 命令查看登录的用户名称，示例：

```bash
$ whoami
mingrn
```

得到用于名之后，你也可以将上面的命令替换为：

```bash
$ sudo chown -R mingrn /usr/local/*
```

这两个命令是等效的，不过更建议直接使用 `$(whoami)` ，因为可以防止出错！
