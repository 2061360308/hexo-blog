---
title: rainmeter脚本编写
categories:
  - 技术
date: 2023-06-17 20:29:18
tags:
---

大概几个月前就为电脑安装并配置了雨滴桌面，当时在网上找到的教程很少，所以导致自己仅是稍加改动这些脚本也是痛苦万分。正好今天有时间所以所幸记录一下目前已知的一些语法吧！

<!-- more -->


## 鼠标操作
### Leftmousedownaction
鼠标左键点下执行一个操作.提示一下的是这个禁用了拖动.你可以看看下面的

### Rightmousedownaction
鼠标右键点下执行一个操作.提示一下的是这个禁用了上下文菜单.

### Middlemousedownaction
鼠标中键点下执行一个操作

### Leftmouseupaction
左键点击松开之后执行一个操作,这个不会禁用拖动.这个一般在你希望点击一个东西触发点动作的时候使用.Leftmousedown应该尽量不要和这个连用,除非一些特殊的情况.

### Rightmouseupaction
右键点击松开之后执行一个操作.这个不会禁用上下文菜单.

### Middlemouseupaction
中键点击松开之后执行一个操作.

### Leftmousedoubleclickaction
左键连击两次执行一个操作.如果这个没有添加,那么Leftmousedownaction将会被执行.

### Rightmousedoubleclickaction
右键连击两次执行一个操作.注意的是这个也禁用了右键上下文菜单.

### Middlemousedoubleclickaction
中键连击两次执行一个操作

### Mouseoveraction
鼠标经过一个皮肤或者Meter执行一个操作

### Mouseleaveaction
鼠标离开一个皮肤或者Meter执行一个操作

### Mouseactioncursor
鼠标动作游标.这个当设定为1(默认情况)的时候默认为手型游标.当你鼠标经过一个带有鼠标动作的Meter或皮肤的时候显示这个游标.
对于个别的皮肤你可以设定Mouseactioncursor=0,这个可以取消鼠标经过相应Meter或者皮肤时候出现的游标设定.这个是在一个皮肤的[RainMeter]节点下面进行定义的,你也可以在Meter类型的节点下面定义.
提示:
如果你的一个Meter或者一个按钮有一个鼠标动作.而且有一个Meter在它的上头,你可能需要设定Mouseactioncursor=0于靠上的Meter(即使它没有任何的鼠标动作)

### Mouseactioncursorname
如果一个自定义游标(比如:.Cur或者.Ani)在皮肤的根目录下面的@Resources\Cursors文件夹里面被索引到,那么一系列带有鼠标动作的Meter或者皮肤会自动的加载替代默认的”手型”指针,这个选项可以在[RainMeter] 节点下面或者在任何一个Meter节点下面通过Mouseactioncursorname=Mycursor.Cur的格式使用.在选项里面你无需指定路径,另外,一些Windows内置的游标可以直接的使用而不需要任何的文件名或者扩展名(比如:Hand, Text, Help, Busy, Cross, Pen)

