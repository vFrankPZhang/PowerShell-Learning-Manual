# PowerShell脚本概述

## 执行脚本

### 在桌面中

基于安全的考虑，PowerShell是有第三方脚本的执行的安全策略的。

PowerShell脚本的文件名后缀是ps1，默认情况下，脚本双击打开是通过记事本编辑这个ps1文件，而不是执行这个脚本。

这样设定是安全的，比如一个脚本的作用是关机，而这个机器不能关机，不知道的情况下，直接双击脚本就关机了就会出问题。

### 在命令行中

在命令行中一个不是内置的脚本也不是可以直接执行的。

比如我自己写了一个脚本`do-something`

```powershell
PS C:\Users\vFrank\Desktop> dir *.ps1


    目录: C:\Users\vFrank\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2019/9/14     21:57             21 do-something.ps1


PS C:\Users\vFrank\Desktop> do-something
do-something : 无法将“do-something”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径
，请确保路径正确，然后再试一次。
所在位置 行:1 字符: 1
+ do-something
+ ~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (do-something:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException


Suggestion [3,General]: 找不到命令 do-something，但它确实存在于当前位置。默认情况下，Windows PowerShell 不会从当前位置加载命令。如果信任此命令，请改为键入“.\do-something”。有关详细信息，请参阅 "get-help about_Command_Precedence"。
PS C:\Users\vFrank\Desktop> .\do-something.ps1
.\do-something.ps1 : 无法加载文件 C:\Users\vFrank\Desktop\do-something.ps1，因为在此系统上禁止运行脚本。有关详细信息，
请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
+ .\do-something.ps1
+ ~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

会发现，需要指明具体路径才可能执行不是内置的脚本。

## 执行策略

当我们执行加上路径执行自己的脚本呢的时候，也会报错，我们发现PowerShell告诉我们，我们需要更改PowerShell的Execution_Policies才能执行自己的脚本。

`Set-ExecutionPolicy Bypass`可以使我们执行自己写的脚本。

```powershell
PS C:\Users\vFrank\Desktop> .\do-something.ps1
Hello PS
```

关于ExecutionPolicy的详细说明可以通过下面的命令查看：

`help about_execution_policies`

## 编辑脚本

如果我们要想自己写脚本，首选要选一个趁手的编辑器。

PowerShell默认有一个编辑器，PowerShell ISE。

ISE有一系列好处，比如提示，高亮显示等等，可以让我们更流畅的写脚本。

除了默认的，我们还可以使用一个微软开发的开源的编辑器，叫Vistual Studio Code。

## 练习

1、打开Vistual Studio Code，写一个简单的PowerShell脚本，比如：`Write-Host "My first PS script."`并在你的机器上执行。
