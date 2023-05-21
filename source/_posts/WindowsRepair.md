---
title: 常见系统问题的正确处理方式
date: 2022-8-27
cover: /img/hzy.png
description: 自救流程
categories:
  - 教程
tags:
  - 系统
---

# "在没有错误日志的情况下对任何问题进行故障排除就像闭着眼睛开车一样"

## 入门部分

既然你点了进来，那么大概率你的电脑出了出了一点小问题，在继续观看之前，你必须清楚下面两个事项：

1. 本教学仅限于系统部分，不包括硬件部分。
2. 如果使用破解软件出现了问题，最好的解决方法是重装；因为pj软件的修改所出现的错误很有可能是破解补丁的问题（如有必要，破解的大概思路也是需要了解的，这有助于解决为什么我就不可以）

下面开始正式内容（嫌麻烦直接跳第四步）：

在这里我们首先假定你是一个有一点电脑知识的小白，如果有什么名词不太清楚，可以百度一下；这是一个好习惯，因为随后我们用到搜索引擎的次数会比较多。

好，假设你现在遇到了一个问题。

## 第一步：回忆问题出现之前你的操作

问题不可能无缘无故的出现，如果出现了，那么大概率事出有因。回忆问题出现之前你使用了什么应用，删除了什么文件，或者是进行了一次windows更新。通过回忆你的操作有助于你搜索解决方案时更快地筛选出正确的解决方案。

## 第二步：判断你的问题

判断你的问题是一个重要的步骤，它能够帮助你正确地从搜索引擎得到答案；同时，更为详细的问题也有助于搜索引擎索引答案，比如“电脑蓝屏 0x000532542”比“电脑蓝屏”更有可能得到你想要地答案。

## 第三步：使用搜索引擎或站点

对于较为普通的问题，那么搜狗，百度大概率可以应付，比如“网络重置”，“DNS缓存清除”等等；

而对于比较复杂的问题，可以通过F搜这种搜索引擎，也可以通过知乎，简书等应用的站内搜索；

对于**有条件**的同学，建议使用 google 搜索；而如果中文互联网无法给出你满意的答案，那么可以借助 google 翻译获得你满意的答案。

顺便插一句，虽然用的是windows，但是尝试自己解决问题的时候最好知道自己做的是什么，病急乱投医只会让问题更加复杂

## 第四步：关于E管家

如果你解决关于你电脑的问题，可以把你的解决过程（包括系统环境，问题所在，解决方法）发给我们留存档案，方便更多同学收益；

如果你对于一到三不太熟悉，请直接联系E管家，让我们来解决你的烦恼。

我们的公众号QQ：2985586863

我们的服务群：728568691

PS：关于搜索部分的“有条件”，我们已经在开始给出了答案🤫🤫🤫

## 进阶部分

实际上，windows也是有崩溃日志的，可以通过使用WinDbg Preview进行在C:\Windows\Minidump找到dmp文件进行分析

