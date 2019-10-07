# 管理作业

Remove-Job 移除作业，包括内存中该作业的输出结果。

例如移除已经获得了结果的作业：

```powershell
Get-Job | Where {-Not $_.HasMoreData} | Remove-Job
```

Stop-Job   停止作业，仍然可以获取截止到此时此刻产生的结果。

Wait-Job   当使用一段脚本开启一个作业，同时希望该脚本在作业运行完毕后继续执行。
