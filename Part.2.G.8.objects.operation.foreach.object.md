
# 对象的操作——ForEach-Object

对每个项执行操作。

### 【例子】

1.依次把1,2,3除以2

```powershell
PS C:\> 1,2,3 | ForEach-Object -Process {$_/2}
0.5
1
1.5
```

2.获取文件夹下的文件和大小

```powershell
Get-ChildItem $pshome | ForEach-Object -Process {if (!$_.PSIsContainer) {$_.Name; $_.Length/1024; " " }}
```
