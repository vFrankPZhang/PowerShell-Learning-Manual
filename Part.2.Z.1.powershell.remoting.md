
# 远程执行命令

Windows PowerShell支持多种远程技术，包括WMI，RPC和WS-Management。

你可以在一台，甚至上百台的机器上远程执行PowerShell命令。

## 自带远程的cmdlet

有很多的cmdlets是有`-ComputerName`这个参数的，对于这些命令，不需要什么特别的配置，就可以在一台或者多台机器上远程执行命令或者收集数据。

**【例子】**

```bash
Restart-Computer
Get-EventLog
Get-HotFix
Get-Process
Get-Service
Get-WmiObject
```

## Windows PowerShell远程

哪怕没有`-ComputerName`这个参数，基于WS-Management这个协议，Windows PowerShell可以在一个多个机器上执行Windows PowerShell命令。

想通过Windows PowerShell远程的方式远程执行命令，这些远程的机器必须配置了远程管理。

### 一对一

**建立交互式会话**

可以通过下面的命令跟一台远程的机器建立一个交互式的会话：

```bash
Enter-PSSession Server01
```

命令行提示符会变成你远程的目标计算机的名字，你输入的命令都会在远程计算机上执行，结果会显示在你当前的本地计算机上。

可以通过下面的命令结束会话：

```bash
Exit-PSSession
```

交互式会话，更像是你在本地打开了远程目标的机器的命令行窗口，或者也可以理解为，你远程登录到了目标机器上，使用PowerShell，这个对话框是一直活着的，直到你结束会话。

### 一对多

**远程执行命令**

如果只是想在远程的一台或多台机器上执行命令，我们可以使用Invoke-Command命令。

```bash
Invoke-Command -ComputerName Server01, Server02 -ScriptBlock {Get-UICulture}
```


**远程执行一个脚本**

如果你想远程在一台或多台机器上执行一个脚本，还是Invoke-Command命令，你需要用到参数`-FilePath`来指定脚本的位置，请注意，这个脚本必须在你本地计算机上，或者是一个你可以访问的位置，脚本运行的结果会返回到你的本地计算机。

```bash
Invoke-Command -ComputerName Server01, Server02 -FilePath c:\Scripts\DiskCollect.ps1
```

**建立一个持久的会话**

假如你需要在一些机器上保持一个会话，可以通过New-PSSession建立持久的会话。

比如：

```bash
$s = New-PSSession -ComputerName Server01, Server02
Invoke-Command -Session $s {$h = Get-HotFix}
Invoke-Command -Session $s {$h | where {$_.InstalledBy -ne "NTAUTHORITY\SYSTEM"}}
```

**为什么有时候需要持久的会话？**

使用`Invoke-Command`或`Enter-PSSession`的`-ComputerName`参数时，Windows PowerShell会建立一个与远程的临时连接，命令完成时，会关闭该连接，这样，远程时创建的数据都会丢失，而持久会话，数据就可以保留，持续使用。