## 我的桌面组件源码展示
### Explore
```
[Rainmeter]
Author=Vit-Ok | vit-ok.deviantart.com
Update=-1

//定义变量
[Variables]
SolidColor=0,0,0,1
Start="C:\Users\86152\AppData\Roaming\Microsoft\Windows\Libraries\当前使用.library-ms"
Folder1="D:\"
Folder1Name=数据
Folder2="E:\"
Folder2Name=代码
Folder3="F:\"
Folder3Name=资源
Folder4="D:\全部应用"
Folder4Content="C:\自定义logo\全部应用.png"
Folder5="D:\网络快捷方式"
Folder5Content="C:\自定义logo\URL快捷方式.png"
Folder6="D:\users\Documents\my"
Folder6Content="C:\自定义logo\文档.png"
Folder7="D:\software\hexo_blog"


[Style]
FontFace=Calibri
FontSize=9
StringStyle=Bold
FontColor=255,255,255,220
StringEffect=Shadow
FontEffectColor=0,0,0,40
AntiAlias=1

//背景长条
[Border]
Meter=IMAGE
ImageName=bar.png
W=#ScreenAreaWidth#
H=40


//标头
[BeginButton]
Meter=IMAGE
ImageName=文档管理.png
X=5
Y=0
SolidColor=#SolidColor#
H=30
W=40
LeftMouseDownAction=!Execute ["#Start#"]
MouseOverAction=!Execute [!RainmeterShowMeter Back][!RainmeterRedraw]
MouseLeaveAction=!Execute [!RainmeterHideMeter Back][!RainmeterRedraw]


[Folder1]
Meter=STRING
MeterStyle=Style
Text=#Folder1Name#
X=65
Y=7
SolidColor=#SolidColor#
H=16
w=30
LeftMouseDownAction=!Execute ["#Folder1#"]

[Folder2]
Meter=STRING
MeterStyle=Style
Text=#Folder2Name#
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=30
LeftMouseDownAction=!Execute ["#Folder2#"]

[Folder3]
Meter=STRING
MeterStyle=Style
Text=#Folder3Name#
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=30
LeftMouseDownAction=!Execute ["#Folder3#"]

[SplitLline1]
Meter=STRING
MeterStyle=Style
Text=|
x=15R
y=7
SolidColor=#SolidColor#
H=16
w=25

[Folder4]
Meter=IMAGE
ImageName=#Folder4Content#
x=6R
y=0
SolidColor=#SolidColor#
H=30
w=30
LeftMouseDownAction=!Execute ["#Folder4#"]
MouseOverAction=!Execute [!RainmeterShowMeter Back][!RainmeterRedraw]
MouseLeaveAction=!Execute [!RainmeterHideMeter Back][!RainmeterRedraw]


[Folder5]
Meter=IMAGE
ImageName=#Folder5Content#
x=6R
y=0
SolidColor=#SolidColor#
H=25
w=30
LeftMouseDownAction=!Execute ["#Folder5#"]
MouseOverAction=!Execute [!RainmeterShowMeter Back][!RainmeterRedraw]
MouseLeaveAction=!Execute [!RainmeterHideMeter Back][!RainmeterRedraw]


[Folder6]
Meter=IMAGE
ImageName=#Folder6Content#
x=6R
y=2
SolidColor=#SolidColor#
H=25
w=30
LeftMouseDownAction=!Execute ["#Folder6#"]
MouseOverAction=!Execute [!RainmeterShowMeter Back][!RainmeterRedraw]
MouseLeaveAction=!Execute [!RainmeterHideMeter Back][!RainmeterRedraw]

[SplitLline2]
Meter=STRING
MeterStyle=Style
Text=|
x=15R
y=7
SolidColor=#SolidColor#
H=16
w=25


[Pycharm]
Meter=STRING
MeterStyle=Style
Text=Pycharm
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=50
LeftMouseDownAction=!Execute ["D:\software\JetBrains\PyCharm 2022.2\bin\pycharm64.exe"]

[Clion]
Meter=STRING
MeterStyle=Style
Text=Clion
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=30
LeftMouseDownAction=!Execute ["D:\software\JetBrains\CLion 2022.2\bin\clion64.exe"]

[Tabby]
Meter=STRING
MeterStyle=Style
Text=Tabby
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=32
LeftMouseDownAction=!Execute ["D:\software\Tabby\Tabby.exe"]

[Steam++]
Meter=STRING
MeterStyle=Style
Text=Steam++
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=50
LeftMouseDownAction=!Execute ["D:\software\steam++\Steam++.exe"]

[SplitLline3]
Meter=STRING
MeterStyle=Style
Text=|
x=15R
y=7
SolidColor=#SolidColor#
H=16
w=25

[Ps]
Meter=STRING
MeterStyle=Style
Text=Ps
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=20
LeftMouseDownAction=!Execute ["D:\software\Adobe\Photoshop2020\Photoshop.exe"]


[Pr]
Meter=STRING
MeterStyle=Style
Text=Pr
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=20
LeftMouseDownAction=!Execute ["D:\software\Adobe\PremierePro2021\Adobe Premiere Pro 2021\Adobe Premiere Pro.exe"]

[SplitLline4]
Meter=STRING
MeterStyle=Style
Text=|
x=15R
y=7
SolidColor=#SolidColor#
H=16
w=25


[chat]
Meter=STRING
MeterStyle=Style
Text=社交
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=54
Leftmousedoubleclickaction=!Execute ["D:\software\WeChat\WeChat.exe"]["D:\software\WXWork\WXWork.exe"]["D:\software\TIM\Bin\QQScLauncher.exe"]
LeftMouseDownAction=!Execute ["D:\software\WeChat\WeChat.exe"]

[Folder7]
Meter=STRING
MeterStyle=Style
Text=博客
x=6R
y=7
SolidColor=#SolidColor#
H=16
w=54
LeftMouseDownAction=!Execute ["#Folder7#"]
MouseOverAction=!Execute [!RainmeterShowMeter Back][!RainmeterRedraw]
MouseLeaveAction=!Execute [!RainmeterHideMeter Back][!RainmeterRedraw]
;计算机


[Computer]
Meter=IMAGE
ImageName=电脑.png
X=((#SCREENAREAWIDTH#) - 70)
Y=0
SolidColor=#SolidColor#
H=28
W=28
Hidden=0
MouseOverAction=!Execute [!RainmeterShowMeter BinBak][!RainmeterRedraw]
MouseLeaveAction=!Execute [!RainmeterHideMeter BinBak][!RainmeterRedraw]
LeftMouseDownAction=!execute [!RainmeterPluginBang "MeasureBin OpenBin"]
LeftMouseUpAction =!execute [::{20D04FE0-3AEA-1069-A2D8-08002B30309D}]

;垃圾桶
[MeterBin]
Meter=IMAGE
ImageName=垃圾桶满.png
X=((#SCREENAREAWIDTH#) - 28)
Y=0
SolidColor=#SolidColor#
H=28
W=28
Hidden=0
MouseOverAction=!Execute [!RainmeterShowMeter BinBak][!RainmeterRedraw]
MouseLeaveAction=!Execute [!RainmeterHideMeter BinBak][!RainmeterRedraw]
LeftMouseDownAction=!execute [!RainmeterPluginBang "MeasureBin OpenBin"]
LeftMouseUpAction =!execute [::{645FF040-5081-101B-9F08-00AA002F954E}]
```

