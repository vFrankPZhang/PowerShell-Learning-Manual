# 管道参数绑定

PowerShell必须决定由命令的哪个参数接收。这个决定过程称为管道参数绑定(Pipeline parameter binding)。

首先尝试使用ByValue，如果这种方法行不通，将会尝试ByPropertyName。

## ByValue

### 【例子】

1.

```powershell
Get-Service -ComputerName localhost
```

2.

```powershell
Get-Service -ComputerName (Get-Content .\computername.txt)
```

3.

```powershell
Get-Content .\computername.txt | Get-Service
```

4.

```powershell
Get-Process -Name notepad | Stop-Process
```

5.

```powershell
Get-Service  -Name s* | Stop-Process
```

## ByPropertyName

当ByValue不能工作的时候，PowerShell会尝试ByPropertyName。

### 【例子】

保存下面的内容为CSV文件：
name
notepad

然后：

```powershell
PS C:\> $a = Import-Csv C:\Users\Frank\Desktop\notepad.csv
PS C:\> $a | gm

   TypeName:System.Management.Automation.PSCustomObject

Name        MemberType   Definition
----        ----------   ----------
Equals      Method       bool Equals(System.Object obj)
GetHashCode Method       int GetHashCode()
GetType     Method       type GetType()
ToString    Method       string ToString()
name        NoteProperty string name=notepad

PS C:\> $a | Stop-Process
```

很明显，`$a`的类型不是`TypeName:System.Diagnostics.Process`，但是notepad还是被结束了，因为PowerShell根据ByPropertyName对对象进行匹配，匹配到了Name，读取了Name的值notepad，作为了`Stop-Process`命令的`-Name`的参数值。

 