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

LaTeX对TeX做了包装，使其尽可能像现今流行的编程语言——包括明确的函数作用域、可选参数之类，但其底层依然脱离不了宏语言的实质。
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

## 文档

所有文档都可以在这里找到：[CTAN: Comprehensive TeX Archive Network](https://www.ctan.org)
如果下载的是完整的TEX发行版，那么你应该可以在本地看到这些文档。在VS Code下，这需要将鼠标悬浮在需要的包上（例如，`\usepackage{cleveref}`中的`cleveref`），在弹出的提示框内点击`View Document`即可。

已知例外是，`TiKZ`包的文档在`pgf`下。

## Shebang

暂且记录于此。可以选择性使用。

```LaTeX
% !TEX encoding = UTF-8
% !TEX program = latexmk
% % !TEX root = somefile.tex
% !BIB program = biber % or bibtex
```

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

### biblatex

- `biblatex`包下常用的引用格式参见[这里](https://www.overleaf.com/learn/latex/Biblatex_citation_styles).

### 子图、子表格

我们使用如下命令以引用子图：（默认引用`Figure 1a`）

```LaTeX
\usepackage{subcaption}
```

如果我们想让子图引用为`Figure 1(a)`，则应采取以下写法：

```LaTeX
\usepackage[labelformat=simple]{subcaption}
\renewcommand\thesubfigure{(\alph{subfigure})}
\renewcommand\thesubtable{(\alph{subtable})}
```

### 改变enumerate的枚举格式

```LaTeX
\renewcommand{\labelenumi}{(\arabic{enumi}).}
```

### 编号

在节内编号

```LaTeX
\numberwithin{equation}{section}
\numberwithin{figure}{section}
\numberwithin{table}{section}
```

让图片、表格和算法共享编号。

```LaTeX
\makeatletter
\let\c@table\c@figure
\let\c@algorithm\c@figure
\makeatother
```

## 中文显示

- 选项`scheme=plain`用来在支持中文的同时不改变文档原格式（比如显示“Section”而非“节”，首页显示英文日期而非中文日期，等等）。用法：

  ```LaTeX
  \usepackage[scheme=plain]{ctex}
  ```

## 草稿模式

- 选项`final`和`draft`用来控制是在草稿模式还是终稿模式。草稿模式下，如果某一行过长会有黑框。
- 如上所述，`final`模式下宏包`showkeys`自动被禁用。

### 定义草稿模式下的额外行为

草稿模式较其他模式唯一的区别为：草稿模式下`\overfullrule`大于0（作用是，在行末溢出处加个黑框提示溢出）。
因此可以通过检测`\overfullrule`的值判断是否在草稿模式。

[参考网页。](https://tex.stackexchange.com/questions/21234/doing-something-only-when-the-draft-option-is-on)

```LaTeX
% 定义在draft模式下的额外行为
\makeatletter
\def\ifdraft{\ifdim\overfullrule>\z@
  \expandafter\@firstoftwo\else\expandafter\@secondoftwo\fi}
\makeatother
% 如果在草稿模式下，那么每节前分页，以节省打印纸张。
\ifdraft{\pretocmd{\section}{\clearpage}{}{}}{}
```

如果写包的话，问题简单得多：draft将会作为选项传递给包。因此直接检测选项即可。

## 文档样式

- 如下命令可以禁用底部页码。

  ```LaTeX
  \pagestyle{plain}
  ```
- 如下命令以手动调整页面大小。

  ```LaTeX
  \special{papersize=148mm,210mm} % A5 paper size.
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

## 数学公式

### 字符间距

- 建议使用`\lvert`和`\rvert`作为左右`|`符号，以保证正确间距。
- `\bigl`, `\bigm`, `\bigr`, `\big`效果几乎一样，但是其作用后的符号类型分别为`\mathopen`, `\mathrel`, `\mathclose`, `\mathord`.
- 十分不幸， $\TeX$ 中可以正确处理双目运算符和左结合单目运算符，但是无法正常处理右结合单目运算符（思考：`n! r!`）。这是 $\TeX$ 的锅，我们为了正常显示，只能在`!`后加上 $\frac{3}{18}$ em 的空格，即`\,`。

### 手动标号

经常遇见的情况是，一长串公式只有一个想要标号，因此不想每行加个`\nonumber`。能不能造个相反功能的`\numberthis`? (比如说，用来在`align*`环境中加入tag)
以下给出两种方案：（我的导言区采用了第一种）

- `\newcommand{\numberthis}{\refstepcounter{equation}\tag{\theequation}}`
- `\newcommand{\numberthis}{\addtocounter{equation}{1}\tag{\theequation}}`


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

## 证明中分情况讨论

证明中分情况讨论，可能还需要引用，但是似乎`amsthm`却缺少相应环境……怎么办？
——自己动手丰衣足食嘛~

这是(默认环境的参数)[https://tex.stackexchange.com/questions/17551/amsthm-what-are-the-newtheoremstyle-parameters-for-the-default-styles]。

仿照之，可以魔改出Case的参数，如以下代码。
随后使用`\renewcommand{\thecase}{\arabic{case}}`修改默认显示（引用）方法。

我的最终代码如下：
```LaTeX
% Case in proof
\newtheoremstyle{ModifiedRemark}
    {0.5\topsep}             % ABOVE SPACE
    {0.5\topsep}             % BELOW SPACE
    {\normalfont}            % BODY FONT
    {0pt}                    % INDENT (empty value is the same as 0pt)
    {\scshape}               % HEAD FONT % Different from Environment"remark"
    {.}                      % HEAD PUNCT
    {5pt plus 1pt minus 1pt} % HEAD SPACE
    {}                       % CUSTOM-HEAD-SPEC
\theoremstyle{ModifiedRemark}
\newtheorem{case}{Case}[theorem]
\renewcommand{\thecase}{\arabic{case}}
\crefname{case}{Case}{Cases} % 告诉cleveref如何引用case环境
```

## 我的导言区代码（已弃用）

附上我的导言区文件：

```LaTeX
\documentclass[a4paper,10pt]{article}
\usepackage{amsmath,amsthm,amssymb,mathtools} % Basic Packages % amssymb依赖amsfonts
\usepackage{regexpatch} % 替代etoolbox提供各种钩子。本导言区中用到的是\pretocmd。

% Fonts
% \usepackage{mathrsfs} % 花体 \mathscr
% \usepackage{bbm}      % 空心体 \mathbbm
% \usepackage{dsfont}   % 空心体 \mathds

% Change margin
% \pagestyle{plain} % 文档样式; empty则底部不放置页码.
% \special{papersize=148mm,210mm} % A5 paper size. % 如果需要手动调整页面大小
\usepackage{geometry}
\geometry{a4paper,scale=0.8}

% Graph
\usepackage{tikz}
\usetikzlibrary{calc}
\usetikzlibrary{positioning}
% \usepackage{graphicx}   % 支持插入图片
% 支持子图，引用为Figure 1(a)
\usepackage[labelformat=simple]{subcaption}
\renewcommand\thesubfigure{(\alph{subfigure})}
\renewcommand\thesubtable{(\alph{subtable})}

% Label, Link
\usepackage[style=alphabetic,sorting=none,isbn=false]{biblatex} % numeric, authoryear.
% \addbibresource{./put/bib/file/here.bib}
\ExecuteBibliographyOptions
{
    maxcitenames=3, maxbibnames=100, minnames=3,
}
\AtEveryBibitem{
    \clearfield{issn}
}
\usepackage[colorlinks]{hyperref}
\usepackage[capitalize]{cleveref}
\usepackage[notref,notcite]{showkeys} % 文档格式为final时自动禁用
\usepackage{url}

% Other packages
% \usepackage{ifthen} % 支持条件判断
% \usepackage[toc,page]{appendix} % 支持附录
% \usepackage[scheme=plain]{ctex} % 支持中文

\makeatletter
\def\ifdraft{\ifdim\overfullrule>\z@ % 定义在draft模式下的额外行为
    \expandafter\@firstoftwo\else\expandafter\@secondoftwo\fi}
\makeatother
% \ifdraft{\pretocmd{\section}{\clearpage}{}{\GenericWarning{Error when prepending to command \backslash section}}}{} % 如果在草稿模式下，那么每节前分页，以节省打印纸张。
\newcommand{\ProofStyle}[2][\square]{
    \cspreto{#2}{\pushQED{\qed}\renewcommand{\qedsymbol}{\ensuremath{#1}}}
    \cspreto{end#2}{\popQED}
}

% % 编号
% \numberwithin{equation}{section}
% \numberwithin{figure}{section}
% \numberwithin{table}{section}
% \renewcommand{\labelenumi}{(\arabic{enumi}).}
% % 让图片、表格、算法共享编号。
% \makeatletter
% \let\c@table\c@figure
% \let\c@algorithm\c@figure
% \makeatother

% Environments
\newtheoremstyle{ModifiedRemark}
    {0.5\topsep}             % ABOVE SPACE
    {0.5\topsep}             % BELOW SPACE
    {\normalfont}            % BODY FONT
    {0pt}                    % INDENT (empty value is the same as 0pt)
    {\scshape}               % HEAD FONT % Different from Environment{remark}
    {.}                      % HEAD PUNCT
    {5pt plus 1pt minus 1pt} % HEAD SPACE
    {}                       % CUSTOM-HEAD-SPEC
\theoremstyle{plain}
\newtheorem{theorem}{Theorem}[section]
\newtheorem{proposition}[theorem]{Proposition}
\newtheorem{corollary}[theorem]{Corollary}
\newtheorem{lemma}[theorem]{Lemma}
\newtheorem{conjecture}[theorem]{Conjecture}
\newtheorem{question}[theorem]{Question}
\theoremstyle{definition}
\newtheorem{definition}[theorem]{Definition}
\theoremstyle{remark}
\newtheorem{example}[theorem]{Example}
\newtheorem{remark}[theorem]{Remark}
\ProofStyle[\Diamond]{example}
\ProofStyle[\Diamond]{remark}
\theoremstyle{ModifiedRemark}
\newtheorem{case}{Case}[theorem]
\newtheorem{claim}[case]{Claim}
% 告诉cleveref如何引用环境
\renewcommand{\thecase}{\arabic{case}}
\crefname{conjecture}{Conjecture}{Conjectures}
\crefname{example}{Example}{Examples}
\crefname{case}{Case}{Cases}
\crefname{claim}{Claim}{Claims}

% 从我导师的preamable template中继承的. % 我并不知道其效果。
% \newcommand{\OEIS}[1]{\href{http://oeis.org/#1}{\nolinkurl{#1}}}
% \newcommand{\doi}[1]{\textsc{doi}: \href{http://dx.doi.org/#1}{\nolinkurl{#1}}}

% New commands
\newcommand{\email}[1]{\href{mailto:#1}{\nolinkurl{#1}}}
\newcommand{\keywords}{\par\bigskip\noindent\textbf{Keywords: }} % 用来在摘要里加上keywords
\newcommand{\eqgap}{\mathrel{\phantom{=}}} % 在align环境中对齐使用 % 等于一个等号的间距
\newcommand{\numberthis}{\refstepcounter{equation}\tag{\theequation}} % 用来在align*加入tag

\begin{document}

\end{document}
```
