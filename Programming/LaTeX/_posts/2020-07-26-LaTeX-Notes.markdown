---
layout: post
title:  "给新手的LaTeX小技巧（导言区篇）"
---

目录：

- （此行不会显示）
{:toc}

## 引用

- 可以使用宏包`cleveref`，调用命令为`\Cref`和`\cref`，可以自动填充引用对象（比如可以显示`Theorem 1.1`而非`1.1`）。
- 可以使用如下命令来使`cleveref`总是使用首字母大写格式：

  ```LaTeX
  \usepackage[capitalize]{cleveref}
  ```

- 宏包`showkeys`用来在引用对象附近注明引用标签。`notref`选项用来关闭在`\ref`，`\Cref`等命令附近的标签。`final`模式下宏包`showkeys`自动被禁用。宏包用法如下：

  ```LaTeX
  \usepackage[notref,notcite]{showkeys} % 文档格式为final时自动禁用
  ```

- 这条没有考证：据说`hyperref`要在`cleveref`前导入。
- 我们使用`~`来输出不能断行的空格。一般引用中的页码“p. 123”或者“引理 1.1”中间的空格不断行，因此建议使用命令`p.~123`和`引理~1.1`。

## 中文显示

- 选项`scheme=plain`用来在支持中文的同时不改变文档原格式（比如显示“Section”而非“节”，首页显示英文日期而非中文日期，等等）。用法：

  ```LaTeX
  \usepackage[scheme=plain]{ctex}
  ```

## 草稿模式

- 选项`final`和`draft`用来控制是在草稿模式还是终稿模式。草稿模式下，如果某一行过长会有黑框。
- 如上所述，`final`模式下宏包`showkeys`自动被禁用。

## 文档样式

- 如下命令可以禁用底部页码。

  ```LaTeX
  \pagestyle{plain}
  ```

### 改变页边距

- 使用宏包的方法：

  ```LaTeX
  \usepackage{geometry}
  \geometry{a4paper,scale=0.8}
  ```

- 不使用宏包的方法：

  ```LaTeX
  \addtolength{\oddsidemargin}{-.875in}
  \addtolength{\evensidemargin}{-.875in}
  \addtolength{\textwidth}{1.75in}
  \addtolength{\topmargin}{-.875in}
  \addtolength{\textheight}{1.75in}
  ```

- 当时寻找下面一种不使用宏包的方法，是因为机械硬盘下宏包`geometry`导入似乎很慢。固态硬盘下看起来问题不大。（别问我为什么编译依然这么慢，我不知道。）

## 字体选择

### 字体简介

LaTeX字体分为三个维度: family, shape, series. 通常而言, 同一个维度只能选择一种字体, 不同维度的字体可以任意组合.

- family字体族: `\rmfamily`, `\sffamify`, `\ttfamily`.
- series字体族: `\mdseries`, `\bfseries`, `\lfseries`.
- shape 字体族: `\upshape`, `\itshape`, `\slshape`, `\scshape`.

这些命令的含义请参见[wikibook](https://en.wikibooks.org/wiki/LaTeX/Fonts#Font_styles). 这些命令的作用域是自命令位置起的全局, 有时候我们只需要使其作用于部分位置, 当如下使用:

```LaTeX
{\rmfamily This is sample text.}
```

或者, 取首两个字母, 前缀`text`或`math`构成单参数命令, 使被作用参数变成指定字体, 形如`\textrm{some text here}`或`\mathit{some text here}`. 如只需采用默认字体, 亦可单使用`\text{some text here}`或`\textnormal{some text here}`.

### 字体大小

字体从小到大依次为:

- `\tiny`
- `\scriptsize`
- `\footnotesize`
- `\small`
- `\normalsize`
- `\large`
- `\Large`
- `\LARGE`
- `\huge`
- `\Huge`

使用方式为 (以下二者效果相同):

- ```LaTeX
  {\large abcd}
  ```

- ```LaTeX
  \begin{large}
      abcd
  \end{large}
  ```

### 参考字体

在选择合适字体时, 如下图片可供参考:
![字体选择参考](/assets/img/LaTeX/fonts choosing.png "字体选择参考")

## 我的导言区代码

附上我的导言区文件：

```LaTeX
\documentclass[a4paper,10pt]{article}
% \pagestyle{plain} % 文档样式; empty则底部不放置页码.

% Basic Packages
\usepackage{amsmath,amsthm,amsfonts,amssymb,mathtools}

% Label, Link
\usepackage[colorlinks]{hyperref}
\usepackage[capitalize]{cleveref}
\usepackage[notref,notcite]{showkeys} % 文档格式为final时自动禁用
\usepackage{url}

% 从我导师的preamable template中传承的. % 我并不知道其效果。
% \newcommand{\email}[1]{\href{mailto:#1}{\nolinkurl{#1}}}
% \newcommand{\OEIS}[1]{\href{http://oeis.org/#1}{\nolinkurl{#1}}}
% \newcommand{\doi}[1]{\textsc{doi}: \href{http://dx.doi.org/#1}{\nolinkurl{#1}}}

% Graph
\usepackage{tikz}
\usetikzlibrary{calc}
\usetikzlibrary{positioning}
% \usepackage{graphicx}   % 支持插入图片
% \usepackage{subcaption} % 支持子图

% Fonts
% \usepackage{mathrsfs} % 花体 \mathscr
% \usepackage{bbm}      % 空心体 \mathbbm
% \usepackage{dsfont}   % 空心体 \mathds

% Other packages
% \usepackage{ifthen} % 支持条件判断
\usepackage[toc,pages]{appendix} % 支持附录
% \usepackage[scheme=plain]{ctex} % 支持中文

% % 定义在draft模式下的额外行为
% % ref: https://tex.stackexchange.com/questions/21234/doing-something-only-when-the-draft-option-is-on
% \makeatletter
% \def\ifdraft{\ifdim\overfullrule>\z@
%   \expandafter\@firstoftwo\else\expandafter\@secondoftwo\fi}
% \makeatother
%
% \usepackage{etoolbox} % 提供各种钩子。本导言区中用到的是\pretocmd。
%
% % 如果在草稿模式下，那么每节前分页，以节省打印纸张。
% \ifdraft{\pretocmd{\section}{\clearpage}{}{}}{}

% New commands
\newcommand{\eqgap}{\;\phantom{=}\;} % 在align环境中对齐使用 % 等于一个等号的间距

% Environments
\theoremstyle{plain}
\newtheorem{theorem}{Theorem}[section]
\newtheorem{corollary}[theorem]{Corollary}
\newtheorem{lemma}[theorem]{Lemma}
\theoremstyle{definition}
\newtheorem{definition}[theorem]{Definition}
\newtheorem{example}[theorem]{Example}
\theoremstyle{remark}
\newtheorem{remark}[theorem]{Remark}
\newtheorem{conjecture}[theorem]{Conjecture}
\crefname{conjecture}{conjecture}{conjectures} % 告诉cleveref如何引用conjecture环境

% Change margin
\usepackage{geometry}
\geometry{a4paper,scale=0.8}

\begin{document}

\end{document}
```
