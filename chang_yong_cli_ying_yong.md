# 常用 CLI 应用

##HomeBrew & HomeBrew Cask

Homebrew 是 Mac 下最好用的包管理器，推荐所有控制台应用都尽可能使用 brew 管理。

##安装 HomeBrew：

ruby -e “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)”
Homebrew Cask 是 Homebrew 的插件，作为 Homebrew 在 GUI 应用上的补充。

##安装 HomeBrew Cask：

brew install caskroom/cask/brew-cask
##zsh & oh-my-zsh!

被称为终极 shell 也没什么问题。Mac 已经预装了 zsh ，只需要安装 oh-my-zsh! 即可：

curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
或者：

wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O - | sh
Ubuntu 中需要先注册 zsh 可用：

command -v zsh | sudo tee -a /etc/shells
chsh -s "$(command -v zsh)" "${USER}"
切换 sh 之后可能需要重新登录才能生效。

##tree

显示目录的树状图

##htop

进程管理器，比自带的 top 要强大许多。

##autojump

在常用目录间快速跳转，命令是 j。

##IPython

提供更强大的 Python REPL 环境，配合 IPython Notebook 能提供强大的 web 执行环境。

##ag

文件查找命令，类似于 ack。

##ncdu

磁盘空间占用分析。

##dos2unix

换行符转换工具（Windows下换行符是 \r\n，OS X 是 \n）

##coreutils & advcopy

Unix 命令增强。手动安装即可提供基本功能，不过为了安装 advcopy 我还是下载了源代码， 打了补丁之后编译安装了一遍：

wget http://ftp.gnu.org/gnu/coreutils/coreutils-8.23.tar.xz
tar xvJf coreutils-8.23.tar.xz
cd coreutils-8.23/
wget https://raw.githubusercontent.com/felixonmars/aur-mirror/master/advcopy/advcpmv-0.5-8.23.patch
patch -p1 -i advcpmv-0.5-8.23.patch
./configure –-without-gmp –-program-prefix g $(brew diy)
make && make install
brew link coreutils
如果 ./configure ... 那条命令执行失败，可以去掉参数重新编译，然后手动添加软链接：

ln -s src/cp /usr/local/bin/
alias cp="cp -gR"
这样就可以将编译出来的可执行文件链接到应用程序目录。

##mpv-player

brew tap mpv-player/mpv
brew install mpv
##git

最流行的分布式代码管理工具。Mac 自带了 git ，只需要做一下简单的配置：

git config --global user.name "Kane Blueriver"
git config --global user.email "kxxoling@gmail.com"
git config --global credential.helper osxkeychain     # 设置 Mac KeyChain 工具来自动提交 GitHub 验证信息
git 教程：小猫都能学会的 git 教程

常用的 git 插件有：

zsh git 插件
git-flow（AVH Edition brew install git-flow-avh）
legit。legit 主要提供 git-flow 的命令封装。
PS：zsh git 插件在较大的项目中会导致启动缓慢，建议在大型项目中关闭该插件。

##pip & gem & npm & cnpm

pip 以外的所有 Python 应用都应该使用 pip 来安装！

gem 是 Ruby 的包管理器，安装 compass / puppet / vagrant 等 Ruby 应用时离不开 gem。

npm 是 node.js 包管理器，cnpm 是 淘宝提供的 npm 替代品。

brew install npm
npm install cnpm -g --registry=https://registry.npm.taobao.org         # 安装 cnpm 来替代 npm 命令
##pgcli

pgcli 是控制台中的 PostgreSQL 管理工具，功能强大！ 推荐使用 brew 安装：

brew install pgcli
不过，由于 pgcli 是 Python 应用，你也可以使用 pip 或者 easy_install 安装：

pip install pgcli
##uwsgi & gunicorn

提供 WSGI 支持的 web 服务器。

##sphinx

Python 社区的文档管理标准，著名的文档托管服务 ReadTheDocs 服务就是基于其搭建， 支持 i18n，配合 Python 注释自动生成非常方便。

##LiveReload

LiveReload 是使用 Python 开发的自动刷新工具， 可以配合 livereload Chrome 插件使用。

文档：en

##virtualenv

提供 Python 虚拟环境的隔离。

VirtualEnvWrapper

virtualenv 命令复杂，目录的管理也比较混乱，VirtualEnvWrapper 是在这之上的一层封装。

##tmux

控制台中的标签页管理工具以及分屏管理工具。

##Vim

目前最常用的编辑器是 Vim，配置主要来自胡淼的 dot-vim， 也有一些自己的特别配置。安装：

mv ~/.vim ~/.vim.orig
mv ~/.vimrc ~/.vimrc.orig
git clone git://github.com/humiaozuzu/dot-vimrc.git ~/.vim
ln -s ~/.vim/vimrc ~/.vimrc
git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
vim +BundleClean +BundleInstall +qall
##aria2

aira2 是 OS X 和 Linux 下通用的下载工具，可以安装在路由器、NAS 等 Linux 机器上。

配合 YAAW 或者 webui-arias2 非常方便。

附：

aria2 配置示例
YAAW
Aria2 & YAAW 使用说明