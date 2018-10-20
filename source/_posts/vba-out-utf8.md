---
title: 使用VBA输出UTF-8
date: 2018-06-19 15:52:01
tags: [vba,utf-8,输出]
categories: [other]
---
以前使用excel导出xml时使用的， 默认导出的xml貌似是ANSI编码， 调用这个转码可以使导出的文件为UTF-8编码
<!--more-->
```css
Public Function writeOut(cText As String, file As String) As Integer
    On Error GoTo errHandler
    Dim fsT
    'Create Stream object
    Set fsT = CreateObject("ADODB.Stream")
    'Specify stream type - we want To save text/string data.
    fsT.Type = 2
    'Specify charset For the source text data.
    fsT.Charset = "utf-8"
    'Open the stream And write binary data To the object
    fsT.Open
    fsT.writetext cText
    'Save binary data To disk
    fsT.SaveToFile file, 2
    GoTo finish
errHandler:
    MsgBox (Err.Description)
    writeOut = 0
    Exit Function
finish:
    writeOut = 1
End Function
```
调用方法 `Call writeOut(datastr, filePath)`

参考网址: https://gist.github.com/JoBrad/1023484