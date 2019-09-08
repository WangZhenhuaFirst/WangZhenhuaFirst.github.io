---
layout: post
title: 安利Markdown神器typora
date: 2019-09-08 00:00:00
author: "王振华"
tags:

- Markdown
- typora
---



少数派的[Typora 完全使用详解](https://sspai.com/post/54912) 已经对 [typora](https://www.typora.io/) 介绍的比较详细了，我这里只做几点补充。

我用的是MacBook，但Windows和Linux应该也大同小异。



### 写好后想存到其他地方，如何保留Markdown格式？

因为typora毕竟想做一个安安静静的Markdown编辑器，所以在保存、管理大量文件上的确还有些不尽如人意的地方。所以很多人用typora写完之后，希望存到印象笔记，或者发布到微信公众号上。但因为typora的Markdown是没有预览过程的，所写即所得，所以有人会担心转存到别的地方无法保留Markdown格式。

其实很简单，如果写好后想在微信公众号发布，直接复制粘贴即可。如果想保存到印象笔记中，只需要全选后右键单击，选择`复制为 Markdown(copy as Markdown)`，粘贴到印象笔记中，所有Markdown格式都会完好无损。



### 如何打开多个标签页？

我们工作时，很多时候都需要打开多个文件。将鼠标放在左侧sidebar要打开的文件上，右键单击，你会看到`open` 和`open in new windwow` 这两个选项，并没有 `open in new tab`。

那要如何实现打开多个标签页呢？其实很简单，鼠标移动到左侧sidebar要打开的文件上，按住command键的同时，鼠标单击即可。

你会发现这个操作和在Google、Safari 等浏览器中打开一个超链接时的操作是一样的。是的，这就是少数派的[Typora 完全使用详解](https://sspai.com/post/54912) 这篇文章中说的——typora其实是个伪装成文本编辑器的浏览器，我真是不能再赞同了。



### 快捷键的修改

因为我写代码的时候用的是Sublime编辑器，Sublime打开/关闭侧边栏的快捷键是 `command+\`，所以我希望用typora时也是同样的快捷键，但typora默认打开侧边栏的快捷键是`shift+command+L`，要如何修改呢？

参考[Shortcut Keys](https://support.typora.io/Shortcut-Keys/)，这里有typora的所有快捷键列表，找到我想修改的那个：`Toggle Sidebar  Command + Shift +L`，然后按照Shortcut Keys的 Change Shortcut Keys 部分的操作步骤，加上一个`Toggle Sidebar Command+\`即可。

注意，因为在typora的默认快捷键中，有`Source Code Mode Command+/`，所以如果你只是加了以上快捷键，就会有快捷键冲突，所以你需要再给`Source Code Mode` 加一个其他不用的快捷键。



顺便说一句，本文就是用typora写成的:smile:。





参考：

- [Typora 完全使用详解](https://sspai.com/post/54912)
- [Shortcut Keys](https://support.typora.io/Shortcut-Keys/)
- [Opening files In same tab, new tab, or new window: A flexible & consistent UI proposal](https://github.com/typora/typora-issues/issues/1000)