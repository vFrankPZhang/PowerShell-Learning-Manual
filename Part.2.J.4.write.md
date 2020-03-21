# Write命令

PowerShell有一系列的Write命令，用于返回结果并输出。

## Write-Host

用于把结果或想要呈现的内容输出到屏幕上，可以使用`-ForegroundColor`和`-BackgroundColor`两个参数来定义显示的颜色。

```powershell
PS C:\Windows\system32> write-host "COLORFUL!" -ForegroundColor Yellow -BackgroundColor Black
COLORFUL!
```

但要注意一点，`-Host`命令输出到屏幕的任何东西都无法被捕捉，`-Host`仅仅用于与人进行直接交互。

## Write-Output

与Write-Host不同，Write-Output可以将对象发送给管道。

看上去Write-Output的效果跟Write-Host的效果类似，但是工作机是有区别的。

![Write-Host](images\write_host.png)

![Write-Output](images\write_output.png)

```powershell
PS C:\Windows\system32> Write-Host 'hello'
hello
PS C:\Windows\system32> Write-Output 'hello'
hello
PS C:\Windows\system32> Write-Host 'hello'| ? {$_.length -gt 10}
hello
PS C:\Windows\system32> Write-Output 'hello'| ? {$_.length -gt 10}
```

![Write-Output + Where-Object](images\write_output_where.png)

## 其他输出

PowerShell针对每种输出方式都有对应的内置配置变量。如果配置变量设置为“Continue”,那么输出就能看到结果，如果设置为“SilentlyContinue”,结果就不会输出。

|Cmdlet|作用|配置变量|
|----|----|----|
|Write-Warning|显示警告信息，默认以黄色字体显示，同时前面带有警告字样|$WarningPreference（默认为Continue）|
|Write-Verbose|显示详细信息，默认会以黄色字体显示，同时前面带有详细信息字样|$VerbosePreference（默认为SilentlyContinue）|
|Write-Debug|显示调试信息，默认会以黄色字体显示，同时前面带有调试字样|$DebugPreference（默认为SilentlyContinue）|
|Write-Error|产生一个错误信息|$ErrorActionPreference（默认为Continue）|

```powershell
PS C:\Windows\system32> Write-Host "hello"
hello
PS C:\Windows\system32> Write-Output "hello"
hello
PS C:\Windows\system32> Write-Verbose "hello"
PS C:\Windows\system32> Write-Verbose "hello" -Verbose
VERBOSE: hello
PS C:\Windows\system32> Write-Debug "hello"
PS C:\Windows\system32> Write-Debug "hello" -Debug
DEBUG: hello

Confirm
Continue with this operation?
[Y] Yes  [A] Yes to All  [H] Halt Command  [S] Suspend  [?] Help (default is "Y"):
PS C:\Windows\system32> Write-Error "hello"
Write-Error "hello" : hello
    + CategoryInfo          : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException
```
