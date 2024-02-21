---
layout: post
title: C语音软件编程规范第二版
description: C语音软件编程规范第二版
categories:
  - C语言
tags:
  - 编程规范
  - C语言
---

# C语音软件编程规范第二版

## 一、排版

### 1.缩进

程序块要采用缩进风格编写，缩进的空格数为4个

### 2.空行

相对独立的程序块之间、变量说明之后必须加空行

错误示例：

```c
if(i2c_read_flag == true)
{
    ... // program code
}
uint8_t i2c_recv_nums;
uint8_t i2c_send_nums;
```

应如下书写

```c
if(i2c_read_flag == true)
{
    ... // program code
}

uint8_t i2c_recv_nums;
uint8_t i2c_send_nums;
```

### 3.长语句处理

较长的语句（>80字符）要分成多行书写，长表达式要在低优先级操作符处划分新行，操作符放在新行之首，划分出的新行要进行适当的缩进，使排版整齐，语句可读

示例：

```c
gpio_mode_set(I2C1_GPIO_PROT, GPIO_MODE_AF, GPIO_PUPD_PULLUP,
                          				  I2C1_SDA_GPIO_PIN);
gpio_output_options_set(I2C1_GPIO_PROT, GPIO_OTYPE_OD,
						GPIO_OSPEED_50MHZ, I2C1_SDA_GPIO_PIN);
```

### 4.长语句表达式划分

循环、判断等语句中若有较长的表达式或语句，则要进行适应的划分，长表达式要在低优先级操作符处划分新行，操作符放在新行之首

```c
if ((taskno < max_act_task_number) 
 && (n7stat_stat_item_valid (stat_item))) 
{ 
 ... // program code 
} 
for (i = 0, j = 0; (i < buffer_key_word[word_index].word_length) 
 && (j < new_key_word.word_length); i++, j++) 
{ 
 ... // program code 
} 
for (i = 0, j = 0; 
 (i < first_word_length) && (j < second_word_length); 
 i++, j++) 
{ 
 ... // program code 
}
```

### 5.短语句换行

不允许把多个短语句写在一行中，即一行只写一条语句

错误示例：

```c
rect.length = 0; rect.width = 0;
```

应如下书写：

```c
rect.length = 0; 
rect.width = 0;
```

### 6.大括号

if、for、do、while、case、switch、default等语句自占一行，且if、for、do、while等语句的执行语句部分无论多少都要加括号{}

错误示例：

```c
if (pUserCR == NULL) return;
```

应如下书写：

```
if (pUserCR == NULL) 
{ 
    return; 
}
```

### 7.代码对齐

对齐只使用空格键，不使用TAB键，以免用不同的编辑器阅读程序时，因 TAB 键所设置的空格数目不同而造成程序布局不整齐

### 8.缩进风格

函数或过程的开始、结构的定义及循环、判断等语句中的代码都要采用缩进风格，case语句下的情况处理语句也要遵从语句缩进要求

### 9.大括号

程序块的分界符（如C/C++语言的大括号‘{’和‘}’）应各独占一行并且位于同一列，同时与引用它们的语句左对齐。在函数体的开始、类的定义、结构的定义、枚举的定义以及if、for、do、while、switch、case语句中的程序都要采用如上的缩进方式

错误示例：

```c
for (...) { 
 ... // program code 
} 
if (...) 
    { 
    ... // program code 
    }
void example_fun( void ) 
    { 
    ... // program code 
    }
```

应如下书写

```c
for (...) 
{ 
 ... // program code 
} 
if (...) 
{ 
 ... // program code 
} 
void example_fun(void) 
{ 
 ... // program code 
}
```

### 10.操作符空格

在两个以上的关键字、变量、常量进行对等操作时，它们之间的操作符之前、之后或者前后要加空格；进行非对等操作时，如果是关系密切的立即操作符（如－>），后不应加空格

