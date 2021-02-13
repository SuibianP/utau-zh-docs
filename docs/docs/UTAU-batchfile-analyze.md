# UTAU 渲染脚本

[原文链接（日语）](https://ch.nicovideo.jp/delta_kimigatame/blomaga/ar429837) 原作者：[delta_kuro](https://twitter.com/delta_kuro)

---

## 声明
本文依据个人分析所写，因此可能存在少量不严谨。另外，翻译过程中补充了一些内容。

- [UTAU 渲染脚本](#utau-渲染脚本)
  - [声明](#声明)
  - [渲染脚本的位置](#渲染脚本的位置)
    - [临时文件夹](#临时文件夹)
    - [脚本文件](#脚本文件)
  - [渲染脚本的作用](#渲染脚本的作用)
    - [单核处理](#单核处理)
      - [temp.bat](#tempbat)
      - [temp_helper.bat](#temp_helperbat)
    - [多核处理](#多核处理)
      - [temp.bat](#tempbat-1)
      - [其他批处理文件](#其他批处理文件)
  - [渲染脚本的内容](#渲染脚本的内容)
    - [temp.bat 页首](#tempbat-页首)
    - [temp.bat 正文](#tempbat-正文)
      - [对于休止符](#对于休止符)
      - [对于除休止符以外的音符](#对于除休止符以外的音符)
    - [temp_helper.bat 内容](#temp_helperbat-内容)
      - [[与ust无关的指令]](#与ust无关的指令)
    - [temp.bat 页脚](#tempbat-页脚)
      - [[与ust无关的指令]](#与ust无关的指令-1)


## 渲染脚本的位置
### 临时文件夹
一般地，UTAU 启动后，会在**系统环境变量为 TEMP 的路径下**创建后台工作目录 `utauX`（X为正整数，由于utau可以多开，按启动顺序依次递增），用来存放合成脚本文件。

一般是这样一个文件夹：`C:\user\<user名>\AppData\Local\Temp\utau1\`

### 脚本文件

一般会产生以下两个文件。
+ temp.bat
+ temp_helper.bat

如果使用了 UTAU 的多核渲染，那么不会产生 helper，会产生以下文件，具体数量与线程数相同。
+ temp.bat
+ temp1.bat
+ temp2.bat
+ temp3.bat
+ ...


## 渲染脚本的作用

### 单核处理
#### temp.bat
+ 从UTAU直接启动
+ 将各种参数存在批处理的变量中
+ 对于休止符，直接启动wavtool

#### temp_helper.bat
+ 从temp.bat中启动
+ 将temp.bat中设定的参数发送给resampler和wavtool

### 多核处理
#### temp.bat
+ 一次性调用合成器，在最后启动。

#### 其他批处理文件
+ 在合成开始后同时启动重采样器，并行进程。
+ 所有进程结束后启动 `temp.bat`。

**本文只讨论单核渲染的批处理文件。**

## 渲染脚本的内容
### temp.bat 页首
**@rem project**  
工程文件名

**@set loadmodule**  
模块的设定？似乎一直是空白的  

**@set tempo**  
工程曲速

**@set samples**  
采样频率

**@set oto**  
音源文件夹的完整路径

**@set tool**  
合成引擎 (wavtool)的完整路径

**@set resamp**  
重采样引擎(resampler) 的完整路径

**@set output**  
temp.wav的相对路径

**@set helper**  
temp_helper.bat的路径

**@set cachedir**  
缓存文件夹的路径

**@set flag**  
全局Flags

**@set env**  
默认音量包络

**@set stp**  
定义变量stp

**@del "%output%" 2>nul**  
如果temp.wav存在，则将它删掉  
"2>nul"指的是不输出错误信息

**@mkdir "%cachedir%" 2>nul**  
创建缓存文件夹

### temp.bat 正文
#### 对于休止符
**@"%tool%" "%output%""%oto%\R.wav" 0 480@120+0.0 0 0**  
直接调用wavtool
> + 第一个参数：输出文件
> + 第二个参数：在原音设定文件夹下的R.wav`（似乎用这个来识别休止符）`
> + 第三个参数：stp`（休止符为0）`
> + 第四个参数：输出的实际长度  
格式：音长@tempo+校正值  
校正值：`(先行发声) - (后一个音符的先行发声) + (后一个音符的重叠)`  
> + 第五个参数（以及后面一堆）：音量包络`（对于休止符:0 0）`

#### 对于除休止符以外的音符
**@set params=100 0 !120 AA#5#　`[必填]`**  
设定音调参数
> + 第一个参数：音量
> + 第二个参数：移调
> + 第三个参数：曲速`（格式：!tempo） `
> + 第四个参数：音高曲线字符串

**@set flag="g5"**  
设定 Flags
> + 一般是全局flags在后，音符flags在前
> + 出现同一个参数时，只有第一个有效
> + 当出现 e 或者 E 时，在前面添加反斜杠

**@set env=0 5 35 0 100 100 0 70 0 10 100 `[必填]`**  
音量包络`（除了休止符，有三种格式，任选其一）`
> + p1 p2 p3 v1 v2 v3 v4 ove
> + p1 p2 p3 v1 v2 v3 v4 ove p4
> + p1 p2 p3 v1 v2 v3 v4 ove p4 p5 v5

**@set stp=150**  
设定 stp 数值

**@set vel=100`【必填】`**  
辅音速度

**@set temp="%cachedir%\1_あ_C4_OkgOOW.wav"`【必填】`**  
缓存文件的路径
> + 文件名是 `[ust音符序号]_[歌词]_[音阶]_[六位英文数字].wav`
> + 歌词中的`半角空格`会被替换为`“+”`，`“*”`替换为`“$”`，`“?”`替换为`“=”`，`正反斜杠`替换为`下划线`，其他文件名敏感符号直接忽略。
> + 乱码后缀意义不明（可能是随机生成）
> + 当对同一个音符进行操作后，通过改变文件名来区分缓存，这种方法比较好。

**@echo ##########------------------------------(1/4)`【必填】`**  
显示合成进度

**@call %helper% "%oto%\_おおうえあ.wav" C4 480@120+70.0 30 3170.884 600 50.0 -1169.19 0`【必填】`**  
temp_helper.bat的启动指令
> + 第1个参数：采样的文件名("%oto%\あ.wav")
> + 第2个参数：渲染目标音阶名(C4)
> + 第3个参数：音符长度(与休止符相同)(480@120+70.0)
> + 第4个参数：先行发声(30)
> + 第5个参数：偏移(3170.884)
> + 第6个参数：resampler输出的声音长度(600)  
`与wavtool结合前，没有经过裁剪直接输出的长度`  
`(第3个参数换算为ms后的长度+stp)/50，四舍五入以后再乘以50`
> + 第7个参数 固定范围(50.0)
> + 第8个参数 右侧空白(-1169.19)
> + 第9个参数 序号(0)  
`多线程合成时这个参数如果错误可能引起问题`

### temp_helper.bat 内容
#### [与ust无关的指令]

**@if exist %temp%goto A**  
如果存在缓存文件，那么跳过重采样步骤

**@if exist"%cachedir%\%9_*.wav" del "%cachedir%\%9_*.wav"**  
如果有相同序号但不同名称的文件，则删除

**@"%resamp%"%1 %temp% %2 %vel% %flag% %5 %6 %7 %8 %params%**  
启动重采样引擎（之前已经讲过）

**:A**  
提供给第一行跳跃的标签

**@"%tool%""%output%" %temp% %stp% %3 %env%**  
启动合成引擎（与休止符一样）

### temp.bat 页脚
#### [与ust无关的指令]

**@if not exist"%output%.whd" goto E**  
**@if not exist"%output%.dat" goto E**  
检查是否生成了 temp.whd 和 temp.dat。如果没有，跳过以下步骤。
> 当temp_helper.bat正在被调用时，还没有生成temp.wav，temp.whd与temp.dat已经生成了。

**copy /Y"%output%.whd" /B + "%output%.dat" /B "%output%"**  
将 temp.whd 和 temp.dat 合并为 temp.wav。
> #whd可能是 `wav head` 的意思。

**del "%output%.whd"**  
**del"%output%.dat"**  
删除temp.whd与temp.dat。

**:E**  
跳过合并与删除过程的标签。
