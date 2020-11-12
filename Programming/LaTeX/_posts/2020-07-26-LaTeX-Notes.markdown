---
layout: post
title:  "给新手的LaTeX小技巧（导言区篇）"
---

目录：

- （此行不会显示）
{:toc}

## 宏语言

就像那些上古的语言——bash, commandline之类一样，TeX是宏语言。
简单地说，宏语言就是完全依赖字符串匹配执行任务的语言。
（如果对此理解困难，想想C++的`#define`和`#ifdef`之类。）

> [宏语言为何不受欢迎？](https://zhuanlan.zhihu.com/p/55122317)
>
> Donald Knuth 所开发的 TeX 系统，其排版原语只有 300 多个，但是通过 TeX 宏可以将这些排版原语组合起来，从而完成更为复杂的排版任务。对于这种任务，宏语言的运行效率要高于一种通用的编程语言。对于 Knuth 而言，这一决策是正确的，因为这样的 TeX 完全满足了他的需求。后来随着排版任务的复杂化，宏的局限性就日益的呈现了出来。如果始终坚持用宏的方式来扩展 TeX 的功能，进度是缓慢的，参与者的数量是逐步减少的，而且这一切都依赖于底层不能发生任何变化。这种系统迟早会变成恐龙的。Knuth 的 TeX 只支持 8 位字符，后来要让它支持中文，Hacker 们不得不绞尽脑汁的在宏包的层面上去做工作，以至于如何让 TeX 支持中文，对于中文用户而言，长期以来一直是初学者遇到的第一个本来不应该是障碍的障碍。

LaTeX对TeX做了包装，使其尽可能像现今流行的编程语言——包括明确的函数作用域、可选参数之类，使其更像是现代语言，但其底层依然脱离不了宏语言的实质。
所幸这种麻烦大多只在编写宏包时遇到，作为初学者大约不用在意这么多（也因此我颇觉得，LaTeX应该支持Python中间层用来编写宏包qwq）。
以下列举对初学者而言，宏语言导致的坑点。

### 作为函数参数的中括号不能嵌套

TeX对大括号做了特别处理，使其可以嵌套；但是LaTeX并没有对中括号作同样的处理。因而函数的中括号定位是基于字符串匹配的！它只会向右找到第一个右中括号，然后配对。因此以下写法报错(只是举例)：

```latex
\section[\cite[def]{ghi}]{abc}
```

为了得到正确结果，我们需要用大括号屏蔽内层函数，也就是如下写法：

```latex
\section[{\cite[def]{ghi}}]{abc}
```

### 数值计算

如你所见，宏对文本非常友好。但是，不妨思考如下问题：如何只用字符串匹配完成10进制数的乘除法呢？对于排版和绘图而言，这是再常用不过的功能。（比如`0.8\textwidth`和绘图里面各种算出来的位置/长度。）

反正这个坑我也不知道怎么办……

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

% 从我导师的preamable template中继承的. % 我并不知道其效果。
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
% % 参考: https://tex.stackexchange.com/questions/21234/doing-something-only-when-the-draft-option-is-on
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
\newcommand{\keywords}{\par\bigskip\noindent\textbf{Keywords: }} % 用来在摘要里加上keywords

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