说明：采用这种松散方式编写代码的目的是使代码更加清晰。由于留空格所产生的清晰性是相对的，所以，在已经非常清晰的语句中没有必要再留空格，
如果语句已足够清晰则括号内侧(即左括号后面和右括号前面)不需要加空格，多重括号间不必加空格，因为在 C/C++语言中括号已经是最清晰的标志了。
在长语句中，如果需要加的空格非常多，那么应该保持整体清晰，而在局部不加空格。给操作符留空格时不要连续留两个以上空格。

示例：

-  逗号、分号只在后面加空格

```c
int a, b, c;
```

- 比较操作符, 赋值操作符"="、 "+="，算术操作符"+"、"%"，逻辑操作符"&&"、"&"，位域操作符"<<"、"^"等双目操作符的前后加空格

```c
if(current_time >= MAX_TIME_VALUE) 
a = b + c; 
a *= 2; 
a = b ^ 2;
```

- "!"、"~"、"++"、"--"、"&"（地址运算符）等单目操作符前后不加空格

```c
*p = 'a'; 			// 内容操作"*"与内容之间
flag = !isEmpty; 	// 非操作"!"与内容之间
p = &mem; 			// 地址操作"&" 与内容之间
i++; 				// "++","--"与内容之间
```

- "->"、"."前后不加空格

```c
p->id = pid; // "->"指针前后不加空格
```

- if、for、while、switch 等与后面的括号间应加空格，使 if 等关键字更为突出、明显

```c
if (a >= b && c > d)
```

## 二、注释

### 1.换行

一行程序以小于80字符为宜，不要写得过长

### 2.注释量

一般情况下，源程序有效注释量必须在20％以上

说明：注释的原则是有助于对程序的阅读理解，在该加的地方都加了，注释不宜太多也不能太少，注释语言必须准确、易懂、简洁

### 3.文件说明

说明性文件（如头文件.h文件、.inc文件、.def文件、编译说明文件.cfg等）头部应进行注释，注释必须列出：文件名、作者、描述、函数列表、最后编辑时间、文件路径等

示例：下面这段头文件的头注释比较标准，当然，并不局限于此格式，但上述信息建议要包含在内

```c
/***************************************************
 * Copyright (C), 2020-2023, hkzy Co., Ltd.:
 * FileName     : i2c_driver.c
 * Author       : 马开心
 * Description  : i2c驱动模块
 * Others       : 
 * Function List: void i2c_init_task(void);            // I2C初始化函数
 *                void i2c_nvic_init(void);            // i2c中断初始化函数
 *                void i2c_master_init(void);          // 主机初始化函数
 *                void i2c_slave_init(void);           // 从机初始化函数
 *                void i2c_dma_init(uint32_t port);    // I2C DMA初始化函数
 *                void i2c_dma_send(void);             // I2C DMA发送单字节数据
 * LastEditTime : 2023-01-05 15:31:57
 * FilePath     : \gd32_iic_slave_master\app\source\i2c_driver.c
 ***************************************************/
```

### 4.函数注释

函数头部应进行注释，列出：函数的目的/功能、输入参数、返回值、等

示例：下面这段函数的注释比较标准，当然，并不局限于此格式，但上述信息建议要包含

```c
/***************************************************
 * 名称: i2c_init
 * 描述: i2c初始化函数封装
 * 作者: 马开心
 * 参数: port：I2C端口号
 *      speed：I2C速度
 *      addr：主从机地址
 * 返回: void
 ***************************************************/
```

### 5.代码和注释一致性

边写代码边注释，修改代码同时修改相应的注释，以保证注释与代码的一致性。不再有用的注释要删除

### 6.注释需要明确

注释的内容要清楚、明了，含义准确，防止注释二义性

说明：错误的注释不但无益反而有害

### 7.避免注释缩写

避免在注释中使用缩写，特别是非常用缩写

说明：在使用缩写时或之前，应对缩写进行必要的说明

### 8.注释位置

注释应与其描述的代码相近，对代码的注释应放在其上方或右方（对单条语句的注释）相邻位置，不可放在下面，如放于上方则需与其上面的代码用空行隔开

错误示例1：

```c
/* get replicate sub system index and net indicator */ 


repssn_ind = ssn_data[index].repssn_index; 
repssn_ni = ssn_data[index].ni; 
```

