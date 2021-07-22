# doppeltler自述文件

原文链接：[日语](http://utau2008.xrea.jp/2020/engine/doppeltler_readme.txt) [德语](http://utau2008.xrea.jp/2020/engine/doppeltler_readme_de.html) [英语](http://utau2008.xrea.jp/2020/engine/doppeltler_readme_en.txt) 作者：[飴屋P](https://twitter.com/ameyaP_)

---

Doppeltler重采样器ver0.XX测试

doppeltler32.exe (32位版)

doppeltler64.exe (64位版)

(2020.01.12 ~ )

## ■日志

### (2020.02.03) ver 0.08

* T flag变更为t flag。不好意思。

### (2020.01.23) ver 0.07

* 修正未设定G flag且无法读取FRQ文件时自动生成的FRQ存在异常的问题。

### (2020.01.19) ver 0.06

* 修正输出长度短于原音固定范围的情况下不能正确渲染的问题。

### (2020.01.18) ver 0.05

* T flag：实装了音程微调整。

* n flag：安静模式，假实装。

* 整理了部分源代码。音质等应当没有差异。

### (2020.01.15) ver0.04 Test

* 因为没法说频率表生成器“适配44100Hz以外”过于可怕所以换掉了。

* 顺便支持了24位PCM、32位浮点PCM。

### (2020.01.13) ver0.03 Test

* 因子音速度的计算式错误而进行修正。

* 向下修正气息成分的混合比例。

* 修正无信号部分中气息成分的计算生成异常数值的问题。

* 修正如果指定S（线程数）flag则峰值压缩器失效的问题。

### (2020.01.13) ver0.02 Test

* 修正音源为立体声时，无法正确渲染的问题。

* 调整B flag和a flag相关的默认值和敏感度。

### (2020.01.12) ver0.01 Test

* 发布

## ■使用方法

请设定为UTAU的工具2（重采样）使用。

## ■解说

* resampler系引擎（从零开始新作）

* 支持多线程、64位

* 支持g, B, N, P flag。

* 独有flag：

      a 噪声成分的高频强调率（默认a100）

      R 前后反转

      S 线程数

      i  后部固定范围的长度（毫秒）

      v 后部固定范围的子音速度

* 其他

  * 线程数默认设定为机器的逻辑核心数。

  * 可以使用采样率44100Hz以外的文件作为输入文件，

  * 既然在这里这么写了差不多就是支持了。

  * 可用“前方/后部”的形式来指定第4参数的子音速度，但UTAU本体暂未对应因此暂无很大意义。

  * 可用“前方/后部”的形式来指定第8参数的固定范围，但UTAU本体暂未对应因此暂无很大意义。

  * 若将环境变量lamepath设置为lame.exe的路径，.mp3文件也可以读取。

  * 若将环境变量flacpath设置为flac.exe的路径，.flac文件也可以读取。

  * 若将环境变量oggpath设置为oggdec.exe的路径，.ogg文件也可以读取。

  * 对现行的UTAU如在oto.ini中指定.wav以外的文件格式会向文件名附加.wav后传递给引擎，因此为了与此对应此引擎将".mp3.wav"、".flac.wav"、".ogg.wav”分别作为".mp3"、".flac"、".ogg"处理。

## ■免责事项

作者对使用此软件发生的任何损害不负有任何责任。

并且作者不负有任何版本升级或问题修正的义务。

## ■推特

(twitter.com/ameyaP_)[twitter.com/ameyaP_]

版权所有 (C) 2020 Ameya/Ayame 保留所有权利。
