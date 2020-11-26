# 关于setParam插件的开发方法

[原文链接（日语）](http://nwp8861.blog92.fc2.com/blog-entry-365.html) 作者：[nwp8861](http://nwp8861.blog92.fc2.com/)

----------------------------------------------------------

setParam 自动推定用插件开发指南

（version 3.0-b150712以后用）

2013/01/23 作成

2015/07/12 修订

----------------------------------------------------------

此文档为UTAU音源原音设定软件setParam的插件开发相关信息说明。

setParam的插件于ver.2.0-b130130被开发，此后于ver.3.0-b150712进行变更，以下三种类型的插件得以被制作。

　(a) 更新全部数据的插件（自动推定等）
 
　(b) 改变波形窗当前显示数据的插件
 
　(c) 改变参数一览表中多选的数据的插件

----------------------------------------------------------
## 插件安装路径

在“setParam.exe所在文件夹\plugins\”下创建文件夹，并在其中放置所开发的插件程序及后述的plugin.txt。

## 插件执行过程

插件执行按照如下顺序被处理。

1) setParam创建名为“setParam.exe所在\plugins\inParam.txt”的文本文件。

此文件内容如下所示。

```
第1行：要处理的音源文件夹的绝对路径。
第2行：setParam〜插件间数据传递用原音设定文件的绝对路径。
第3行：F0的提取间隔（单位=秒）。
第4行：响度的提取间隔（单位=秒）。
第5行：setParam的波形窗口中当前显示的数据的行号。
　以文件首行作为1进行计数。
第6行：setParam的参数一览表中当前选择的单元格的行号。
　多选时以逗号分隔列举全部行号。
```

inParam.txt的内容示例如下所示。

```
C:/voicebank/example
C:/voicebank/example/oto-autoEstimation.ini
0.01
0.02
5
3,4,5
```

第1、2行的路径格式为与Windows不同的Tcl/Tk格式。若Windows记为“C:\foo\bar”的文件夹，Tcl/Tk则记作“C:/foo/bar”（即日元符号替换为斜杠）。

译注：在日语编码中反斜杠（\）被映射为日元符号（¥），因而其他编码中应为“反斜杠替换为斜杠”。

2) setParam将现有原音设定保存至inParam.txt第二行所示文件夹。

3) 插件的设定文件（plugin.txt）中指定有“needF0=1”时对各wav文件的F0文件进行输出，指定有“needPower=1”时对各wav文件的响度文件进行输出。

4) 启动插件。插件读取inParam.txt等，进行原音设定，用结果覆盖inParam.txt第二行所示文件并终止退出。

5) setParam读取inParam.txt第二行所示文件。

　 此时，取得数据的方式（全部取得或仅取得相应行）随插件设定文件（plugin.txt）中指定的插件类型而改变。

## 关于plugin.txt

setParam启动时会搜索“setParam.exe所在文件夹\plugins\*\plugin.txt”并添加到菜单中。

plugin.txt中记有以下项目，必须项目为“name=”“execute=”，其他项目请在必要时指定（不需要的行请全部省略）。

```
　　name=単独音自動推定：hogehoge ← setParam中显示的插件名
　　execute=hogehoge.exe ← 插件的执行命令
　　argv=-a %S -b %n ← 插件执行时传递的参数（详见后文）
　　needF0=0 ← 若需要setParam的F0数据则设置为1（详见后文）
　　F0unit=semitone ← 指定F0值的单位（Hz或semitone）
　　needPower=0 ← 若需要setParam的响度数据则设置为1（详见后文）
　　edit=all ← 插件的操作类型（详见后文）
```

## 关于plugin.txt的argv

在plugin.txt的“argv=”中指定的字符串会在运行时作为参数传递给插件。若在参数字符串中指定以下记号，执行时会传递展开后的对应字符串。

```
　　%S ← 要处理的音源文件夹的绝对路径。
　　%s ← 要处理的音源文件夹的绝对路径。
　　 　 保留了与旧版插件的向后兼容性，路径含有空格时字符串会被分解。
　　%R ← setParam〜插件间数据传递用原音设定文件的绝对路径。
　　%r ← setParam〜插件间数据传递用原音设定文件的绝对路径。
　　 　 保留了与旧版插件的向后兼容性，路径含有空格时字符串会被分解。
　　%f ← F0的提取间隔（单位=秒）。
　　%p ← 响度的提取间隔（单位=秒）。
　　%n ← 当前显示的波形为数据中转原音设定文件中的行数。
　　%l ← 当前选择的各数据为中转原音设定文件中的行数列表。
　　 　 例如若选中第2行〜第4行即为“2,3,4”的逗号分隔字符串。
```

并且，%S、%s、%R、%r的路径格式为与Windows不同的Tcl/Tk格式。若Windows记为“C:\foo\bar”的文件夹，Tcl/Tk则记作“C:/foo/bar”（即日元符号替换为斜杠）。

译注：在日语编码中反斜杠（\）被映射为日元符号（¥），因而其他编码中应为“反斜杠替换为斜杠”。

以及音源路径名如“c:/Program Files/”一样含有空格的情况下，在%s和%r中，会将"c:/Program"与"Files/"两个字符串分开处理，需要加以注意。因而尽可能使用%S和%R读取inParam.txt的第1、2行大概会更好。

## 关于plugin.txt的needF0、F0unit

如在plugin.txt中写入“needF0=1”，setParam会在插件执行时提取各音频文件的F0数据并输出至文本文件。

・F0文件名：是wav文件的扩展名替换为“.setParam-f0”。（例：“あ.wav”的F0文件为“あ.setParam-f0”）

・F0文件的保存位置：与wav文件相同的文件夹。

・文件内容：
　　F0值一行一数据的向下连续。
  
　　F0值的単位可以使用plugin.txt的"F0unit"指定。
  
　　其他F0提取用参数则使用setParam显示F0时所用的值。

## 关于plugin.txt的needPower

如在plugin.txt中写入“needPower=1”，setParam会在插件执行时提取各音频文件的响度数据并输出至文本文件。

・响度文件名：是wav文件的扩展名替换为“.setParam-power”。（例：“あ.wav”的响度文件为“あ.setParam-power”）

・响度文件的保存位置：与wav文件相同的文件夹。

・文件内容：
　　响度值一行一数据的向下连续。
  
　　响度提取用参数使用setParam显示响度时所用的值。
  
　　值的単位为dB。

## 关于plugin.txt的edit

按开发的插件的行为指定以下任一对应项。

・若为仅更新setParam波形窗中当前显示数据的插件　　→ edit=single

・若为仅更新setParam参数一览表中选择范围的行的插件　　→　edit=region

・若为更新全部数据的插件　　→　edit=all

读取插件的执行结果时，setParam会对应于edit的值，切换仅读取相应行抑或全部重新读取，如下例。此时，取得数据的方式（全部取得或仅取得相应行）随插件设定文件（plugin.txt）中指定的插件类型而改变。

例1：oto.ini第三行被显示时执行edit=single的插件的情况下，setParam会丢弃结果文件开头两行仅取得第三行。

例2：oto.ini的第3、4、7行数据在一览表中被显示时执行edit=region的插件的情况下，setParam会丢弃结果文件开头2行取得第3〜7行。

例3：执行edit=all的插件时，setParam会将结果文件全部取得。

## 其他

插件开发方法今后仍有变更的可能性。如有“想从setParam侧接收此数据”、“想向setParam侧发送此数据”等需求，请在博客的评论区等处进行告知，不胜感激。