错误示例2：

```c
repssn_ind = ssn_data[index].repssn_index; 
repssn_ni = ssn_data[index].ni; 
/* get replicate sub system index and net indicator */
```

应如下书写

```c
/* get replicate sub system index and net indicator */
repssn_ind = ssn_data[index].repssn_index; 
repssn_ni = ssn_data[index].ni; 
```

### 9.变量常量注释

对于所有有物理含义的变量、常量，如果其命名不是充分自注释的，在声明时都必须加以注释，说明其物理含义。变量、常量、宏的注释应放在其上方相邻位置或右方

示例：

```c
/* active statistic task number */ 
#define MAX_ACT_TASK_NUMBER 1000

#define MAX_ACT_TASK_NUMBER 1000 /* active statistic task number */
```

### 10.数据结构声明

数据结构声明(包括数组、结构、类、枚举等)，如果其命名不是充分自注释的，必须加以注释。对数据结构的注释应放在其上方相邻位置，不可放在下面；对结构中的每个域的注释放在此域的右方

示例：

```c
/* sccp interface with sccp user primitive message name */ 
enum SCCP_USER_PRIMITIVE 
{ 
    N_UNITDATA_IND, /* sccp notify sccp user unit data come */ 
    N_NOTICE_IND, 	/* sccp notify user the No.7 network can not */ 
    				/* transmission this message */ 
    N_UNITDATA_REQ, /* sccp user's unit data transmission request*/ 
};
```

### 11.注释缩进

注释与所描述内容进行同样的缩排

错误示例：

```c
void example_fun( void ) 
{ 
/* code one comments */ 
    CodeBlock One
        /* code two comments */ 
    CodeBlock Two 
}
```

应如下书写

```c
void example_fun( void ) 
{ 
    /* code one comments */ 
    CodeBlock One 

    /* code two comments */ 
    CodeBlock Two 
}
```

### 12.注释空行

将注释与其上面的代码用空行隔开

示例：如下例子，显得代码过于紧凑

```c
/* code one comments */ 
program code one 
/* code two comments */ 
program code two
```

应如下书写

```c
/* code one comments */ 
program code one 
    
/* code two comments */ 
program code two
```

### 13.分支语句注释

对变量的定义和分支语句（条件分支、循环语句等）必须编写注释

说明：这些语句往往是程序实现某一特定功能的关键，对于维护人员来说，良好的注释帮助更好的理解程序，有时甚至优于看设计文档。

### 14.代码中间不能添加注释

避免在一行代码或表达式的中间插入注释

说明：除非必要，不应在代码或表达中间插入注释，否则容易使代码可理解性变差

### 15.命名和注释

通过对函数或过程、变量、结构等正确的命名以及合理地组织代码的结构

说明：注释的目的是解释代码的目的、功能和采用的方法，提供代码以外的信息，帮助读者理解代码，防止没必要的重复注释信息

示例：如下注释意义不大

```c
/* if receive_flag is TRUE */ 
if (receive_flag)
```

而如下注释则给出了额外有用的信息

```c
/* if mtp receive a message from links */ 
if (receive_flag)
```

### 16.注释语言选择

注释应考虑程序易读及外观排版的因素，使用的语言若是中、英兼有的，建议多使用中文，除非能用非常流利准确的英文表达

说明：注释语言不统一，影响程序易读性和外观排版，出于对维护人员的考虑，建议使用中文

## 三、标识符命名

### 1.命名清晰

标识符的命名要清晰、明了，有明确含义，同时使用完整的单词或大家基本可以理解的缩写，避免使人产生误解

说明：较短的单词可通过去掉“元音”形成缩写；较长的单词可取单词的头几个字母形成缩写；一些单词有大家公认的缩写。

示例：如下单词的缩写能够被大家基本认可

- temp 可缩写为 tmp ; 

- flag 可缩写为 flg ; 

- statistic 可缩写为 stat ; 

- increment 可缩写为 inc ; 

- message 可缩写为 msg ;

### 2.缩写需要注释

命名中若使用特殊约定或缩写，则要有注释说明

