---
layout: post
title: Mac环境下将Sublime Text 3 作为Ruby on Rails 编辑器的安装及配置
date: 2019-09-03 00:00:00
author: "王振华"
tags:
    - Mac
    - Sublime
    - Ruby on Rails
---


## 背景介绍

我并没有什么背景，有的只是背影。。。

做Rails开发已经两年多了，一直在用 [Atom 编辑器](https://atom.io/) ，觉得也挺好用。来到新公司，大部分同事用的都是 Sublime，为了和大家更好地协作，我也就"抱着试试看的态度，试用了一个疗程"的 Sublime 。

## 使用体验

用了两周的时间，"感觉腰也不酸了，腿也不疼了"，代码质量都有所提高。

*   快，特别快，贼快：几乎没有任何卡顿，尤其是跳转到函数的定义处时，从Atom过来的我甚至觉得有点太快了。

*   修改配置的方式更适合程序员：之前我一直决定Atom的一大好处是安装插件时是可视化界面，操作更简单。但现在觉得全部用JSON来修改配置反而更清晰。

*   对Markdown的支持不如 Atom 好，也有可能是我没找到更好的插件吧。需要装多个插件，使用起来也比较麻烦。

下面我就把我安装、配置 Sublime 过程中遇到的坑、趟过的雷分享一下。

## 安装Sublime Text 3(ST3)

找到 [Sublime 的官网](http://www.sublimetext.com/)，点击右上角的 Download ，选择你的环境对应的版本下载即可。如果想安装 Sublime Text 2 可以去这里 [Sublime Text 2](http://www.sublimetext.com/2) 。

## 安装 Package Control

Package Control 是 Sublime的插件管理工具，安装了它，就可以很方便地安装各种插件了。

你Google一下，很多文章都推荐输入以下命令来按照 Package Control ：`import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())`

但我反复尝试后还是报错，最终也没搞清楚什么原因：可能是我的电脑上的Python安装的有问题吧；但也可能是Package Control 的网站无法访问导致的。

最终我选择了离线安装的方式：[Package Control 离线包的下载地址](Package%20Control%20%E7%A6%BB%E7%BA%BF%E5%8C%85%E7%9A%84%E4%B8%8B%E8%BD%BD%E5%9C%B0%E5%9D%80)， 点击 `Clone or download` 后，点击 `Download ZIP` 。

*   把下载好的zip包解压，重命名为 `Package Control` （**注意：两个单词的首字母都要大写**）。

*   打开 ST3，Preferences — Browse Packages，把 `Package Control`文件夹复制到该目录下。

*   重启 ST3，如果 Preferences有Package Settings 和 Package Control 这两项，就说明 Package Control 安装成功了。

但重启后会有以下提示：`It appears you have Package Control installed as both a .sublime-package file and a directory inside of the Packages folder.`  解决方法是：Preferences — Browse Packages，此时会进入 Package Control 目录，点击 Finder 左上角的几种文件查看方式中的第三个，找到 Sublime Text 3 下的Installed Packages文件夹，删除Installed Packages下的文件 Package Control.sublime-package 即可。

## 安装插件的方法

我们已经安装了 Package Control，正常情况下，`Command + Shift + P`，在出现的框中输入install，选中`Package Control: Install Package` 就会进入安装插件模式。在搜索框中输入你要安装的插件的名字，选中后回车就会安装。安装插件的过程中，Sublime左下角会有个 `=` 左右摇晃。

但我选择 Install Package 之后，不出现搜索框，且报错：
```
Package Control  
There are no packages available for installation  
Please see https://packagecontrol.io/docs/troubleshooting for help</pre>
```
报错原因是ST3读取插件列表的接口地址发生了变化，而天朝访问不了这个地址。

解决方法是下载[https://raw.githubusercontent.com/SuCicada/channel_v3.json/master/channel_v3.json](https://raw.githubusercontent.com/SuCicada/channel_v3.json/master/channel_v3.json)这个文件放到本地电脑某个路径下，在 Preferences—Package Setting—Package Control —Setting User 中添加下列配置，写明这个文件在你电脑中的路径：
```
"channels":
[
"/Users/huazai/Documents/channel_v3.json"
],</pre>
```
此时，就可以安装各种插件了。问题来了，该安装哪些插件呢？

## 我已安装的插件

*   **All Autocomplete** ST3本身也有自动补全功能，但这个插件能大大加强这一功能。

*   **SideBarEnhancements** 增强侧边栏的功能

*   **Origami** 增强分屏功能。 [Origami](%20https://github.com/SublimeText/Origami)

*   **markdown**功能：安装 **Markdown Editing** 插件后，新建一个Markdown文件，点击右下角的文件格式提示，可以在弹出的菜单的Markdown Editing中选择使用哪种Markdown语法，一般用MultiMarkdown即可。再安装**Markdown Preview** 插件，然后进行如下操作 `command+shift+p — preview in browser — markdown / github`，即可在浏览器中预览Markdown 文件。

*   **rubocop 和 SublimeLinter** 安装后，编辑器就会按照 [ruby-style-guide](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md) 的标准检查你的代码，对不符合标准的代码给出提示。这一功能来自于 [rubocop](https://github.com/rubocop-hq/rubocop) ， 首先执行 `gem install rubocop` 来安装这个gem。然后在ST3 中安装三个插件来实现rubocop在后台的自动运行： [SublimeLinter](https://packagecontrol.io/packages/SublimeLinter) 、[SublimeLinter-Ruby](https://packagecontrol.io/packages/SublimeLinter-ruby) 和 [SublimeLinter-rubocop](https://packagecontrol.io/packages/SublimeLinter-rubocop) 。

    SublimeLinter自动检查代码的默认时间为0.1s，这太快了，你刚敲进去半句代码，它就会提示错误。自定义设置rubocop的响应时间的方法是，去ST3—Preferences—Package Settings—SublimeLinter—Settings，在打开的SublimeLinter.sublime-settings 文件中，添加如下设置，就会将其响应时间设为1s ：
```
{
     "delay": 1,
}
```

*   **CTags**  [CTags](https://github.com/SublimeText/CTags) 会为全部代码建立索引，增强类、函数等的跳转功能。

    首先在ST3中安装 CTags 插件，然后在终端执行 `brew install ctags` 。

    如果只想建立你的项目中所定义的函数的索引，只需在终端中进入你的项目目录，执行`ctags -R -f .tags`，就会创建一个`.tags`文件，将这个项目中需要建立的索引都写进去。

    因为Rails项目中要用到大量的gem，如果想同时建立所有用到的gem的索引，可以在你的 $PATH目录下创建一个名为ctags_for_ruby 的脚本文件，并执行 `chmod +x ctags_for_ruby` 命令将其设置为可执行文件。在在文件中写入以下内容：
```
#!/usr/bin/env ruby
    system "find . -name '*.rb' | ctags -f .tags -L -"
    ​
    if File.exist? './Gemfile'
      require 'bundler'
      paths = Bundler.load.specs.map(&:full_gem_path).join(' ')
      system "ctags -R -f .gemtags #{paths}"
    end
```

执行`ctags_for_ruby` 命令以执行以上脚本，既会为项目中的函数建立一个.tags 索引文件，又会单独在源码根目录下为所有gem建立一个 .gemtags 索引文件。

这时把光标移动到项目中的函数名上，右键`Goto Definition`即可跳转到函数定义处，右键`Jump Back`即可跳回。

我们肯定不希望 .tags 和 .gemtags 文件的内容出现在全文搜索的结果中，设置的方法是，进入 ST3—Preferences— Settings ，在`Preferences.sublime-settings——User`文件中加入以下设置：  `"file_exclude_patterns": [".tags", ".tags_sorted_by_file", ".gemtags"]`

## 配置文件

ST3—Preferences—Settings 即可打开配置文件。ST3 的配置文件有两个，一个是 Default，一个是 User。二者都以 JSON 格式记录配置信息，其中前者记录着 Sublime Text 的默认配置，禁止用户修改；后者默认为空，允许用户修改。User 配置文件的内容会覆盖 Default 的相应内容，所以只要修改 User 配置文件就好了。

比如我现在的User配置如下：
```
{
 "auto_complete": true,  #打开自动补全功能，这可以提供基本的自动补全
 "copy_with_empty_selection": true,
 "ensure_newline_at_eof_on_save": false, # 保存文件时自动在最后补一行空白行
 "file_exclude_patterns":
 [
 ".tags",
 "tags",
 ".tags_sorted_by_file",
 ".gemtags"
 ],
 "font_size": 18,
 "ignored_packages":
 [
 "Markdown",
 "Vintage"
 ],
 "index_files": true,   #提供基础的索引功能
 "origami_auto_close_empty_panes": true,
 "rulers":
 [
 79
 ],
 "show_minimap": false,  #默认隐藏右侧的地图
 "tab_size": 2,
 "translate_tabs_to_spaces": true,
 "trim_trailing_white_space_on_save": false  #自动去掉你无意间留下的空格
}
```

## 用命令行打开ST3

执行`ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl`，即可在终端执行 `subl projet_name`来用ST3打开项目代码。

## 将sublime设置为git的默认编辑器

执行`git config --global core.editor "subl -n -w"` 即可。执行这个命令后，可以打开`~/.gitconfig`文件看到你的设置。

## 常用的快捷键

### 打开、关闭，前往

command + N 新建文件

command + P 跳转、前往文件、前往项目、命令提示、前往方法（goto anything）  command + T 前往文件  command + R 前往方法  control + G 前往行

control + ` 开关控制台

command + W 关闭当前文件的快捷键

command + K + B 开关左侧栏

在文件中右键，选择reveal in side bar 或 command + shift + \ 在左侧栏打开当前文件所在的路径

### 编辑

command + L 选择行（重复按下将下一行加入选择）  command + D 选择词（重复按下时多重选择相同的词进行多重编辑）

command + shift + V 粘贴并自动缩进

command + shift + S 保存所有文件

### 查找、替换

command + option + F 查找并替换

command + control + g 查找所有符合当前选择的内容进行多重编辑

### 拆分窗口、标签页

command + option + [1,2,3] 单列、双列、三列


---
## 参考

*   [Mac端搭建Ruby环境和使用Sublime Text](https://www.jianshu.com/p/82a86c487990)

*   [解决sublime text3安装Package Control问题](https://segmentfault.com/a/1190000017919431)

*   [https://www.jianshu.com/p/d1d97f8ac64b](https://www.jianshu.com/p/d1d97f8ac64b)

*   [Sublime Text 3 Markdown](http://cheng.logdown.com/posts/2015/06/30/sublime-text-3-markdown)

*   [Setting up Sublime Text 3 for Rails Development](https://mattbrictson.com/sublime-text-3-recommendations)

*   [How to configure Sublime Text 3 for Rubocop and Ruby coding standards](http://joshfrankel.me/blog/how-to-configure-sublime-text-3-for-rubocop-and-ruby-coding-standards/)

*   [https://github.com/SublimeLinter/SublimeLinter/blob/master/SublimeLinter.sublime-settings](https://github.com/SublimeLinter/SublimeLinter/blob/master/SublimeLinter.sublime-settings)

*   [Sublime Text 2的一些插件配置(MacOSX)](https://cnbin.github.io/blog/2016/04/28/sublime-text-2de-xie-cha-jian-pei-zhi-macosx/)

*   [Efficiency with Sublime Text and Ruby](https://www.thunderboltlabs.com/blog/2013/11/19/efficiency-with-sublime-text-and-ruby/)

*   [如何设置Mac OS X下的Sublime Text 3配置文件？](https://www.zhihu.com/question/30242959)

*   [Launch Sublime Text 2 from the Mac OS X Terminal](https://gist.github.com/artero/1236170)

*   [Sublime Text 快捷键（MAC环境）](https://www.jianshu.com/p/6185dc5eb507)