### Note
```
[Rainmeter]
Update=5000

[Variables]

Note=#CURRENTPATH#Notes.txt

[MeasureNotes]
Measure=Plugin
Plugin=Plugins\QuotePlugin.dll
PathName=#Note#
Disabled=0
Separator=?
#Subfolders=0
#FileFilter=*.txt

[BG]
Meter=IMAGE
ImageName=bg.png
X=5
Y=5
AntiAlias=1

[Click]
Meter=IMAGE
SolidColor=0, 0, 0, 1
X=100
Y=r
W=160
H=20
AntiAlias=1
LeftMouseDownAction=!execute ["#Note#"][!RainmeterRedraw]

[Notes]
Meter=STRING
MeasureName=MeasureNotes
X=24
Y=42r
W=320
H=238
FontColor=255,255,255,200
FontFace=微软雅黑
FontSize=10
StringAlign=LEFT
StringStyle=BoldItalic
AntiAlias=1
ClipString=1
```

### 玻璃相册

```
[Rainmeter]
;这里设置更换图片的时间
Update=60000
;Metadata added by RainBrowser
;http://rainmeter.net/RainWiki/index.php?title=Rainmeter_101#.5BMetadata.5D

[Metadata]
Name=
Config=
Description=
Instructions=
Version=
Tags=
License=
Variant=
Preview=

;End of added Metadata


---------------------------------设置文件夹路径----
[Variables]
Picture=D:\卢澳\Pictures\飞火壁纸库\FFWallPaper\custom\img
DefaultWallPaper=D:\卢澳\Pictures\飞火壁纸库\FFWallPaper\custom\img\2e72a3aa74044bdbbc0479fb4b09c18a.jpeg
------------------------------------------
[Rai]
MouseOverAction=!execute [!RainmeterShowMeter MeterNext][!RainmeterShowMeter MeterPrevious][!RainmeterRedraw]
MouseLeaveAction=!execute [!RainmeterHideMeter MeterNext][!RainmeterHideMeter MeterPrevious][!RainmeterRedraw]


-----------------------------------------------


[Random]
Measure=Plugin
Plugin=Plugins\QuotePlugin.dll
PathName=#Picture#
Subfolders=1

[win7rw]
Meter=IMAGE
ImageName=png.png
X=0
Y=0
w=494
h=310

[Image]
MeasureName=Random
Meter=IMAGE
X=58r
Y=45r
W=384
H=216

[ChangeWallPaper]
Meter=BUTTON
ButtonImage=b.png
X=20
Y=20
ButtonCommand=!Execute ["D:\software\Rainmeter\Rainmeter\Skins\我的桌面\玻璃相册\wallPaper.exe""[Random]"]
Hidden=0

[SetDefaultWallPaper]
Meter=BUTTON
ButtonImage=b.png
X=240
Y=20
ButtonCommand=!Execute ["D:\software\Rainmeter\Rainmeter\Skins\我的桌面\玻璃相册\wallPaper.exe""#DefaultWallPaper#"]
Hidden=0

[RefreshImg]
Meter=BUTTON
ButtonImage=b.png
X=454
Y=20
ButtonCommand=!Execute [!RainmeterRefresh]
Hidden=0


[ShowImage]
Meter=BUTTON
ButtonImage=b.png
X=20
Y=270
Hidden=0
ButtonCommand=!Execute ["[Random]"]

[OpenPath]
Meter=BUTTON
ButtonImage=b.png
x=454
y=270
Hidden=0
ButtonCommand=!Execute ["#Picture#"]
```
```
# wallPaper.py
# 需要将其打包为exe放置到对应同级目录下
import ctypes,sys

class Main:
    def __init__(self):
        path = 'D:\卢澳\Pictures\飞火壁纸库\FFWallPaper\custom\img\ef93148c2df9ca39b3a3ddbdb6171b22.jpg'
        ctypes.windll.user32.SystemParametersInfoW(20, 0, sys.argv[1], 0)

        input()


application = Main()
```
### 环形时钟
```
;========================================
; NIGHT SHADE CLOCK by murasaki55
;========================================


[Rainmeter]
Author=murasaki55
AppVersion=1001000
Update=1000

[Variables]
HandColor=238, 238, 238

[MeasureTime]
Measure=Time

[Clock]
Meter=IMAGE
ImageName=face.png
X=0
Y=0
W=256
H=256

[Hours]
Meter=ROUNDLINE
MeterStyle=Seconds
LineWidth=4
LineLength=80
LineColor=#HandColor#
ValueReminder=43200

[Minutes]
Meter=ROUNDLINE
MeterStyle=Seconds
LineWidth=4
LineLength=110
LineColor=#HandColor#
ValueReminder=3600

[Seconds]
Meter=ROUNDLINE
MeasureName=MeasureTime
X=r
Y=r
W=256
H=256
LineWidth=2
StartAngle=4.7123889
RotationAngle=6.2831853
LineLength=114
LineStart=0
AntiAlias=1
LineColor=#HandColor#
Solid=0
ValueReminder=60
```
```
;========================================
; NIGHT SHADE CLOCK by murasaki55
;========================================


[Rainmeter]
Author=murasaki55
AppVersion=1001000
Update=1000

[Variables]
HandColor=238, 238, 238

[MeasureTime]
Measure=Time

[Clock]
Meter=IMAGE
ImageName=face.png
X=0
Y=0
W=128
H=128

[Hours]
Meter=ROUNDLINE
MeterStyle=Seconds
LineWidth=2
LineLength=40
LineColor=#HandColor#
ValueReminder=43200

[Minutes]
Meter=ROUNDLINE
MeterStyle=Seconds
LineWidth=2
LineLength=55
LineColor=#HandColor#
ValueReminder=3600

[Seconds]
Meter=ROUNDLINE
MeasureName=MeasureTime
X=r
Y=r
W=128
H=128
LineWidth=1
StartAngle=4.7123889
RotationAngle=6.2831853
LineLength=57
LineStart=0
AntiAlias=1
LineColor=#HandColor#
Solid=0
ValueReminder=60
```

