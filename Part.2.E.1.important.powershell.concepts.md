
# 理解重要的PowerShell概念

## 基于对象

前面在介绍PowerShell是什么的时候，我们提到了PowerShell是基于对象的，今天我们就解释一下PowerShell的基于对象。

在计算机科学中，有一个词叫Shell，俗称壳（用来区别于核），是指为使用者提供操作界面的软件。

Linux中Bash是基于文本的CLI Shell，PowerShell是基于对象的CLI Shell。

### 【例子】**  

在Linux中我想用Kill命令结束掉以字母p开头的进程。

以Ubuntu为例，我需要运行下面的命令：

```bash
ps -e | grep " p" | awk '{ print $1 }' | xargs kill
```

这条命令做了什么事情呢？

因为需要对kill命令提供PID来结束进程，所以我先通过`ps -e`获取了当前进程的清单，然后通过`grep`命令搜索了以“p”开头的进程。

![Ubuntu:ps -e](images\ubuntu_1.png)

![Ubuntu:ps -e | grep " p"](images\ubuntu_2.png)

接下来，又通过`awk`命令，选取了第1列，第1列是PID信息。

![Ubuntu:ps -e | grep " p" | awk '{ print $1 }'](images\ubuntu_3.png)

最后通过`kill`命令结束了进程。

类似的，如果要在PowerShell中执行相同的命令，只需要执行下面的操作：

```bash
Get-Process p* | Stop-Process
```

PowerShell中的`Get-Process`命令查找了以p开头的进程，然后通过管道输出给`Stop-Process`命令结束掉。

这两者在工作逻辑上是不同的，对于基于文本的，我需要PID信息，我就通过各种方式从一堆信息中心把PID拿到，去用。对于基于对象的，我需要进程，我就直接拿到进程。

## 命令是可以扩展的

不同于传统的cmd.exe这样的命令行工具，你不能直接的扩展自己想用的命令，对于PowerShell，内置的命令我们称为cmdlet，除了cmdlet，你可以通过编译的代码或者脚本，创建你自己的模块和功能，来扩展PowerShell命令行的功能。

### 【例子】

通过`Get-Command *-Disk`命令我们可以看到PowerShell默认的跟Dsik相关的所有命令。

![extension command](images\extension_command.jpg)

我现在想要获取C盘的可用空间，我发现没有现成的命令。

通过下面我自己写的简单的脚本，就可以实现这个需求了。

```powershell
function get-diskCFreeSpace {
    param (
        $computername=$env:COMPUTERNAME
    )
    Get-WmiObject win32_logicaldisk | Where-Object {$_.deviceid -eq 'c:'} | Format-Table @{name="FreeSpace(GB)";expression={$_.freespace/1gb};formatstring='F2'}
}
```

![extension command cdisk](images\extension_command_cdisk.jpg)

## PowerShell使用一些C#的语法

PowerShell 是基于.NET Framework建立的。它跟C#有这一些相同的语法和关键字。学习PowerShell可以让你更容易去学习C#，如果你已经比较熟悉C#，你也会很容易上手PowerShell。
