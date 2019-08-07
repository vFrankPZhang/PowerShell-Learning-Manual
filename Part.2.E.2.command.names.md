
# 学习PowerShell命令的命名规则

## Cmdlet使用 动词-名词 的命名规则

PowerShell的Cmdlets使用“动词-名词”的命名方式来对命令命名。每一个cmdlet的名字都是由一个标准的动词和可以自定义的名字来组成的。  
命令中的动词是用来描述这个命令是什么行为，名词是用来描述系统中各种特定的对象，所以，通过名字就很容易能够猜到，这个命令是干什么的。

**【例子】**  
比如：  
```bash
Get-Service
Get-Process
```
通过名字你很容可以猜到，第一个命令是获取当前的服务，而第二个命令是获取当前的进程。  
类似的：
```bash
Stop-Service
Stop-Process
```
你也就很容易猜到，这两个命令分别是结束服务和结束进程。

通过对于传统的命令行工具的命令来进行一个简单的了解，你就可以体会到PowerShell命令格式的精妙之处。

**【例子】**  
比如在CMD中，我们想要获取eventlog这个服务的状态，我们可以通过下面的命令：  
```bash
sc query eventlog
```  
我们想要查看记事本这个进程，我们可以通过下面的命令：
```bash
tasklist /fi "imagename eq notepad.exe"
```
如果是在PowerShell中，我们可以通过：
```bash
Get-Service -Name eventlog
Get-Process -Name notepad
```

我们可以看到，在传统的命令行工具里，不同的命令是相对独立的，因此有着自己的没有规律的名字，而且使用的语法也各不相同。  
而在PowerShell里面，统一的命名规则，让我们很容易猜到命令的意思，甚至通过命名规则去找到我们想要的命令。

**【例子】**  
通过动词，来找我们能Get什么东西？
```bash
PS> Get-Command -Verb Get

CommandType     Name                            Definition
-----------     ----                            ----------
Cmdlet          Get-Acl                         Get-Acl [[-Path] <String[]>]...
Cmdlet          Get-Alias                       Get-Alias [[-Name] <String[]...
Cmdlet          Get-AuthenticodeSignature       Get-AuthenticodeSignature [-...
Cmdlet          Get-ChildItem                   Get-ChildItem [[-Path] <Stri...
...
```

通过名词，来找我们能对Service做什么动作？
```bash
PS> Get-Command -Noun Service

CommandType     Name                            Definition
-----------     ----                            ----------
Cmdlet          Get-Service                     Get-Service [[-Name] <String...
Cmdlet          New-Service                     New-Service [-Name] <String>...
Cmdlet          Restart-Service                 Restart-Service [-Name] <Str...
Cmdlet          Resume-Service                  Resume-Service [-Name] <Stri...
Cmdlet          Set-Service                     Set-Service [-Name] <String>...
Cmdlet          Start-Service                   Start-Service [-Name] <Strin...
Cmdlet          Stop-Service                    Stop-Service [-Name] <String...
Cmdlet          Suspend-Service                 Suspend-Service [-Name] <Str...
...
```

**【练习】**  
1.看看你能stop什么东西？  
2.看看你能对process做什么操作？  
3.使用命令Get-Verb看一下PowerShell中都有那些定义好的动词。

## Cmdlet使用标准的参数

如前面命令中用到的参数`-Name`，PowerShell对于命令中参数的使用，也是使用相同的规则，都是以“-”开始。

**【例子】**  
比如我们前面用到过的一个例子：  
```bash
PS C:\Users\Frank\Desktop\fps> Get-Process -Name notepad

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    403      23     8484      32080       0.55  14116   1 notepad
```
假如我们知道notepad这个应用的进程的当前的ID值，我们也可以通过`-ID`这个参数来查看notepad的进程状态
```bash
PS C:\Users\Frank\Desktop\fps> Get-Process -Id 14116

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    403      23     8484      32080       0.55  14116   1 notepad
```
我们可以看到，ID这个参数是“-ID”。

不同的PowerShell命令也会有`-Name`,`-ID`这样的参数，起着同样的作用。


```python

```