说明：应该在源文件的开始之处，对文件中所使用的缩写或约定，特别是特殊的缩写，进行必要的注释说明

### 3.风格固定

自己特有的命名风格，要自始至终保持一致，不可来回变化

说明：个人的命名风格，在符合所在项目组或产品组的命名规则的前提下，才可使用。（即命名规则中没有规定到的地方才可有个人命名风格）

### 4.局部变量命名

对于变量命名，禁止取单个字符（如i、j、k...），建议除了要有具体含义外，还能表明其变量类型、数据类型等，但i、j、k作局部循环变量是允许的

说明：变量，尤其是局部变量，如果用单个字符表示，很容易敲错（如 i 写成 j），而编译时又检查不出来，有可能为了这个小小的错误而花费大量的查错时间

### 5.命名方式

**采用UNIX的全小写加下划线的命名方式**

- 直观可以拼读，望文得知意，便于记忆，采用英文单词或组合，不能使用拼音，英文单词也不要太复杂，必要时可以使用简写
- 尽量避免名字中出现数字编号

### 6.标识符

除非必要，不要用数字或较奇怪的字符来定义标识符

示例：如下命名，使人产生疑惑

```c
#define _EXAMPLE_0_TEST_ 
#define _EXAMPLE_1_TEST_ 
void set_sls00( BYTE sls );
```

应改为有意义的单词命名

```c
#define _EXAMPLE_UNIT_TEST_ 
#define _EXAMPLE_ASSERT_TEST_ 
void set_udt_msg_sls( BYTE sls );
```

### 7.反义词使用

用正确的反义词组命名具有互斥意义的变量或相反动作的函数等

说明：下面是一些在软件中常用的反义词组。

- add / remove 
- begin / end 
- create / destroy 

- insert / delete 
- first / last 
- get / release 

- increment / decrement 
- put / get 

- add / delete 
- lock / unlock 
- open / close 

- min / max
- old / new 
- start / stop 

- next / previous 
- source / target 
- show / hide 

- send / receive 
- source / destination 

- cut / paste 
- up / down 

### 8.下划线使用

除了编译开关/头文件等特殊应用，应避免使用`_EXAMPLE_TEST_`之类以下划线开始和结尾的定义

### 9.标识符前缀和后缀

- 结构体后缀为`_t`
- 联合体后缀为`_u`
- 枚举类型后缀为`_e`
- 全局变量前缀为`g_`
- 回调函数后缀为`_cb`

### 10.函数命名

全局函数命名采用小写+下划线方式，禁止使用拼音命名

静态函数使用驼峰命名方式，禁止使用拼音命名

整体命名结构采用主-谓-宾的命名方式，如`client_get_users_info()`

## 四、可读性

### 1.优先级

注意运算符的优先级，并用括号明确表达式的操作顺序，避免使用默认优先级

说明：防止阅读程序时产生误解，防止因默认的优先级与设计思想不符而导致程序出错

示例：下列语句中的表达式

```c
word = (high << 8) | low (1) 
if ((a | b) && (a & c)) (2) 
if ((a | b) < (c & d)) (3)
```

如果书写为

```c
high << 8 | low 
a | b && a & c 
a | b < c & d
```

由于

```c
high << 8 | low = ( high << 8) | low, 
a | b && a & c = (a | b) && (a & c)，
```

(1)(2)不会出错，但语句不易理解；

`a | b < c & d = a | （b < c） & d`，(3)造成了判断条件出错。

### 2.避免使用数字

避免使用不易理解的数字，用有意义的标识来替代。涉及物理状态或者含有物理意义的常量，不应直接使用数字，必须用有意义的枚举或宏来代替

示例：如下的程序可读性差

```c
if (Trunk[index].trunk_state == 0) 
{ 
    Trunk[index].trunk_state = 1; 
    ... // program code 
}
```

应改为如下形式

```c
#define TRUNK_IDLE 0 
#define TRUNK_BUSY 1 

if (Trunk[index].trunk_state == TRUNK_IDLE) 
{ 
    Trunk[index].trunk_state = TRUNK_BUSY;
    ... // program code 
}
```

