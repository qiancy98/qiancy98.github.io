---
layout: post
title:  "miniconda使用说明"
---

目录：

- （此行不会显示）
{:toc}

## 使用说明

| 操作 | 命令 | 说明 |
| --- | --- | --- |
| 创建环境 | `conda create -n env_name python=3.11` | 创建一个名为`env_name`的环境，使用Python 3.11 |
| 查询环境 | `conda env list` | 是 `conda info --envs` 的别名 |
| 激活环境 | `conda activate env_name` | 激活名为`env_name`的环境 |
| 退出环境 | `conda deactivate` | 退出当前环境 |
| 删除环境 | `conda remove -n env_name --all` | 删除名为`env_name`的环境 |

然后用`python`命令而不是`py`命令启动Python解释器。(`py`命令是Windows系统的Python解释器启动命令)

## 配合Terminal使用

使用如下命令行配置Terminal:

```cmd
%windir%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy ByPass -NoExit -Command "& '%userprofile%\miniconda3\shell\condabin\conda-hook.ps1'; Set-Alias -Name py -Value python; conda activate gptac_venv "
```

其中`gptac_venv`是启动时的环境名。我在这里设置了`py`命令为`python`命令的别名，这样就可以直接使用`py`命令启动Python解释器(主要是我习惯用`py`命令了)。

如下是原始的命令行(虽然我不清楚为什么要用绝对路径来activate环境):

```cmd
%windir%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy ByPass -NoExit -Command "& '%userprofile%\miniconda3\shell\condabin\conda-hook.ps1' ; conda activate '%userprofile%\miniconda3' "
```
