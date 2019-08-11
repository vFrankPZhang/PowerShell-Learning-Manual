# 格式化

## 格式化输出

Format-Wide

Format-List

Format-Wrap

默认：对象结果4个或以下，以表格显示，5个或以上，以列表显示。

## 默认格式

1.

```powershell
Get-Process | gm
```

2.

```powershell
cd $pshome
code dotnettypes.format.ps1xml
```

3.在内容中找到Process类的描述

## 格式化的过程

1.把Process的对象放入管道
2.管道的最后有一个隐藏的cmdlet，out-default，作用是把需要运行的命令全部放入管道中。
3.Out-Defaut把结果传给Out-Host。（Host是指显示屏）
4.Out-Host判断是不是标准对象，是，传递给格式化系统，按照配置文件格式化显示。
5.如果没从标准配置文件中找到对对象的格式化的定义，会查找是否有人为对该对象预定义。types.ps1xml中找到，比如"win32_OperatingSystem"，在"DefaultDisplayPropertySet"中定义。
6.如果还没找到，就会考虑对象全部的属性值。

## 自定义列与列表条目

```powershell
Get-Process | ft name,@{name='VM(MB)';expression={$_.VM/1mb -as [int]}} -AutoSize
```

```powershell
Get-Process | ft name,@{name='VM(MB)';expression={$_.VM};formatstring='F2';alignment='right'} -AutoSize
```
