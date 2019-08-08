# 对象的操纵

后面，我们会开始学习，如果通过属性，来操纵对象。

我们也先来看一下，我们能对对象做什么：

```powershell
PS C:\> gcm *-object*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Compare-Object                                     3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          ForEach-Object                                     3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Group-Object                                       3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Measure-Object                                     3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          New-Object                                         3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Register-ObjectEvent                               3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Select-Object                                      3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Sort-Object                                        3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Tee-Object                                         3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Where-Object                                       3.0.0.0    Microsoft.PowerShell.Core
```

# 选择对象——Select-Object

可以通过Select-Object，选择对象或者对象的属性。

###【例子】

1.根据需要的属性选择对象：

```powershell
Get-Process | Select-Object -Property ProcessName,Id,WS
```

2.列出前n个对象：

```powershell
PS C:\Windows\system32> Get-Process | select -First 5

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    681      27    14384      12640       5.16  17696   2 AcroRd32
    878      85   159760     139288     150.97  21244   2 AcroRd32
    207      17     8312       7996     304.83  16480   2 adintermaster
    164       8     2452       4280       2.42   5024   0 AdminService
    164      11     2012       4408      48.31  20716   2 ApMsgFwd

```

3.跳过前n个对象

```powershell
PS C:\Windows\system32> Get-Process | select -Skip 2

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    207      17     8312       8008     314.83  16480   2 adintermaster
    164       8     2452       4280       2.42   5024   0 AdminService
```

跳过首行会是比较多用到的。

