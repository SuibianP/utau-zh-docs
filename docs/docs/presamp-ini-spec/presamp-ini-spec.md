# presamp.ini说明文件

[原文链接（日语）](https://ch.nicovideo.jp/delta_kimigatame/blomaga/ar483589) 作者：[デルタ](https://www.nicovideo.jp/user/17751382)

---

## 注意事项

因为用来编辑预设的编辑器还没有做，所以写了这篇面向“无论如何（都要自己搞presamp.ini）”的人的十分硬核的文档。

哪怕是在正确照着本文档使用的情况下，也不保证没有bug。

如果删除了presamp.ini的话，下次启动时程序将按照默认值重新生成一个。

若文件里有空行的话会读取失败。（关于这一点，ver1.7中大概有所改善）

## 版本适用范围

- ver1.2:

    presamp ver 0.64起

    predit ver 1.20起

- ver1.3:

    presamp ver 0.830起

    predit ver1.30起

- ver1.4:

    presamp ver0.841起

    predit ver1.40起

- ver1.5:

    presamp ver0.850起

    predit ver1.50起

- ver1.5(面向英语音源做了扩展的版本，仅有音源侧预设):

    presamp ver0.865起

- ver1.6:

    presamp ver0.866起

    predit ver1.60起

- ver1.7:

    presamp ver0.890起

    predit ver1.70起

## 存放路径

- ～presamp ver 0.714为止

    %appdata%\UTAU\presamp\presamp.ini

- ～presamp ver 0.801为止

    %appdata%\UTAU\presamp_DV\presamp.ini

- ～presamp ver 0.845为止

    %appdata%\UTAU\presamp_DV08\presamp.ini

- ～presamp ver 0.871为止

    %appdata%\UTAU\presamp_DV0850\presamp.ini

- 目前版本

    %appdata%\UTAU\presamp_DV0890\presamp.ini

并且，如果在音源文件夹的根目录下（与prefix.map等在同一位置）放置presamp.ini的话，则可以对该音源独立设定某些设置项。

## 格式

[条目]
参数
参数
参数
[条目]
参数
…

这样按照顺序设定下去。

### [VERSION]

默认值：

    1.7

该项指定了本presamp.ini的版本。

目前的可用值：**1.2**、**1.3**、**1.4**、**1.5**、**1.6**、**1.7**。

0.830DV起为1.3
0.841DV起为1.4
0.850DV起为1.5
0.866DV起为1.6
0.890DV起为1.7。

### [LOCALE]

默认值：

    0

该项指定predit使用的语言。

**0为日语，1为英语，2为韩语，3为繁体中文。**

错误信息的话只有日语版本。
有将来从外部文件读取语言设置的预定。

### [RESAMP]

默认值是

    C:\Program Files\UTAU\resampler.exe

    C:\Program Files\UTAU\resampler.exe

指定重采样器(resampler)的绝对路径。

**第1行是默认（没有flag时）被使用的标准引擎；**

**第2行是用flag“/1”来使用的引擎；**

**第3行是用flag“/2”来使用的引擎；**

**…**

请按照如上说明的方式进行设定。

虽然没有仔细测试过，但理论上可以在之后追加任意多行。

指定的exe不存在的情况下，将使用UTAU本体文件夹下的resampler.exe。

文件中最多只指定到/2，ust文件中却设定了/4之类的flag的这种情况下，程序将使用标准引擎（第1行）。

### [TOOL]

默认值：

    C:\Program Files\UTAU\wavtoolex.exe

**该项指定wav结合器的绝对路径。**

指定的exe不存在的情况下，将使用UTAU本体文件夹下的wavtool.exe。

### [VOWEL]

默认值：

    a=あ=ぁ,あ,か,が,さ,ざ,た,だ,な,は,ば,ぱ,ま,ゃ,や,ら,わ,ァ,ア,カ,ガ,サ,ザ,タ,ダ,ナ,ハ,バ,パ,マ,ャ,ヤ,ラ,ワ=100
    e=え=ぇ,え,け,げ,せ,ぜ,て,で,ね,へ,べ,ぺ,め,れ,ゑ,ェ,エ,ケ,ゲ,セ,ゼ,テ,デ,ネ,ヘ,ベ,ペ,メ,レ,ヱ=100
    i=い=ぃ,い,き,ぎ,し,じ,ち,ぢ,に,ひ,び,ぴ,み,り,ゐ,ィ,イ,キ,ギ,シ,ジ,チ,ヂ,ニ,ヒ,ビ,ピ,ミ,リ,ヰ=100
    o=お=ぉ,お,こ,ご,そ,ぞ,と,ど,の,ほ,ぼ,ぽ,も,ょ,よ,ろ,を,ォ,オ,コ,ゴ,ソ,ゾ,ト,ド,ノ,ホ,ボ,ポ,モ,ョ,ヨ,ロ,ヲ=100
    n=ん=ん=100
    u=う=ぅ,う,く,ぐ,す,ず,つ,づ,ぬ,ふ,ぶ,ぷ,む,ゅ,ゆ,る,ゥ,ウ,ク,グ,ス,ズ,ツ,ヅ,ヌ,フ,ブ,プ,ム,ュ,ユ,ル,ヴ=100
    N=ン=ン=100

该项是关于元音的设定。

该设置项可以在音源侧的presamp.ini中进行设定。

格式是，

**v=V=CV,CV,CV…=vol**

在前一音符末尾含有上述CV时，其后的VC或者VCV音符的歌词中的前面的元音部分会使用指定的v。

CV目前限定为1字符。[^VowelCVOne]

[^VowelCVOne]:润色注：应用中此处的CV可以使用多个字符。

<p style="color: grey">V是为了调用长采样或语尾息而设计的参数，现在暂未被使用。</p>

<p style="color: grey">是为了在歌词为「a あ息R」「あー」这样的形式时，程序能够写出其中「あ」而设置。</p>

<p style="color: grey">vol是想要做出根据音量的倍率对每一母音施加音量变化的功能而加入的参数，现在暂未被使用。</p>

### [CONSONANT]

默认值：

    ch=ch,ち,ちぇ,ちゃ,ちゅ,ちょ=1
    gy=gy,ぎ,ぎぇ,ぎゃ,ぎゅ,ぎょ=1
    ts=ts,つ,つぁ,つぃ,つぇ,つぉ=1
    ty=ty,てぃ,てぇ,てゃ,てゅ,てょ=1
    py=py,ぴ,ぴぇ,ぴゃ,ぴゅ,ぴょ=1
    ry=ry,り,りぇ,りゃ,りゅ,りょ=1
    ny=ny,に,にぇ,にゃ,にゅ,にょ=0
    r=r,ら,る,れ,ろ=1
    hy=hy,ひ,ひぇ,ひゃ,ひゅ,ひょ=0
    dy=dy,でぃ,でぇ,でゃ,でゅ,でょ=1
    by=by,び,びぇ,びゃ,びゅ,びょ=1
    b=b,ば,ぶ,べ,ぼ=1
    d=d,だ,で,ど,どぅ=1
    g=g,が,ぐ,げ,ご=1
    f=f,ふ,ふぁ,ふぃ,ふぇ,ふぉ=0
    h=h,は,へ,ほ=0
    k=k,か,く,け,こ=1
    j=j,じ,じぇ,じゃ,じゅ,じょ=0
    m=m,ま,む,め,も=0
    n=n,な,ぬ,ね,の=0
    p=p,ぱ,ぷ,ぺ,ぽ=1
    s=s,さ,す,すぃ,せ,そ=0
    sh=sh,し,しぇ,しゃ,しゅ,しょ=0
    t=t,た,て,と,とぅ=1
    w=w,うぃ,うぅ,うぇ,うぉ,わ,を=0
    v=v,ヴ,ヴぁ,ヴぃ,ヴぅ,ヴぇ,ヴぉ=0
    y=y,いぃ,いぇ,や,ゆ,よ,ゐ,ゑ=0
    ky=ky,き,きぇ,きゃ,きゅ,きょ=1
    z=z,ざ,ず,ずぃ,ぜ,ぞ=0
    my=my,み,みぇ,みゃ,みゅ,みょ=0

该项是关于辅音的设定。

该设置项可以在音源侧的presamp.ini中进行设定。

格式是

**c=CV,CV,…=\[crossfade flag]**(=\[VCLENGTH])

在调用VC时，如果后面的音符歌词对应上述CV，则当前VC音符的辅音部分使用c。

和对元音的设置中说明的不同，此处对CV的比较是完全匹配。

请将[crossfade flag]设定为0或1。

此处设定为1则意味着VC音符与CV音符不会进行交叉渐变。

在“声音消失”或者“对爆破音进行交叉渐变”等“交叉渐变反而坏处比较大”的情况下，请将本设定设置为1。

*ver1.4起*

[VClength]写不写都可以。不写的情况下本项为0。

本项为0时，程序会从CV部分的原音设定中取得VC部的长度，设定为1时则从VC部的原音设定中取得VC部长度。

### [PRIORITY]

默认值：

    k,ky,g,gy,t,ty,d,dy,ch,ts,b,by,p,py,r,ry

告知程序在连续音与CVVC原音设定项同时存在时，是否优先使用CVVC的设置。

该设置项可以在音源侧的presamp.ini中进行设定。

格式为

**CV,CV,CV…**

如果[あ][k]变换为[あ][a k]后，k的声音并没有被发出，则会希望将其变换为[あ][a k][k]。

本设置项是在类似上述的状况下使用的。

此处CV需要完全一致才能匹配。

因为不能变换成连续音，在无法CVVC化的情况下会以单独音输出。

在作为音源侧设定时，可以用该设定来迂回解决连续音部分出错的问题。但由于会对所有接在前面的元音应用该设置，所以对该设置的使用有些微妙。

例子：如果为了迂回解决[o も]出错的问题，[も]被设定在本项中，那么[a も]也会被程序试着CVVC化（若无[a m]则会被单独音化）

### [REPLACE]

默认值：

    由于很多所以省略。

    (是将罗马字变换为平假名，[を]变换为[お]的规则。)

该项是指定程序以何种规则进行歌词替换。

该设置项可以在音源侧的presamp.ini中进行设定。

格式为

**CV1=CV2**

使用时，程序会把CV1替换为CV2。

使用歌词为罗马音的ust；方便海外用户使用；防止漏录的音素缺音；对类似[ホモくれ](https://dic.nicovideo.jp/a/%E3%83%9B%E3%83%A2%E3%81%8F%E3%82%8C%E9%9F%B3%E6%BA%90)的音源进行音符变换（可以作为歌词元音化插件的替代）。

该设置项可以用于这些情况。

### [ALIAS]

默认值：

    VCV=%v%%VCVPAD%%CV%
    BEGINING_CV=-%VCVPAD%%CV%
    CROSS_CV=*%VCVPAD%%CV%
    VC=%v%%vcpad%%c%,%c%%vcpad%%c%
    CV=%CV%,%c%%V%
    C=%c%
    LONG_V=%V%ー
    VCPAD=
    VCVPAD=
    ENDING1=%v%%VCPAD%R
    ENDING2=-

版本 ver1.5 起该设置项存在，<span style="text-decoration: line-through; color: red">但目前还不被使用。</span>

版本 ver1.7 中已支持该设置项。

该项设置指定不同情况下各别名的格式。

程序将从[VOWEL]段中获取%v%,%V%，从[CONSONANT]段中获取%c%,%CV%。

该项设置中对于 VCPAD,ENDING1,ENDING2 的设定，如果已经在后述的[VCPAD][ENDTYPE1][ENDTYPE2]中设置过了，那么在那边的设置会被优先考虑。

该设置项可以在音源侧的presamp.ini中进行设定。

### [PRE]

默认值：

    （空）

**格式**

prefix1
prefix2
…

该项设置自 版本 ver1.5 起可用。

是为了解析类似[A3あ]带有前缀的别名的原音设定条目而设的设置项。
请在每一行分别指定一种前缀。

该设置项可以在音源侧的presamp.ini中进行设定。

### [SU]

默认值：

    %num%%append%%pitch%

- *重复字符：%num%(如[あ2]中的2)*

- *追加音源名：%append%(如[あ強]中的強)*

- *音高名：%pitch%(如[あG4]中的G4)*

指定解析上述文本时的顺序。

该项设置自 版本 ver1.5 起可用。

目前版本中，**输出的别名固定为默认顺序**。请等待之后的更新。

该设置项可以在音源侧的presamp.ini中进行设定。

### [NUM]、[APPEND]、[PITCH]

该项设置自 版本 ver1.5 起可用。

在解析中，以上设置分别指定重复序号、append名和音高名称的可能范围。

**特殊命令:**

#### @UNDERBAR@

被该命令修饰的别名在匹配时将包含前面（如果有）的下划线。

##### 无@UNDERBAR@的情况

[あ_G4]→匹配其中的G4

##### 有@UNDERBAR@的情况

[あ_G4]→匹配其中的_G4

#### @NOREPEAT@

被该命令修饰的别名在匹配时不会允许重复。

##### 无@NOREPEAT@的情况

[あ22]→匹配其中的22

##### 有@NOREPEAT@的情况

[あ22]→匹配其中的2

该设置项可以在音源侧的presamp.ini中进行设定。


### [ALIAS_PRIORITY]<br/>[ALIAS_PRIORITY_DIFAPPEND]<br/>[ALIAS_PRIORITY_DIFPITCH]]
**preset ver1.7起可用。**

分别通常，append名不同，同一append名音高不同时指定别名的优先顺序。

默认值：\
VCV\
CVVC\
CROSS_CV\
CV\
BEGINING_CV\

但是append名不同，音高名不同为VCV与CVVC的顺序逆转。\
关于各种别名分别的含义按[APPEND]的项目中指定的别名。


**可以在音源侧预设设定**


### [SPLIT]
默认值：1

**可以在音源侧预设设定\
preset ver1.5起可用**

是关于音符自动分割功能的设定。\
若设为0则变为不进行音符自动分割。



### [MUSTVC]
默认值：0

**可以在音源侧预设设定\
preset ver1.6起可用**

CVVC化时若CV的元音部分长度低于20ms，VC不生成（presamp说明）\
若将此项目设为1，即使在CV极短的情况下仍然一定要生成VC。\
此情况下的VC长不为原音长，而变为与CV长相同的长度。

对英语或中文等含有双元音的音源库请设为1.




### [CFLAGS]
默认值：p0

单个辅音或双重辅音时自动付与flag。\
**可以在音源侧预设设定**

**preset ver1.3起可用**\
若preset ver1.2中记述则presamp会崩溃


辅音仅识别[consonant]c=CV,CV...=FLAG中指定的C的别名。



### [VCLENGTH]
默认值：0

**preset ver1.4起可用**\
若preset ver1.2中记述则presamp会崩溃

选择取得CVVC的VC音符长的来源。\
0的情况下为从CV的原音设定取得，\
1的情况下为从VC的原音设定取得。

**并且在此指定1的情况下，consonant下每一子音分别的vclength设定将被无视。**

### [ENDTYPE1]<br/>[ENDTYPE2]
默认值：\
无

是语尾音素的格式设定。仅在[ENDFLAG]中指定3的情况下可用。\
**仅在ver1.5英语音源支持版的音源侧预设可以指定**

转换休止符时的语尾音规则指定[ENDTYPE1]，\
末尾补足别名时的语尾音规则指定[ENDTYPE2]。\
格式与[ENDTYPE]相同。

**ver1.7后与APPEND被统合，但作为向前兼容性被留下来。**

### [ENDTYPE]
默认值：\
%v% R

是语尾音素的格式设定。\
**可以在音源侧预设指定**

**preset ver1.3起可用**\
根据下述ENDFLAG的值改变行为。


**ver1.7后与APPEND被统合，但作为向前兼容性被留下来。**\
%v%：a,i,u等等vowel中设定的原音\
%V%：あ,い,う等等vowel中设定的代表CV\
除此以外的字符预计会被不变输出。


[VCPAD]
默认值：

（半角空格）


**是可以在音源侧预设设定的项目。**

因为好久之前收到了“CV是[ka]之类的那么VC也想[ak]这样输入！”的请求而实装的功能。\
VC的格式写作\
%v%%vcpad%%c%

主要面向海外。\
突然想把VC的别名命名为[a+k]之类的人也能使用了，但未经详细测试。

**由于若有空格行presamp会崩溃所以现在完全用不上**

**ver1.7后与APPEND被统合，但作为向前兼容性被留下来。**


### [ENDFLAG]
默认值：
1

ver1.3起中默认值为0

**ver1.3起可以使用**\
是是否自动使用语尾音素的设定。\
**是可以在音源侧预设设定的项目。**


0为自动语尾音关闭

1为休止符转换为语尾音的类型设定\
ex)\
音符为[あ][R]，endtype为%v% R的话\
[あ]转换为[a R]

2为语尾音符变化的类型设定\
ex)\
音符为[a k][R]，endtype为-的情况\
转换为[a k-][R]

**仅为ver1.5英语音源扩展版起的音源侧预设**\
若设定为3则自动判断使用1，2哪种行为。


[BATNUM]\
默认值：\
16

是presamp的高速化设定。\
数字为1～CPU核心数的2倍

无论指定多大的数字都被限制为核心数2倍。
