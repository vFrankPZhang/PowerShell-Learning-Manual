# 对象的操作——Measure-Object

计算对象里面的数值属性，如求和、平均数等等。

1.文件夹下有多少文件。

```powershell
PS C:\> Get-ChildItem | Measure-Object


Count    : 27
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

2.计算Length属性

```powershell
PS C:\> Get-ChildItem | Measure-Object -Property length -Minimum -Maximum -Average


Count    : 15
Average  : 26994.4
Sum      :
Maximum  : 181791
Minimum  : 0
Property : length
```

练习

1.`gwmi Win32_LogicalDisk`可以获取磁盘信息，求可用磁盘空间一共有多少？
