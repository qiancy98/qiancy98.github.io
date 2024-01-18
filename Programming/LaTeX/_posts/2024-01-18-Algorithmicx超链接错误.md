---
layout: post
title:  "bug溯源及修复：Algorithmicx环境的超链接总是错误指向第一个算法的行号"
---

目录：

- （此行不会显示）
{:toc}

## 问题描述
使用`algorithmicx`宏包时，如果有多个算法，那么超链接总是指向第一个算法的行号。

## 问题重现
[stackexchange问题](https://tex.stackexchange.com/questions/684241/referencing-lines-in-more-than-one-algorithm/707361#707361)

## 问题溯源
在`hyperref`宏包中，对于每个计数器`\thecounter`，会生成计数器`\theHcounter`，用来记录其（用作超链接的）唯一标签。同时，`hyperref`重定义了`\@settoreset`之类的命令，使其同步修改`\theHcounter`。

在[algorithmicx代码](https://github.com/Illumina/manta/blob/75b5c38d4fcd2f6961197b28a41eb61856f2d976/docs/methods/primary/packages/algorithmicx.sty#L79)中，`ALG@line`为手动置零且未设定`HALG@line`，因此`\theHALG@line`为`\theALG@line`，不能保证唯一。在有多标签时，`hyperref`会将超链接指向第一个标签。

## 问题修复
- 将`\theHALG@line`设为`\thealgorithm.\theALG@line`，即可保证唯一。（未测试）
- 或者使用`\@addtoreset{ALG@line}{algorithm}`即可。（已测试）