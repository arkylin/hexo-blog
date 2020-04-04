---
title: Android目标一：XML输出 （正在进行中）
categories:
  - [Android Develop]
tags:
date: 2020-03-08 00:36:00
translate_title: android-target-1-xm-x-l-output
keywords:
description:
top_img: /2020/03/08/android-target-1-xm-x-l-output/Wrong.png
comments:
cover: /2020/03/08/android-target-1-xm-x-l-output/Wrong.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
创建空白Hello World项目
进入```res/layout/activity_main.xml```
可以看见
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
其中不谈上面一部分
看下面一部分
```
android:layout_width="wrap_content"
```
这个为文字宽度随文本长度变化二变化
```
android:layout_height="wrap_content"
```
同理这个也是
```
android:text="Hello World!"
```
这个就是显示内容
```
app:layout_constraintBottom_toBottomOf="parent"
app:layout_constraintLeft_toLeftOf="parent"
app:layout_constraintRight_toRightOf="parent"
app:layout_constraintTop_toTopOf="parent"
```
这个就应该是定位显示内容，都为继承父配置

当你试着去重新手动设置一个的时候会显示
<img src="Wrong.png" width="100%" alt="Wrong">
会提示
```
This view is not constrained. It only has designtime positions, so it will jump to (0,0) at runtime unless you add the constraints  The layout editor allows you to place widgets anywhere on the canvas, and it records the current position with designtime attributes (such as layout_editor_absoluteX). These attributes are not applied at runtime, so if you push your layout on a device, the widgets may appear in a different location than shown in the editor. To fix this, make sure a widget has both horizontal and vertical constraints by dragging from the edge connections.  Issue id: MissingConstraints
```
```
```
此视图不受约束。它只有设计时位置，因此它将在运行时跳转到（0,0），除非您添加约束，否则布局编辑器允许您将小部件放置在画布上的任何位置，并且它使用设计时属性（例如布局编辑器绝对值）记录当前位置。这些属性在运行时不应用，因此如果您在设备上推送布局，小部件可能会出现在与编辑器中显示的位置不同的位置。要解决此问题，请通过从边缘连接拖动来确保小部件同时具有水平和垂直约束。问题id:缺少约束
```
```
```
Hardcoded string "test", should use @string resource  Hardcoding text attributes directly in layout files is bad for several reasons:  * When creating configuration variations (for example for landscape or portrait) you have to repeat the actual text (and keep it up to date when making changes)  * The application cannot be translated to other languages by just adding new translations for existing string resources.  There are quickfixes to automatically extract this hardcoded string into a resource lookup.  Issue id: HardcodedText
```
```
硬编码字符串“test”，应该在布局文件中直接使用@string资源硬编码文本属性有几个原因：*创建配置变体（例如横向或纵向）时，必须重复实际文本（并在进行更改时保持最新）*仅添加新的现有字符串资源的翻译。有一些快速修复程序可以自动将此硬编码字符串提取到资源查找中。问题id:HardcodedText
```
<img src="fix_it.png" width="100%" alt="fix_it">
对于下面这个我们点fix it
<img src="Fix_it_1.png" width="100%" alt="Fix_it_1">
则会添加值到```res/value/strings```里面去
<img src="res_strings.png" width="100%" alt="res_strings">
<img src="strings_main.png" width="100%" alt="strings_main">
添加约束
本来不会的不过幸好AS提供图形界面可以直接拖动，真的对初学者很友好
那么我们就知道了它的道理
以左上角为原点，往右下增到
<img src="lefttop_00.png" width="100%" alt="lefttop_00">
<img src="rightbottom_11.png" width="100%" alt="rightbottom_11">
```
app:layout_constraintHorizontal_bias="1.0"
```
这个可以说为X轴当到最右边就为1.0
```
app:layout_constraintVertical_bias="0.0"
```
这个就可以说是Y轴到最下边就是1.0
<img src="Vertical.png" width="100%" alt="Vertical">
```
android:layout_marginStart="84dp"
```
MarginStart指的是控件距离开头View部分的间距大小
我们在写layout布局的时候，我们会发现有这样几个比较相似的属性：
```
MarginStart   MarginLeft
MarginEnd    MarginRight
```

这些属性的区别是什么?  根据api注释，我们得知MarginStart指的是控件距离开头View部分的间距大小，MarginLeft则指的是控件距离左边View部分的间距大小，MarginEnd和MarginRight同理。

一般情况下，View开始部分就是左边，但是有的语言目前为止还是按照从右往左的顺序来书写的，例如阿拉伯语，在Android  4.2系统之后，Google在Android中引入了RTL布局，更好了支持了由右到左文字布局的显示，为了更好的兼容RTL布局，google推荐使用MarginStart和MarginEnd来替代MarginLeft和MarginRight，这样应用可以在正常的屏幕和由右到左显示文字的屏幕上都保持一致的用户体验。


  
  
  
  
  
  
  
  