### 3.代码位置

源程序中关系较为紧密的代码应尽可能相邻

说明：便于程序阅读和查找

示例：以下代码布局不太合理

```c
rect.length = 10; 
char_poi = str; 
rect.width = 5;
```

若按如下形式书写，可能更清晰一些

```c
rect.length = 10; 
rect.width = 5; // 矩形的长与宽关系较密切，放在一起。
char_poi = str;
```

### 4.技巧性语句

不要使用难懂的技巧性很高的语句，除非很有必要时

说明：高技巧语句不等于高效率的程序，实际上程序的效率关键在于算法
示例：如下表达式，考虑不周就可能出问题，也较难理解

```c
* stat_poi ++ += 1; 
* ++ stat_poi += 1;
```

应分别改为如下

```c
*stat_poi += 1; 
stat_poi++; // 此二语句功能相当于“ * stat_poi ++ += 1; ”

++ stat_poi; 
*stat_poi += 1; // 此二语句功能相当于“ * ++ stat_poi += 1; ”
```

## 五、示例文件

### 1.template.c

```c
/*******************************************************************************
 * Copyright (C), 2020-2023, hkzy Co., Ltd.:
 * FileName     : template.c
 * Author       : 马开心
 * Description  : 这是一个示例文件
 * Others       :
 * Function List: static void UsartNnvicInit(uint8_t data, uint8_t data_next)
 *                void usart_send_byte(uint8_t data)
 * LastEditTime : 2023-01-30 15:50:04
 * FilePath     : \C语言实现类\template.c
 ******************************************************************************/

/*******************************************************************************
 * 头文件包含
 ******************************************************************************/
/* 库函数头文件*/
#include <stdio.h>
#include <stdlib.h>
/* 外部自定义头文件 */
// #include "xxx.h"
/* 自定义头文件 */
#include "template.h"

/*******************************************************************************
 * 内部结构体声明（private）
 ******************************************************************************/
typedef struct
{
    uint8_t start_flg;
    uint8_t stop_flg;
    uint8_t console_flg;
} priv_tag_t;

/*******************************************************************************
 * 内部函数声明
 ******************************************************************************/
static priv_tag_t *privTagGet(void);
static void        printA(void);
static void        myCallback(uint8_t num);

/*******************************************************************************
 * 外部函数定义
 ******************************************************************************/
/***************************************************
 * 名称: template_init
 * 描述: template类初始化
 * 参数: void
 * 返回: 结构体指针
 ***************************************************/
template_t *template_init(void)
{
    // 外部
    template_t *template_test = (template_t *)malloc(sizeof(template_t));
    template_test->type = 10;
    template_test->cb = myCallback;
    template_test->print = printA;

    // 内部
    priv_tag_t *tag = privTagGet();
    return template_test;
}

/***************************************************
 * 名称: template_destory
 * 描述: template类销毁
 * 参数: template：结构体指针地址
 * 返回: void
 ***************************************************/
void template_destory(template_t *temp)
{
    free(temp);
}

/***************************************************
 * 名称: template_test
 * 描述: 这是一个函数描述
 * 作者: ...
 * 参数: data：数据
 * 返回: void
 ***************************************************/
void template_test(template_t *temp)
{
    // 局部变量
    uint16_t nums_temp = 0;
    uint16_t a_temp = 0;
    uint16_t b_temp = 0;
    uint16_t c_temp = 0;
    uint16_t i = 0;
    // 局部静态变量
    static uint16_t sta_nums_temp;

    // 改变类属性
    temp->type = 66;
    temp->cb(temp->type);

    // 改变私有变量
    priv_tag_t *tag = privTagGet();
    tag->start_flg = 1;

    // switch函数
    switch (nums_temp)
    {
        case STEP0:
            // code
            break;
        case STEP1:
            // code
            break;
        default:
            break;
    }

    // if-else
    if ((sta_nums_temp == 1) || (sta_nums_temp == 2))
    {
        // code
    }
    else if ((sta_nums_temp == 3) && (sta_nums_temp == 4))
    {
        // code
    }
    else if (sta_nums_temp != 100)
    {
        // code
    }

    // 各种运算符运用
    a_temp = b_temp + c_temp;
    a_temp++;
    a_temp *= 10;
    a_temp = a_temp | b_temp;

    // while循环
    while (nums_temp)
    {
        nums_temp--;
    }

    // for循环
    for (i = 0; i < 10; i++)
    {
        nums_temp++;
    }

    // do-while循环
    do
    {
        nums_temp++;
    } while (!nums_temp);
}

/*******************************************************************************
 * 内部函数定义
 ******************************************************************************/
/***************************************************
 * 名称: privTagGet
 * 描述: 单例，获取内部类（私有属性）
 * 参数: void
 * 返回: void
 ***************************************************/
static priv_tag_t *privTagGet(void)
{
    static priv_tag_t *tag;

    if (tag == NULL)
    {
        tag = (priv_tag_t *)malloc(sizeof(priv_tag_t));
    }

    return tag;
}

/***************************************************
 * 名称: print_a
 * 描述: 实现产品函数
 * 参数: void
 * 返回: void
 ***************************************************/
static void printA(void)
{
    printf("I am Product A\n");
}

/***************************************************
 * 名称: my_callback
 * 描述: 回调函数的实现
 * 参数: void
 * 返回: void
 ***************************************************/
static void myCallback(uint8_t num)
{
    printf("Got %d\n", num);
}

/*******************************************************************************
 * EOF (not truncated)
 ******************************************************************************/

```

