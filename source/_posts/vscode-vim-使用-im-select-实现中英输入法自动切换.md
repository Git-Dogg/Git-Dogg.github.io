---
title: vscode vim 使用 im-select 实现中英输入法自动切换
date: 2019-03-05 22:43:09
categories:
	- 雕虫小技
	- 小技
tags:
	- Vs Code
	- Vim
---
使用 VS Code 启用 Vim 插件进行 coding 时， Vim Insert 与 Normal 模式互切，牵扯中英输入法切换问题，各模式对应输入法能否相互独立...

<!-- more -->
### 一、问题描述
- 在 **Vs Code** 中使用 **vim**插件，每次进入编辑（insert）模式给代码添加完中文注释时，esc回到正常（normal）模式依然是中文输入法，习惯性有连贯操作的，屏幕一堆中文输入，BlaBla...
- 通过一番搜素，目标锁定 [im-select](https://github.com/daipeihust/im-select#installation),一个github开源项目，简介如下：
Switch your input method from terminal. This project is a basic support for VSCodeVim. It provides the command line program for VSCodeVim's autoSwitchIM function


### 二、解决方案

#### 官方方案
- 参照 VsCode Vim 插件官方文档配置 [Input Method](https://github.com/VSCodeVim/Vim#input-method)

#### 分解官方方案

- **Mac** 用户可以直接按照 [im-select](https://github.com/daipeihust/im-select#installation) 项目文档进行操作
- **Windows** 用户，也可以根据文档操作，但是可能会遇到一些问题，下面单独列出Win10 下 **Vs Code** 使用[im-select](https://github.com/daipeihust/im-select#installation)的详细步骤


---

#### 一般至此可以解决问题 下面可以跳过

---


#### Windows 详细过程

1. 从 [im-select](https://github.com/daipeihust/im-select#installation) 项目下载对应版本(有64位版本) im-select.exe 文件，放到自定义目录，例如 D:\\bin\\im-select.exe
2. 在命令行执行 im-select.exe , 可以输出 当前 Input Method key ，即当前输入法的Key 值，例如：
> 注意：im-select 无法在 Windows cmd.exe 中运行，在 git bash 中或者其他类 Linux 终端里可以执行
> 注意：系统输入法是否添加了 1033（English - United States） 的输入法，不是搜狗（或者类似）的英文输入法
```
$ /d/bin/im-select.exe
1033

# 切换系统输入法

$ /d/bin/im-select.exe
2052
```

3. 这里 1033 是当前为系统美式英文键盘输入法时， 执行 im-select 得到的返回值
 这里 2052 是切换为中文输入法时运行 im-select 得到的返回值

4. 确认好 im-select 命令可以正确拿到需要用到的输入法，配置 **Vs Code** settings.json
    - 此配置使用 1033（en_US）的输入法Key 
    - im-select.exe 位置 D:\bin\im-select.exe
```
    "vim.autoSwitchInputMethod.enable": true, 
    "vim.autoSwitchInputMethod.defaultIM": "1033", # ESC返回 normal 模式时启用的输入法
    "vim.autoSwitchInputMethod.obtainIMCmd": "D:\\bin\\im-select.exe", # im-select 程序目录
    "vim.autoSwitchInputMethod.switchIMCmd": "D:\\bin\\im-select.exe {im}" # {im} 参数是一个命令行选项，它将被传递给 im-select 表示要切换到的输入法
```
5. 配置完成



> 这样在进入 insert 模式时，会切换到最近一次返回 normal 模式时的输入法
> 在 ESC 返回 normal 模式时，会自动切换到 1033（en_US）输入法

### 三、附加
> Windows详细了解输入法 Locale 可以 参考[Windows Locale Codes - Sortable list](https://www.science.co.il/language/Locale-codes.php) ，其中 LCID Decimal 为 Input Method Key

**PS: 建议配合Windows 切换输入法设置，勾选 允许我为每个应用窗口设置不同的输入法**
```
控制面板\时钟、语言和区域\语言\高级设置
```
- [x] 允许我为每个应用窗口设置不同的输入法