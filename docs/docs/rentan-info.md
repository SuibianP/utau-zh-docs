# 整体录制单独音（连单风单独音）的解说

[原文链接（日语）](https://ch.nicovideo.jp/tatsu3/blomaga/ar491077) 作者：[巽](https://www.nicovideo.jp/user/2828687)

我擅自进行了一些修改，希望能够提高可读性。

---

整体录制单独音（连单风单独音），就是指用连续音音源收录的引导BGM，在一个wav文件中整体录制数个连续的音节的单独音录音方案。

## 录音表

录音表在[这里](https://bowlroll.net/file/110302)配布（外部链接）

## 以此方式收录的音源

* 豹歌ショウ

* 茂名歌ころ

* 愛歌カナ Ver.3.0

## 优势

* 由于使用了引导BGM，预计可缩短录音时间并统一声质

* 比起单独音的手动录音，对子音被切断的担忧更少

* 能够在短时间内录音

## 录音方法

使用[OREMO](https://osdn.net/users/nwp8861/pf/OREMO/files/)的情况下，请用使用引导BGM的自动录音模式录音。

虽然能够用来录制八字的连续音用引导BGM应该就可以录音，但如果就按照配布的样子来会存在不能容纳录音音声的可能性，因此请变更引导BGM的设定文件。

设定文件可以打开引导BGM同捆的txt直接编辑

而OREMO的同捆软件“KOREDE”也能够作成引导BGM的设定文件。

KOREDE的使用方法请参考[此文章](http://nwp8861.blog92.fc2.com/blog-entry-282.html)。

追记

已有对应10字的引导BGM被配布。请配合那个一起使用。

[ガイドBGM - UTAU音源制作wiki](https://w.atwiki.jp/vbmaker/pages/26.html#id_a0402944)

## 发声方法

![发声方法](https://bmimg.nicovideo.jp/image/ch2563977/54635/ecbdb74979c2d843263652417bbaf91dfd328b12.png)

如图所示

“あー（休止）いー（休止）うー（休止）えー（休止）おー”

请像这样发声。

## 原音设定

使用[setParam](https://osdn.net/users/nwp8861/pf/setParam/files/)来生成原音设定的基础。

通过“连续音用oto.ini生成”来生成oto.ini。

连续音用oto.ini生成的步骤请参考[此文章](http://ch.nicovideo.jp/tatsu3/blomaga/ar441502)。

setParam ver.3.0-b140204以后的情况下，请勾选“作为一字分的休止符”进行参数生成。

![作为一字分的休止符](https://bmimg.nicovideo.jp/image/ch2563977/187214/44777f8e4bfd91d4b5fdd8a3c1003bd6bf126ab4.png)

这之后是有两种方法的，因此请进行容易做的一种。

### 方法①

生成参数后，因为有必要删除别名的“- ”

打开“工具”→“批量变更别名”

变换规则为“%a”，当前别名开头删除的文字数输入“2”并执行。

![批量变更别名](https://bmimg.nicovideo.jp/image/ch2563977/187218/2ac86f946470bf1f6ca072dcd40abc9c0194ae2d.png)

之后和单独音同样进行原音设定就OK。

### 方法②

使用叫做“[连单桑](https://bowlroll.net/file/102992)”的setParam插件。

预先导入好插件后，

选择“工具”→“原音参数的自动推定”→“插件”→“连单桑”，会打开连单桑的窗口。

![连单桑](https://bmimg.nicovideo.jp/image/ch2563977/187219/31a39ee1ef3b0194631fcef4404da38ec6cca7a7.png)

按如图所示设定连单桑。

执行后会从别名中去除“- ”，从连续音设定为与单独音接近的原音参数。

数值从另外的文件指定由于某些数值被设定在这里会变得有必要手动调整。

## 最后的

用同一录音表录音多种音源的情况下，如果设定其中一种后复制转用oto.ini的话设定会变得更轻松。