### 2.template.h

```c
/***************************************************
 * Copyright (C), 2020-2023, hkzy Co., Ltd.:
 * FileName     : template.h
 * Author       : 马开心
 * Description  : 示例文件头文件
 * Others       :
 * Function List:
 * LastEditTime : 2023-01-30 15:59:40
 * FilePath     : \C语言实现类\template.h
 ***************************************************/
#ifndef _TEMPLATE_H_
#define _TEMPLATE_H_

/*******************************************************************************
 * 头文件包含,只需要使用头文件用到的
 ******************************************************************************/
#include <stdint.h>

/*******************************************************************************
 * 宏定义
 ******************************************************************************/
#define CON_FLG_ADDR (0x8021u)    // 宏定义注释
#define I2C_ADDR     (0x0013)     // 宏定义注释

/*******************************************************************************
 * typedef声明
 ******************************************************************************/
typedef void (*callback_cb)(uint8_t);
typedef void (*print_cb)();

// typedef unsigned char uint8_t;

/*******************************************************************************
 * 类结构体声明（public）
 ******************************************************************************/
typedef union perdata
{
    int  value;
    char office;
} perdata_u;

typedef enum
{
    STEP0 = 0,
    STEP1,
    STEP2,
    STEP3
} step_e;

typedef struct
{
    uint8_t     type;
    perdata_u   uni;
    step_e      enm;
    print_cb    print;
    callback_cb cb;
} template_t;

/*******************************************************************************
 * 外部函数声明
 ******************************************************************************/
template_t *template_init(void);
void        template_destory(template_t *temp);
void        template_test(template_t *temp);

#endif /* _TEMPLATE_H_ */
/*******************************************************************************
 * EOF (not truncated)
 ******************************************************************************/

```

### 3.client.c

```c
/***************************************************
 * Copyright (C), 2020-2023, hkzy Co., Ltd.:
 * FileName     :
 * Author       : 马开心
 * Description  :
 * Others       :
 * Function List:
 * LastEditTime : 2023-01-30 17:00:40
 * FilePath     : \c_class_test\client.c
 ***************************************************/
#include <stdio.h>
#include "template.h"
#include "template.c"

/***************************************************
 * 名称: main
 * 描述: 主函数调用
 * 参数: void
 * 返回: void
 ***************************************************/
int main(int argc, char *argv[])
{
    template_t *temp = template_init();    // 类的初始化

    temp->print();       // 类的方法调用
    temp->type = 255;    // 改变共有类属性

    template_test(temp);

    template_destory(temp);    // 释放类的内存

    return 0;
}
```

