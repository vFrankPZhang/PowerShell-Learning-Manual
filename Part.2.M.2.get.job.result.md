# 获取作业执行结果

## 获取Job执行状态

通过Get-Job可以查看Job的运行状态。

```powershell
PS C:\WINDOWS\system32> Get-Job job1

Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Job1            BackgroundJob   Completed     True            localhost            dir
```

通过Format-List *可以看到更详细的信息，其中很重要的一个是ChildJobs。

```powershell
PS C:\WINDOWS\system32> Get-Job job1 | fl *


State         : Completed
HasMoreData   : True
StatusMessage :
Location      : localhost
Command       : dir
JobStateInfo  : Completed
Finished      : System.Threading.ManualResetEvent
InstanceId    : 14e4caa8-c19d-4147-a77c-309c6ce9cea2
Id            : 1
Name          : Job1
ChildJobs     : {Job2}
PSBeginTime   : 2019/8/21 14:22:43
PSEndTime     : 2019/8/21 14:22:46
PSJobTypeName : BackgroundJob
Output        : {}
Error         : {}
Progress      : {}
Verbose       : {}
Debug         : {}
Warning       : {}
Information   : {}
```

## 获取Job执行结果

`Receive-Job`命令可以获取Job执行的结果。

### 注意

正常情况下，当获取了一个作业的返回结果之后，会自动在作业的输出缓存中清除对应的数据，这样你不能再次获取他们。

通过`-keep`参数，在内存中保留输出结果的一份拷贝。或者通过输出命令将结果输出到CliXML中。

作业结果课呢鞥是反序列化的，即对象的很多方法会丢掉。

```powershell
PS C:\WINDOWS\system32> Receive-Job job1 -Keep


    目录: C:\Users\Frank\Documents


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2018/9/23     15:32                3dsMax
d-----         2019/5/9     15:06                Adobe
d-----        2019/8/14      8:23                Ashampoo Burning Studio 20
```

这里有一个现象，我们PowerShell所在的路径是`C:\WINDOWS\system32>`，但是Job所查询的路径确是`C:\Users\Frank\Documents`，不要去猜测Job的执行路径，所以使用绝对录几个确保你想要的结果，这也是重要的习惯。

```powershell
PS C:\WINDOWS\system32> Get-Job

Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Job1            BackgroundJob   Completed     False           localhost            dir
3      Job3            BackgroundJob   Failed        False           localhost            Get-Event...
```

`Get-Job`时，HasMoreData中为False表示，内存中已经没有Job的执行结果了。