###频谱
```
[Rainmeter]
Update=15
Author=Connect-R
BackgroundMode=2
SolidColor=0,0,0,1
DynamicWindowSize=1
MouseScrollUpAction=[!SetVariable Scale "(#Scale#+#ScrollMouseIncrement#)"][!WriteKeyValue Variables Scale "(#Scale#+#ScrollMouseIncrement#)"][!Refresh] 
MouseScrollDownAction=[!SetVariable Scale "(#Scale#-#ScrollMouseIncrement# < 0.09 ? 0.09 : #Scale#-#ScrollMouseIncrement#)"][!WriteKeyValue Variables Scale "(#Scale#-#ScrollMouseIncrement# < 0.09 ? 0.09 : #Scale#-#ScrollMouseIncrement#)"][!Refresh]
LeftMouseDoubleClickAction=!ToggleConfig "Abelo\Settings" "Settings.ini"
HardwareAcceleration=1

[Variables]
@include=#@#Variables.inc
Scale=0.22
AverageSize=7

;-------------------------------------------------------------
;-------------------------------------------------------------

[MeasureAudioOutput]
Measure=Plugin
Plugin=AudioLevel
Port=Output
FFTSize=2048
FFTOverlap=1024
FFTAttack=0
FFTDecay=100
FreqMin=100
FreqMax=16500
Sensitivity=50
Bands=80

;-------------------------------------------------------------
;-------------------------------------------------------------

[MeterStyle]
BarOrientation=Vertical
X=(24*#Scale#)r
Y=(0*#Scale#)
W=(12*#Scale#)
H=(600*#Scale#)
BarColor=#FontColor2#

[MeterStyleR]
BarOrientation=Vertical
X=(24*#Scale#)r
Y=(612*#Scale#)
W=(12*#Scale#)
H=(300*#Scale#)
Flip=1
BarColor=#FontColor3#

;-------------------------------------------------------------
;-------------------------------------------------------------

[Measure01]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=1
AverageSize=#AverageSize#

[Measure02]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=2
AverageSize=#AverageSize#

[Measure03]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=3
AverageSize=#AverageSize#

[Measure04]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=4
AverageSize=#AverageSize#

[Measure05]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=5
AverageSize=#AverageSize#

[Measure06]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=6
AverageSize=#AverageSize#

[Measure07]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=7
AverageSize=#AverageSize#

[Measure08]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=8
AverageSize=#AverageSize#

[Measure09]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=9
AverageSize=#AverageSize#

[Measure10]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=10
AverageSize=#AverageSize#

[Measure11]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=11
AverageSize=#AverageSize#

[Measure12]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=12
AverageSize=#AverageSize#

[Measure13]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=13
AverageSize=#AverageSize#

[Measure14]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=14
AverageSize=#AverageSize#

[Measure15]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=15
AverageSize=#AverageSize#

[Measure16]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=16
AverageSize=#AverageSize#

[Measure17]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=17
AverageSize=#AverageSize#

[Measure18]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=18
AverageSize=#AverageSize#

[Measure19]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=19
AverageSize=#AverageSize#

[Measure20]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=20
AverageSize=#AverageSize#

[Measure21]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=21
AverageSize=#AverageSize#

[Measure22]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=22
AverageSize=#AverageSize#

[Measure23]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=23
AverageSize=#AverageSize#

[Measure24]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=24
AverageSize=#AverageSize#

[Measure25]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=25
AverageSize=#AverageSize#

[Measure26]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=26
AverageSize=#AverageSize#

[Measure27]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=27
AverageSize=#AverageSize#

[Measure28]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=28
AverageSize=#AverageSize#

[Measure29]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=29
AverageSize=#AverageSize#

[Measure30]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=30
AverageSize=#AverageSize#

[Measure31]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=31
AverageSize=#AverageSize#

[Measure32]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=32
AverageSize=#AverageSize#

[Measure33]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=33
AverageSize=#AverageSize#

[Measure34]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=34
AverageSize=#AverageSize#

[Measure35]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=35
AverageSize=#AverageSize#

[Measure36]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=36
AverageSize=#AverageSize#

[Measure37]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=37
AverageSize=#AverageSize#

[Measure38]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=38
AverageSize=#AverageSize#

[Measure39]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=39
AverageSize=#AverageSize#

[Measure40]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=40
AverageSize=#AverageSize#

[Measure41]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=41
AverageSize=#AverageSize#

[Measure42]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=42
AverageSize=#AverageSize#

[Measure43]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=43
AverageSize=#AverageSize#

[Measure44]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=44
AverageSize=#AverageSize#

[Measure45]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=45
AverageSize=#AverageSize#

[Measure46]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=46
AverageSize=#AverageSize#

[Measure47]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=47
AverageSize=#AverageSize#

[Measure48]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=48
AverageSize=#AverageSize#

[Measure49]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=49
AverageSize=#AverageSize#

[Measure50]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=50
AverageSize=#AverageSize#

[Measure51]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=51
AverageSize=#AverageSize#

[Measure52]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=52
AverageSize=#AverageSize#

[Measure53]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=53
AverageSize=#AverageSize#

[Measure54]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=54
AverageSize=#AverageSize#

[Measure55]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=55
AverageSize=#AverageSize#

[Measure56]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=56
AverageSize=#AverageSize#

[Measure57]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=57
AverageSize=#AverageSize#

[Measure58]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=58
AverageSize=#AverageSize#

[Measure59]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=59
AverageSize=#AverageSize#

[Measure60]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=60
AverageSize=#AverageSize#

[Measure61]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=61
AverageSize=#AverageSize#

[Measure62]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=62
AverageSize=#AverageSize#

[Measure63]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=63
AverageSize=#AverageSize#

[Measure64]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=64
AverageSize=#AverageSize#

[Measure65]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=65
AverageSize=#AverageSize#

[Measure66]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=66
AverageSize=#AverageSize#

[Measure67]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=67
AverageSize=#AverageSize#

[Measure68]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=68
AverageSize=#AverageSize#

[Measure69]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=69
AverageSize=#AverageSize#

[Measure70]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=70
AverageSize=#AverageSize#

[Measure71]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=71
AverageSize=#AverageSize#

[Measure72]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=72
AverageSize=#AverageSize#

[Measure73]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=73
AverageSize=#AverageSize#

[Measure74]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=74
AverageSize=#AverageSize#

[Measure75]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=75
AverageSize=#AverageSize#

[Measure76]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=76
AverageSize=#AverageSize#

[Measure77]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=77
AverageSize=#AverageSize#

[Measure78]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=78
AverageSize=#AverageSize#

[Measure79]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=79
AverageSize=#AverageSize#

[Measure80]
Measure=Plugin
Plugin=AudioLevel
Parent=MeasureAudioOutput
Type=Band
BandIdx=80
AverageSize=#AverageSize#

;-------------------------------------------------------------
;-------------------------------------------------------------

[MeterBand01]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure01
X=(0*#Scale#)

[MeterBand02]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure02

[MeterBand03]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure03

[MeterBand04]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure04

[MeterBand05]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure05

[MeterBand06]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure06

[MeterBand07]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure07

[MeterBand08]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure08

[MeterBand09]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure09

[MeterBand10]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure10

[MeterBand11]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure11

[MeterBand12]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure12

[MeterBand13]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure13

[MeterBand14]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure14

[MeterBand15]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure15

[MeterBand16]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure16

[MeterBand17]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure17

[MeterBand18]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure18

[MeterBand19]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure19

[MeterBand20]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure20

[MeterBand21]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure21

[MeterBand22]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure22

[MeterBand23]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure23

[MeterBand24]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure24

[MeterBand25]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure25

[MeterBand26]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure26

[MeterBand27]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure27

[MeterBand28]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure28

[MeterBand29]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure29

[MeterBand30]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure30

[MeterBand31]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure31

[MeterBand32]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure32

[MeterBand33]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure33

[MeterBand34]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure34

[MeterBand35]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure35

[MeterBand36]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure36

[MeterBand37]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure37

[MeterBand38]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure38

[MeterBand39]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure39

[MeterBand40]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure40

[MeterBand41]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure41

[MeterBand42]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure42

[MeterBand43]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure43

[MeterBand44]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure44

[MeterBand45]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure45

[MeterBand46]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure46

[MeterBand47]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure47

[MeterBand48]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure48

[MeterBand49]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure49

[MeterBand50]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure50

[MeterBand51]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure51

[MeterBand52]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure52

[MeterBand53]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure53

[MeterBand54]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure54

[MeterBand55]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure55

[MeterBand56]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure56

[MeterBand57]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure57

[MeterBand58]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure58

[MeterBand59]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure59

[MeterBand60]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure60

[MeterBand61]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure61

[MeterBand62]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure62

[MeterBand63]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure63

[MeterBand64]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure64

[MeterBand65]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure65

[MeterBand66]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure66

[MeterBand67]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure67

[MeterBand68]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure68

[MeterBand69]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure69

[MeterBand70]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure70

[MeterBand71]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure71

[MeterBand72]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure72

[MeterBand73]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure73

[MeterBand74]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure74

[MeterBand75]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure75

[MeterBand76]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure76

[MeterBand77]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure77

[MeterBand78]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure78

[MeterBand79]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure79

[MeterBand80]
Meter=Bar
MeterStyle=MeterStyle
MeasureName=Measure80

;-------------------------------------------------------------
;-------------------------------------------------------------

[MeterBand01R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure01
X=(0*#Scale#)

[MeterBand02R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure02

[MeterBand03R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure03

[MeterBand04R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure04

[MeterBand05R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure05

[MeterBand06R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure06

[MeterBand07R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure07

[MeterBand08R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure08

[MeterBand09R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure09

[MeterBand10R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure10

[MeterBand11R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure11

[MeterBand12R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure12

[MeterBand13R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure13

[MeterBand14R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure14

[MeterBand15R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure15

[MeterBand16R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure16

[MeterBand17R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure17

[MeterBand18R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure18

[MeterBand19R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure19

[MeterBand20R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure20

[MeterBand21R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure21

[MeterBand22R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure22

[MeterBand23R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure23

[MeterBand24R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure24

[MeterBand25R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure25

[MeterBand26R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure26

[MeterBand27R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure27

[MeterBand28R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure28

[MeterBand29R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure29

[MeterBand30R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure30

[MeterBand31R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure31

[MeterBand32R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure32

[MeterBand33R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure33

[MeterBand34R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure34

[MeterBand35R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure35

[MeterBand36R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure36

[MeterBand37R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure37

[MeterBand38R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure38

[MeterBand39R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure39

[MeterBand40R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure40

[MeterBand41R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure41

[MeterBand42R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure42

[MeterBand43R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure43

[MeterBand44R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure44

[MeterBand45R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure45

[MeterBand46R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure46

[MeterBand47R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure47

[MeterBand48R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure48

[MeterBand49R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure49

[MeterBand50R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure50

[MeterBand51R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure51

[MeterBand52R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure52

[MeterBand53R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure53

[MeterBand54R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure54

[MeterBand55R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure55

[MeterBand56R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure56

[MeterBand57R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure57

[MeterBand58R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure58

[MeterBand59R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure59

[MeterBand60R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure60

[MeterBand61R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure61

[MeterBand62R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure62

[MeterBand63R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure63

[MeterBand64R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure64

[MeterBand65R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure65

[MeterBand66R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure66

[MeterBand67R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure67

[MeterBand68R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure68

[MeterBand69R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure69

[MeterBand70R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure70

[MeterBand71R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure71

[MeterBand72R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure72

[MeterBand73R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure73

[MeterBand74R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure74

[MeterBand75R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure75

[MeterBand76R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure76

[MeterBand77R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure77

[MeterBand78R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure78

[MeterBand79R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure79

[MeterBand80R]
Meter=Bar
MeterStyle=MeterStyleR
MeasureName=Measure80
```
### 日期
```
[Rainmeter]
BackgroundMode=0

[MeasureMonth]
Measure=Time
Format=%#m

[MeasureDate]
Measure=Time
Format=%d

[MeasureDay]
Measure=Time
Format=%a

;==========================================================================================
;					BACKGROUND COLOR
;==========================================================================================

[MeterBlack]
Meter=IMAGE
X=0
Y=0
W=110
H=35
SolidColor=0, 0, 0, 0

; --------------------------------------------------------------------------------------------------------------------------------------
;				DATE
; --------------------------------------------------------------------------------------------------------------------------------------

[MeterMonth]
Meter=STRING
MeasureName=MeasureMonth
X=80
Y=-9
FontColor=0, 0, 0, 200
FontSize=30
StringAlign=RIGHT
FontFace=MicrogrammaDBolExt
AntiAlias=1

[MeterDate]
Meter=STRING
MeasureName=MeasureDate
X=110
Y=-1
FontColor=0, 0, 0, 200
FontSize=14
StringAlign=RIGHT
FontFace=MicrogrammaDBolExt
AntiAlias=1

[MeterDay]
Meter=STRING
MeasureName=MeasureDay
X=89
Y=19
FontColor=0, 0, 0, 100
FontSize=7
StringAlign=CENTER
FontFace=MicrogrammaDBolExt
AntiAlias=1
```

