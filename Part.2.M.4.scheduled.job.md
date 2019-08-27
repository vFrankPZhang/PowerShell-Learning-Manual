# 计划作业

类似于Windows的计划任务，PowerShell支持计划作业。

通过命令`New-JobTrigger`创建一个计划Job的触发器，主要用于定义Job的运行时间。

之后使用`Register-ScheduledJob`将该作业注册到计划任务程序中。该命令采用计划任务程序中的XML格式来创建作业的定义，之后在磁盘上新建一个层级结构的文件夹存放每次作业运行的结果。

```powershell
Register-ScheduledJob -Name DailyProcList -ScriptBlock {Get-Process} -Trigger (New-JobTrigger -Daily -At 2am) -ScheduledJobOption (New-ScheduledJobOption -WakeToRun -RunElevated)
```

调度作业跟常规作业不同，获取结果之后并不会删除Job结果，因为调度作业的结果保存在缓存中。

移除作业时，对应的结果也会从磁盘上被移除。可以通过`Register-ScheduledJob`命令的`-MaxResultCount`参数控制存放的结果数量。