一般来说，启动软件后会自动检测最新dmp[日志文件](https://www.zhihu.com/search?q=日志文件&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2584007159})，可以直接载入分析

载入完成后，窗口中会出现!analyze -v超链接按钮

可以得到类似这样的文件

```bash
||2:6: kd> !analyze -v
*******************************************************************************
*                                                                             *
*                        Bugcheck Analysis                                    *
*                                                                             *
*******************************************************************************

IRQL_NOT_LESS_OR_EQUAL (a)
An attempt was made to access a pageable (or completely invalid) address at an
interrupt request level (IRQL) that is too high.  This is usually
caused by drivers using improper addresses.
If a kernel debugger is available get the stack backtrace.
Arguments:
Arg1: ffffffafe367f6bc, memory referenced
Arg2: 0000000000000002, IRQL
Arg3: 0000000000000000, bitfield :
	bit 0 : value 0 = read operation, 1 = write operation
	bit 3 : value 0 = not an execute operation, 1 = execute operation (only on chips which support this level of status)
Arg4: fffff80765c4af31, address which referenced memory

Debugging Details:
------------------


KEY_VALUES_STRING: 1

    Key  : Analysis.CPU.mSec
    Value: 6437

    Key  : Analysis.DebugAnalysisManager
    Value: Create

    Key  : Analysis.Elapsed.mSec
    Value: 6546

    Key  : Analysis.Init.CPU.mSec
    Value: 3046

    Key  : Analysis.Init.Elapsed.mSec
    Value: 105711

    Key  : Analysis.Memory.CommitPeak.Mb
    Value: 122


FILE_IN_CAB:  102121-7515-01.dmp

DUMP_FILE_ATTRIBUTES: 0x8
  Kernel Generated Triage Dump

BUGCHECK_CODE:  a

BUGCHECK_P1: ffffffafe367f6bc

BUGCHECK_P2: 2

BUGCHECK_P3: 0

BUGCHECK_P4: fffff80765c4af31

READ_ADDRESS: fffff807666fb390: Unable to get MiVisibleState
Unable to get NonPagedPoolStart
Unable to get NonPagedPoolEnd
Unable to get PagedPoolStart
Unable to get PagedPoolEnd
unable to get nt!MmSpecialPagesInUse
 ffffffafe367f6bc 

BLACKBOXBSD: 1 (!blackboxbsd)


BLACKBOXNTFS: 1 (!blackboxntfs)


BLACKBOXPNP: 1 (!blackboxpnp)


BLACKBOXWINLOGON: 1

CUSTOMER_CRASH_COUNT:  1

PROCESS_NAME:  System

TRAP_FRAME:  fffffd83c147f430 -- (.trap 0xfffffd83c147f430)
NOTE: The trap frame does not contain all registers.
Some register values may be zeroed or incorrect.
rax=0000000000000000 rbx=0000000000000000 rcx=ffff9180c67d6180
rdx=0000000000000000 rsi=0000000000000000 rdi=0000000000000000
rip=fffff80765c4af31 rsp=fffffd83c147f5c0 rbp=0000000000000003
 r8=0000000000000000  r9=0000000000000000 r10=000000000000ffff
r11=ffff9180c67d6180 r12=0000000000000000 r13=0000000000000000
r14=0000000000000000 r15=0000000000000000
iopl=0         nv up ei ng nz na po nc
nt!KiDeferredReadySingleThread+0x3e1:
fffff807`65c4af31 0fbebbc3000000  movsx   edi,byte ptr [rbx+0C3h] ds:00000000`000000c3=??
Resetting default scope

STACK_TEXT:  
fffffd83`c147f2e8 fffff807`65e07d69     : 00000000`0000000a ffffffaf`e367f6bc 00000000`00000002 00000000`00000000 : nt!KeBugCheckEx
fffffd83`c147f2f0 fffff807`65e04069     : 00000000`00000000 00000000`0000ffff 00000000`00000000 fffff807`00000000 : nt!KiBugCheckDispatch+0x69
fffffd83`c147f430 fffff807`65c4af31     : 00000000`00000000 ffffa505`216d8080 00000000`00000008 00000000`00000000 : nt!KiPageFault+0x469
fffffd83`c147f5c0 fffff807`65c44c4d     : 00000000`0016708a fffffd83`c147fa10 00000000`00000080 ffff9180`c67d6180 : nt!KiDeferredReadySingleThread+0x3e1
fffffd83`c147f7b0 fffff807`65c45030     : ffff9180`c67d6180 00000000`00000000 ffffa505`219481f0 ffffa505`017db1b0 : nt!KiReadyThread+0x4d
fffffd83`c147f7e0 fffff807`65c06eed     : 00000000`00000000 00000000`00000000 00000000`00140001 00000000`0016708a : nt!KiProcessExpiredTimerList+0x290
fffffd83`c147f8d0 fffff807`65df99ae     : ffffffff`00000000 ffff9180`c67d6180 ffff9180`c67e1440 ffffa505`22607080 : nt!KiRetireDpcList+0x5dd
fffffd83`c147fb60 00000000`00000000     : fffffd83`c1480000 fffffd83`c1479000 00000000`00000000 00000000`00000000 : nt!KiIdleLoop+0x9e


SYMBOL_NAME:  nt!KiDeferredReadySingleThread+3e1

MODULE_NAME: nt

IMAGE_NAME:  ntkrnlmp.exe

IMAGE_VERSION:  10.0.19041.928

STACK_COMMAND:  .cxr; .ecxr ; kb

BUCKET_ID_FUNC_OFFSET:  3e1

FAILURE_BUCKET_ID:  AV_nt!KiDeferredReadySingleThread

OSPLATFORM_TYPE:  x64

OSNAME:  Windows 10

FAILURE_ID_HASH:  {626f59d5-90cc-ab48-85f0-60e170096c96}

Followup:     MachineOwner
---------
```

通过阅读最后一段的分析可以快速定位大概的问题方向

```bash
SYMBOL_NAME:  nt!KiDeferredReadySingleThread+3e1

MODULE_NAME: nt

IMAGE_NAME:  ntkrnlmp.exe

IMAGE_VERSION:  10.0.19041.928

STACK_COMMAND:  .cxr; .ecxr ; kb

BUCKET_ID_FUNC_OFFSET:  3e1

FAILURE_BUCKET_ID:  AV_nt!KiDeferredReadySingleThread

OSPLATFORM_TYPE:  x64

OSNAME:  Windows 10

FAILURE_ID_HASH:  {626f59d5-90cc-ab48-85f0-60e170096c96}

Followup:     MachineOwner
```

比如这里大概知道是win的NT内核出问题了，不过一般来说这只能当作一个问的解决方向，不能当作原因

如果要更精准地定位问题，可以通过检索PROCESS_NAME、MODULE_NAME 、 IMAGE_NAME和FAILURE BUCKET_ID等关键词定位问题

# PS：如果自己都对自己的问题不上心的话，就不要指望别人会上心了