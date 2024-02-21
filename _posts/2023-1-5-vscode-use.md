---
layout: post
title: VScode 注释插入插件和代码格式化
description: VScode 注释插入插件和代码格式化
categories:
  - C语言
tags:
  - 编程规范
  - C语言
---

# VScode 注释插入插件和代码格式化

## 一、代码格式化

&emsp;&emsp;VScode自带格式化功能：clang_format，默认格式化后的程序可能不太符合我们的代码规范化要求，此教程是通过修改`.clang-format`文件来保证代码格式化能达到我们的需求。

### 1.快捷键

VScode格式化快捷键为：`Alt` + `Shift` + `F`

### 2.快捷使用方式

可以通过在保存时自动格式化和自动保存功能让代码一直保持工整

![自动保存设置](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E8%87%AA%E5%8A%A8%E4%BF%9D%E5%AD%98%E8%AE%BE%E7%BD%AE.png)

![自动保存设置图2](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E8%87%AA%E5%8A%A8%E4%BF%9D%E5%AD%98%E8%AE%BE%E7%BD%AE%E5%9B%BE2.png)

### 3. .clang-format文件配置

1. 将`.clang-format`文件放入Vscode当前打开文件的根目录，如下图所示

![文件放置位置](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%96%87%E4%BB%B6%E6%94%BE%E7%BD%AE%E4%BD%8D%E7%BD%AE.png)

2. 在VScode中打开此文件

3. 将如下代码复制入次文件中

   ```
   { 
   BasedOnStyle: Microsoft, 
   UseTab: Never, 
   IndentWidth: 4, 
   TabWidth: 4, 
   BreakBeforeBraces: Allman,
   # 缩进case标签
   IndentCaseLabels: true,
   # 每行字符的限制
   ColumnLimit: 80, 
   AccessModifierOffset: -4, 
   # 允许重新排版注释
   ReflowComments:	true,
   # 在赋值运算符之前添加空格
   SpaceBeforeAssignmentOperators:	true,
   # 对齐连续宏定义
   AlignConsecutiveMacros: true,
   # 尾随的注释对齐
   AlignTrailingComments: true,
   # 在尾随的评论前添加的空格数(只适用于//)
   SpacesBeforeTrailingComments: 4, 
   # 允许短的循环while保持在同一行
   AllowShortLoopsOnASingleLine: false,
   # 连续声明时，对齐所有声明的变量名
   AlignConsecutiveDeclarations: false,
   # 指针的对齐: Left, Right, Middle
   PointerAlignment: Right, 
   # 允许短的if语句保持在同一行
   AllowShortIfStatementsOnASingleLine: false, 
   # 允许短的块放在同一行
   AllowShortBlocksOnASingleLine: false,
   # 允许短的枚举放在同一行
   AllowShortEnumsOnASingleLine: false,
   # 允许短的函数放在同一行
   AllowShortFunctionsOnASingleLine: false,
   # 允许函数参数在一行
   AllowAllArgumentsOnNextLine: false,
   # 允许函数声明的所有参数在放在一行
   AllowAllParametersOfDeclarationOnNextLine: false,
   # 允许短的case标签放在同一行
   AllowShortCaseLabelsOnASingleLine:	false,
   # 连续赋值时，对齐所有等号
   AlignConsecutiveAssignments:	false,
   # 表示函数实参要么都在同一行，要么都各自一行
   BinPackArguments: true,
   # false表示所有形参要么都在同一行，要么都各自一行
   BinPackParameters: true,
   # 允许排序#include
   SortIncludes: Never,
   # 使用\r\n换行替代\n
   UseCRLF: true,
   }
   ```

   4. 保存后再次使用快捷键格式化代码即可按照我们设定的格式格式化
   5. 如果需要修改格式，可参考注释修改，或访问[clang_format官方网站](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)

## 二、注释插入插件

插件名称：koroFileHeader

### 1.插件安装教程

1. 在VScode左侧边连选中扩展
2. 搜索`koroFileHeader`
3. 选择图示中的第一个插件安装

![插件安装教程图](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%8F%92%E4%BB%B6%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B%E5%9B%BE.jpg)

### 2.插件设置

1. 安装插件
2. 打开设置界面
3. 在设置界面搜索`fileheader`
4. 选择在settings.json中编辑

![插件设置](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%8F%92%E4%BB%B6%E8%AE%BE%E7%BD%AE.png)

5. 将以下代码复制入设置的json文件中

```
   	//快速添加文件头部注释和函数注释
    "fileheader.configObj": {

        "createFileTime": true, //设置为true则为文件新建时候作为date，否则注释生成时间为date
        "autoAdd": false, //自动生成注释
        "annotationStr": {
            "head": "/*******************************************************************************",
            "middle": " * ",
            "end": "******************************************************************************/"
            //"use": true //设置自定义注释可用
        },
        "language": {
            "c/h/cpp/cs": {
            "head": "/*******************************************************************************",
            "middle": " * ", // 设置中间部分即可
            "end": "******************************************************************************/"
            },
        },
        "createHeader": true ,
        "wideSame": true, // 设置为true开启
        "wideNum": 13, // 字段长度 默认为13
        "moveCursor": true,
        "openFunctionParamsCheck": false
    },
    "fileheader.cursorMode": {
        "名称": "",
        "描述": "",
        "参数": "void",
        "返回": "void",
    },
    "fileheader.customMade": {
        "Copyright (C), 2020-2023, hkzy Co., Ltd.":"",
        "FileName": "",
        "Author": "马开心",
        "Description": "", //文件内容描述
        "Others": "",
        "Function List": "",
        "LastEditTime": "Do not edit",
        "FilePath": "Do not edit", // 设置后，默认生成文件相对于项目的路径
    },
```

注意复制的位置为json文件的第二层，即最大的括号内第一层，示例

```
{
	{
		setting...
	},
	{
		setting...
	},
	// 在这里复制需文件
}
```

6. 快捷键设置

   ![插件设置快捷键](https://kx-image.oss-cn-chengdu.aliyuncs.com/%E6%8F%92%E4%BB%B6%E8%AE%BE%E7%BD%AE%E5%BF%AB%E6%8D%B7%E9%94%AE.png)

在所示位置打开键盘快捷方式设置

在设置中搜索`fileheader`即可配置添加文件头注释的快捷键，当然也可以配置为自动添加文件头注释，在上述的设置文件中的第五行

```
"autoAdd": false, //自动生成注释
```

在设置中搜索`cursorTip`可以配置增加函数注释的快捷键
