# 持续会话

每次使用`Invoke-Command`或`Enter-PSSession`链接远程计算机时，你至少需要指定计算机名称，有时还需要输入账号密码。

有什么办法可以建立一个持久的会话，而不用每次都重复输入这些内容。

你可以通过`New-PSSession`创建一个新的会话，指定一个或多个计算机名称。结果是一个存在PowerShell内存中的对话对象。

```powershell
PS C:\Windows\system32> New-PSSession -ComputerName localhost

 Id Name            ComputerName    ComputerType    State         ConfigurationName     Availability
 -- ----            ------------    ------------    -----         -----------------     ------------
  1 WinRM1          localhost       RemoteMachine   Opened        Microsoft.PowerShell     Available


PS C:\Windows\system32> Get-PSSession

 Id Name            ComputerName    ComputerType    State         ConfigurationName     Availability
 -- ----            ------------    ------------    -----         -----------------     ------------
  1 WinRM1          localhost       RemoteMachine   Opened        Microsoft.PowerShell     Available


PS C:\Windows\system32> Get-PSSession | gm


   TypeName: System.Management.Automation.Runspaces.PSSession

Name                   MemberType     Definition
----                   ----------     ----------
Equals                 Method         bool Equals(System.Object obj)
GetHashCode            Method         int GetHashCode()
GetType                Method         type GetType()
```

更常用的做法是把常用的一组会话存成一个变量。但是要注意，会话会消耗资源，如果关闭Shell，这些会话也会随之关闭。

通过命令`Remove-PSSession`可以关闭会话。

# 一次创建多个会话

```powershell
$s_server1,$s_server2 = new-pssession -computer server-r2,dc01
```

该语法将连接到Server-R2的护花存入变量$s_server1，将连接到DC01的会话存入$s_server2。

# 使用创建好的会话

```powershell
Enter-PSSession -session $session
Invoke-Command -session $session
```