### 时间
```
[Rainmeter]
BackgroundMode=0

[MeasureHour]
Measure=Time
Format=%#I

[MeasureMinute]
Measure=Time
Format=%M

[MeasureAMPM]
Measure=Time
Format=%p

;==========================================================================================
;					BACKGROUND COLOR
;==========================================================================================

[MeterBlack]
Meter=IMAGE
X=0
Y=0
W=110
H=35
SolidColor=0, 0, 0, 0

; --------------------------------------------------------------------------------------------------------------------------------------
;				TIME
; --------------------------------------------------------------------------------------------------------------------------------------

[MeterHour]
Meter=STRING
MeasureName=MeasureHour
X=80
Y=-9
FontColor=0, 0, 0, 200
FontSize=30
StringAlign=RIGHT
FontFace=MicrogrammaDBolExt
AntiAlias=1

[MeterMinute]
Meter=STRING
MeasureName=MeasureMinute
X=110
Y=-1
FontColor=0, 0, 0, 200
FontSize=14
StringAlign=RIGHT
FontFace=MicrogrammaDBolExt
AntiAlias=1

[MeterAMPM]
Meter=STRING
MeasureName=MeasureAMPM
X=90
Y=19
FontColor=0, 0, 0, 100
FontSize=7
StringAlign=CENTER
FontFace=MicrogrammaDBolExt
AntiAlias=1
```
