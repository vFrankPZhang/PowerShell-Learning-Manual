# 输入

## Get还是Import?

如果我们需要导入一个文件的内容，我们可以使用`Get-Content`，也可以使用Import相关的命令，比如对于CSV文件，使用`Import-Csv`。

那我们应该使用哪个命令呢？还是两个都可以呢？我们先来看看结果。

### 【例子】

![get vs import](images\get_vs_import.jpg)

通过结果，我们可以看出来，Get-Content得到的内容格式就丢失了，所以对于一个特定格式的文件，使用Import特定的格式是更合理的。

### 【练习】

1.比较一下Get-Content和Import-Clixml的区别。

## 人与CLI的交互——输入

PowerShell的Read-Host cmdlet的功能事提供一个文本提示，然后收集来自用户的输入信息。

```powershell
PS C:\Windows\system32> Read-Host 'Enter a computer name'
Enter a computer name: Server-R1
Server-R1
```

通常，我们会把输入的结果传入给一个变量来使用。

```powershell
PS C:\Windows\system32> $ComputerName = Read-Host 'Enter a computer name'
Enter a computer name: Server-R1
PS C:\Windows\system32> $ComputerName
Server-R1
```
