---
title: MacOS setup
date: 2022-03-27 12:54:41
categories: macos,tools
tags:
 - macos
---

- [Terminal Applications](#terminal-applications)
	- [iterm2](#iterm2)
		- [Installation <a id="installation"></a>](#installation-)
		- [Customization <a id="customization"></a>](#customization-)
			- [Colors and Font Settings <a id="colors-and-font-settings"></a>](#colors-and-font-settings-)
	- [Terminal Hot keys](#terminal-hot-keys)
	- [Zsh](#zsh)
		- [Install zsh](#install-zsh)
		- [Step2 Install plugins and  theme](#step2-install-plugins-and--theme)
		- [Install Theme](#install-theme)
		- [Re-load configuration](#re-load-configuration)
	- [VIM Hot keys](#vim-hot-keys)
- [MacOS Applications](#macos-applications)
	- [Git and GitHub <a id="git-and-github"></a>](#git-and-github-)
		- [SSH Config for GitHub <a id="ssh-config-for-github"></a>](#ssh-config-for-github-)
			- [Check for existing SSH keys <a id="check-for-existing-ssh-keys"></a>](#check-for-existing-ssh-keys-)
			- [Generate a new SSH key <a id="generate-a-new-ssh-key"></a>](#generate-a-new-ssh-key-)
	- [Homebrew](#homebrew)
		- [Installation <a id="installation"></a>](#installation--1)
	- [cask](#cask)
		- [Installation <a id="installation"></a>](#installation--2)
		- [Search <a id="search"></a>](#search-)
		- [usage](#usage)
	- [Using Homebrew](#using-homebrew)
	- [Sublime TXT](#sublime-txt)
		- [Sublime Text <a id="sublime-text"></a>](#sublime-text-)
			- [Installation <a id="installation"></a>](#installation--3)
			- [Use CLI to open file <a id="use-cli-to-open-file"></a>](#use-cli-to-open-file-)
	- [Subl plugins](#subl-plugins)
		- [Keyboard Shortcuts](#keyboard-shortcuts)
		- [SideBarEnhancements](#sidebarenhancements)
		- [Vintage Mode](#vintage-mode)
		- [Anaconda](#anaconda)
		- [SublimeLinter](#sublimelinter)
		- [GitGutter](#gitgutter)
		- [Markdown Preview](#markdown-preview)
	- [Vagrant](#vagrant)
		- [Installation <a id="installation"></a>](#installation--4)
		- [Configuration](#configuration)
		- [Usage <a id="usage"></a>](#usage-)


# Terminal Applications

## iterm2

[iTerm2](http://www.iterm2.com/) is an open source replacement for Apple's Terminal. It's highly customizable and comes with a lot of useful features.

### Installation <a id="installation"></a>

Use [Homebrew](http://sourabhbajaj.com/mac-setup/Homebrew/) to download and install:

```text
brew cask install iterm2
```

### Customization <a id="customization"></a>

#### Colors and Font Settings <a id="colors-and-font-settings"></a>

Here are some suggested settings you can change or set, **they are all optional**.

* Set hot-key to open and close the terminal to `command + option + i`
* Go to profiles -&gt; Default -&gt; Terminal -&gt; Check silence bell to disable the terminal session from making any sound
* Download [one of iTerm2 color schemes](https://github.com/mbadolato/iTerm2-Color-Schemes/tree/master/schemes) and then set these to your default profile colors
* Change the cursor text and cursor color to yellow make it more visible
* Change the font to 14pt Source Code Pro Lite. Source Code Pro can be downloaded using [Homebrew](https://sourabhbajaj.com/mac-setup/Homebrew/) `brew tap caskroom/fonts && brew cask install font-source-code-pro`
* If you're using BASH instead of ZSH you can add `export CLICOLOR=1` line to your `~/.bash_profile`file for nice coloring of listings


## Terminal Hot keys



```
移动：

ctrl + A  行首
ctrl + E  行尾
Ctrl + B 相当于左箭头键，用于将光标向左移动一格
Ctrl + F 相当于右箭头键，用于将光标向右移动一格

删除：
ctrl + W  删除左侧一个单词
ctrl + U  删除光标前 所有字符
ctrl + K  删除光标后 所有字符
Ctrl + H 删除光标左侧的一个字符
ctrl + D 删除光标后一个字符

其他:

Ctrl + P 相当于上箭头键，即显示上一个命令
Ctrl + N 相当于下箭头键，即显示下一个命令
ctrl + R   搜索旧命令

ctrl + l  clear 清屏
ctrl + n  开一个新tab的terminal
ctrl + t  同一tab新开一个terminal
```

## Zsh

### Install zsh

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Step2 Install plugins and  theme

```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

```
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

```
vim ~/.zshrc

plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

### Install Theme

```
git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```

```
#Set ZSH_THEME=powerlevel10k/powerlevel10k in your ~/.zshrc.

ZSH_THEME="powerlevel10k/powerlevel10k"
```

### Re-load configuration

```
source ~/.zshrc
```


## VIM Hot keys



VIM是一个非常强大的工具及利器，这里只是列出日常工作中最常用的快捷方式

> #### **移动**
>
> ```text
> nw  n为数字  光标向后N个单词
> nb  光标向前N个单词
>
> gg   文首
>
> G    文尾
>
> 0  #绝对行首
> ^  #行首非第一个非字符
> $  #绝对行
>
>
> h或退格: 左移一个字符；
> l或空格: 右移一个字符；
> j: 下移一行；
> k: 上移一行
>
> ctrl+d: 下翻半屏。
> ctrl+u: 上翻半屏。
> ctrl+e: 向下滚动一行。
> ctrl+y: 向上滚动一行。
>
>
> nw  n为数字  光标向后N个单词
> nb  光标向前N个单词
>
> ```
>
> #### 编辑
>
> ```text
> a  #光标后字符插入
> A  #行尾插入
>
> i  #光标位置插入
> I   #光标当前行 行首插入
>
> u/U #撤销操作
> ```
>
> #### 位置
>
> ```text
> 0  #绝对行首
> ^  #行首非第一个非字符
> $  #绝对行尾
> ```
>
> #### 删除
>
> ```text
> x        删除当前光标下的字符
> dw       删除光标之后的单词剩余部分。
> d$       删除光标之后的该行剩余部分。
> dd       删除当前行。
>
> cw     删除光标后一个单词，进入instert mode
> c$
> c        功能和d相同，区别在于完成删除操作后进入INSERT MODE
> cc       也是删除当前行，然后进入INSERT MODE
> ```
>
> #### 其他
>
> ```text
> ：set nu   #显示行数
> v   #选择行
> ctrl + V  #选择列
> ```
>
> #### 如何comment多行或者un-comment多行
>
> ```text
> ctrl + V 进入visual block模式
> shift + i 进入插入模式
> 输入#
> Esc退出插入模式
> ```
>
> #### 搜索/替代
>
> ```text
> /searchword
>
> %s/oldword/newword/g
> ```

# MacOS Applications

## Git and GitHub <a id="git-and-github"></a>

What's a developer without [Git](http://git-scm.com/)? To install, run:

```text
$ brew install git
```

When done, to test that it installed properly you can run:

```text
$ git --version
```

And `which git` should output `/usr/local/bin/git`.

Next, we'll define your Git user \(should be the same name and email you use for [GitHub](https://github.com/)\):

```text
$ git config --global user.name "Your Name Here"
$ git config --global user.email "your_email@youremail.com"
```

They will get added to your `.gitconfig` file.

To push code to your GitHub repositories, we're going to use the recommended HTTPS method \(versus SSH\). To prevent `git` from asking for your username and password every time you push a commit you can cache your credentials by running the following command, as described in the [instructions](https://help.github.com/articles/caching-your-github-password-in-git/).

```text
$ git config --global credential.helper osxkeychain
```

### SSH Config for GitHub <a id="ssh-config-for-github"></a>

The instructions below are referenced from [the official documentation](https://help.github.com/articles/generating-ssh-keys).

#### Check for existing SSH keys <a id="check-for-existing-ssh-keys"></a>

First, we need to check for existing SSH keys on your computer. We do this by running:

```text
$ ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist
```

Check the directory listing to see if you have files named either `id_rsa.pub` or `id_dsa.pub`. If you don't have either of those files then read on, otherwise skip the next section.

#### Generate a new SSH key <a id="generate-a-new-ssh-key"></a>

If you don't have an SSH key you need to generate one. To do that you need to run the commands below, and make sure to substitute the placeholder with your email. The default settings are preferred, so when you're asked to "enter a file in which to save the key,"" just press Enter to continue.

```text
$ ssh-keygen -t rsa -C "your_email@example.com"
# Creates a new ssh key, using the provided email as a label
```


## Homebrew

[Homebrew](https://brew.sh/) calls itself _The missing package manager for macOS_ and is an essential tool for any developer.

### Installation <a id="installation"></a>

Before you can run Homebrew you need to have the **Command Line Tools** for **Xcode** installed. It include compilers that will allow you to build things from source, and if you are missing this it's available through the App Store &gt; Updates.

To install Homebrew run the following: terminal, hit **Enter**, and follow the steps on the screen:

```text
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

One thing you need to do is tell the system to use programs installed by Hombrew \(in `/usr/local/bin`\) rather than the OS default if it exists. You do this by adding `/usr/local/bin` to your `PATH` environment variable \(if you're using `zsh` you should use `.zshrc` instead of `.bash_profile`\):

```text
$ echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile
```

Alternatively, you can also insert `/usr/local/bin` to the first line of `/private/etc/paths` and reboot the Mac to change global paths loading order. Admin password may be required if you modify the file.

To be able to use `brew` you need to start a new terminal session. After that you should make sure everything is working by running:

```text
$ brew doctor
```



## cask



[Homebrew-Cask](https://caskroom.github.io/) extends Homebrew and allows you to install large binary files via a command-line tool. You can for example install applications like Google Chrome, Dropbox, VLC and Spectacle. No more downloading `.dmg` files and dragging them to your Applications folder!

### Installation <a id="installation"></a>

You need Homebrew on your system to use Cask, and you make Cask available by adding it as a tap:

```text
$ brew tap caskroom/cask
```

### Search <a id="search"></a>

To see if an app is available on Cask you can search on the [official Cask website](https://caskroom.github.io/). You can also search in your terminal:

```text
$ brew cask search <package>
```

### usage

"To install, drag this icon..." no more. `brew cask` installs macOS apps, fonts and plugins and other non-open source software.

```text
$ brew cask install firefox
```


## Using Homebrew



To install a package \(or **Formula** in Homebrew vocabulary\) simply type:

```text
$ brew install <formula>
```

To update Homebrew's directory of formulae, run:

```text
$ brew update
```

**Note**: If that command fails you can manually download the directory of formulas like this:

```text
$ cd /usr/local/Homebrew/
$ git fetch origin
$ git reset --hard origin/master
```

To see if any of your formulas need to be updated:

```text
$ brew outdated
```

To update a formula:

```text
$ brew upgrade <formula>
```

Homebrew keeps older versions of formulas installed on your system, in case you want to roll back to an older version. That rarely is necessary, so you can do some cleanup to get rid of those old versions:

```text
$ brew cleanup
```

If you want to see what formulas Homebrew would delete _without actually deleting them_, you can run:

```text
$ brew cleanup --dry-run
```

To see what you have installed \(with their version numbers\):

```text
$ brew list --versions
```

To search for formulas you run:

```text
$ brew search <formula>
```

To get more information about a formula you run:

```text
$ brew info <formula>
```

To uninstall a formula you can run:

```text
$ brew uninstall <formula>
```



## Sublime TXT

### Sublime Text <a id="sublime-text"></a>

[Sublime Text](http://www.sublimetext.com/) is a widely used editor that describes it self as _a sophisticated text editor for code, markup and prose_.

#### Installation <a id="installation"></a>

[Download](http://www.sublimetext.com/) the **.dmg** file and drag-and-drop it to the **Applications** folder.

#### Use CLI to open file <a id="use-cli-to-open-file"></a>

Let's create a shortcut so we can launch Sublime Text from the command-line

```text
$ ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl
```

Now you can open a file with `$ subl myfile.py` or start a new project in the current directory with `$ subl .`.

## Subl plugins

\*\*\*\*

### Keyboard Shortcuts

* **Goto Anything** Cmd+P is used for quickly finding and opening files. Just type in a part of a path and filename within a project and you can easily open that file. This is great for quickly opening files in large Django projects.
* **Goto Line Number** Ctrl+G takes you to a specific line number in an active file.
* **command + shift + p**  to open install package -&gt; search packages 

### SideBarEnhancements

[SideBarEnhancements](https://sublime.wbond.net/packages/SideBarEnhancements) extends the number of menu options in the sidebar, speeding up your overall workflow. Options such as **New File** and **Duplicate** are essential and should be part of ST3 out of the box. The **Delete** option alone makes it worth downloading. This feature simply sends files to the Trash, which may seem trivial, but if you delete a file without it, then it’s very difficult to recover unless you’re using a version control system

![](../.gitbook/assets/image.png)

### Vintage Mode

enable vim mode on sublime

### Anaconda

[Anaconda](https://sublime.wbond.net/packages/Anaconda) is the ultimate Python package. It adds a number of IDE-like features to ST3 including the following:

* **Autocompletion** works by default, but there are a number of configuration [options](https://github.com/DamnWidget/anaconda#anaconda-autocompletion).
* **Code** [**linting**](http://en.wikipedia.org/wiki/Lint_%28software%29) uses either PyLint or PyFlakes with PEP 8. I personally use a different linting package, as I will explain shortly, so I disable linting altogether within the user-defined Anaconda settings file, **Anaconda.sublime-settings**, via the file menu: **Sublime &gt; Preferences &gt; Package Settings &gt; Anaconda &gt; Settings - User**: `{"anaconda_linting": false}`
* **McCabe code complexity checker** runs the [McCabe complexity checker](http://en.wikipedia.org/wiki/Cyclomatic_complexity) tool within a specific file. If you’re not familiar with what complexity is, be sure to visit the link above.
* **Goto Definitions** finds and displays the definition of any variable, function, or class throughout your entire project.
* **Find Usage** quickly searches where a variable, function, or class has been used in a specific file.
* **Show Documentation** shows the docstring for functions or classes \(if defined, of course\).

### SublimeLinter

[SublimeLinter](https://sublime.wbond.net/packages/SublimeLinter) is a framework for ST3 linters. The package itself does not include any actual linters; those must be installed separately via Package Control using the **SublimeLinter-\[linter\_name\]** naming syntax. You can view official linters [here](https://github.com/SublimeLinter). There are also a number of third party linters, which can be viewed in Package Control. Check out the installation instructions [here](http://sublimelinter.readthedocs.org/en/latest/installation.html).

### GitGutter

[GitGutter](https://sublime.wbond.net/packages/GitGutter) shows little icons in ST3’s gutter area that indicate whether a line has been inserted, modified, or deleted since the last commit.

### Markdown Preview

[Markdown Preview](https://sublime.wbond.net/packages/Markdown%20Preview) is used for previewing and building markdown files.

To use, open the Package Manager and type `Markdown Preview` to show the available commands:

* Markdown Preview: Python Markdown: Preview in Browser
* Markdown Preview: Python Markdown: Export HTML in Sublime Text
* Markdown Preview: Python Markdown: Copy to Clipboard
* Markdown Preview: GitHub Flavored Markdown: Preview in Browser
* Markdown Preview: GitHub Flavored Markdown: Export HTML in Sublime Text
* Markdown Preview: GitHub Flavored Markdown: Copy to Clipboard
* Markdown Preview: Open Markdown Cheat Sheet


## Vagrant

Create and configure lightweight, reproducible, and portable development environments. [Vagrant](http://www.vagrantup.com/) is a tool for managing virtual machines via a simple to use command line interface.

### Installation <a id="installation"></a>

Vagrant uses [Virtualbox](https://www.virtualbox.org/) to manage the virtual dependencies. You can [directly download virtualbox](https://www.virtualbox.org/wiki/Downloads) and install or use Homebrew for it. Notice that macOS High Sierra 10.13 introduces a new feature that requires user approval before loading new third-party kernel extensions. In case of failure follow the instructions [here](https://developer.apple.com/library/archive/technotes/tn2459/_index.html).

```text
$ brew cask install virtualbox
```

Now install Vagrant either [from the website](http://www.vagrantup.com/downloads.html) or use Homebrew for installing it.

```text
$ brew cask install vagrant
```

[Vagrant-Manager](http://vagrantmanager.com/) helps you manage all your virtual machines in one place directly from the menu bar.

```text
$ brew cask install vagrant-manager
```

### Configuration

configuration file:

 `~/vagrant/Vagrantfile`

### Usage <a id="usage"></a>

Add the Vagrant box you want to use. We'll use Ubuntu 12.04 for the following example.

```text
$ vagrant box add precise64 https://vagrantcloud.com/hashicorp/boxes/precise64/versions/1.1.0/providers/virtualbox.box
```

You can find more boxes at [Vagrant Cloud](https://app.vagrantup.com/boxes/search).

Now create a test directory and `cd` into the test directory. Then we'll initialize the vagrant machine.

```text
$ vagrant init precise64
```

Now lets start the machine using the following command.

```text
$ vagrant up
```

You can ssh into the machine now.

```text
$ vagrant ssh
```

Halt the vagrant machine now.

```text
$ vagrant halt
```

Other useful commands are `suspend` and `destroy`.





