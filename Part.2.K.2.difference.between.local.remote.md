# 远程命令和本地命令的差异

## Invoke-Command和-ComputerName

```powershell
PS D:\UserData\oe-vfrank\Scripts> Invoke-Command -ComputerName server01,server02 -Command {Get-Ev
entLog Security -Newest 5}

   Index Time          EntryType   Source                 InstanceID Message              PSComputerName
   ----- ----          ---------   ------                 ---------- -------              --------------
13312504 Aug 11 22:31  SuccessA... HostIDS                      1901 The description f... server01
13312503 Aug 11 22:31  SuccessA... Microsoft-Windows...         4689 A process has exi... server01
13312502 Aug 11 22:31  SuccessA... Microsoft-Windows...         4689 A process has exi... server01
13312501 Aug 11 22:31  SuccessA... Microsoft-Windows...         4689 A process has exi... server01
13312500 Aug 11 22:31  FailureA... Microsoft-Windows...         5157 The Windows Filte... server01
14013485 Aug 11 22:31  SuccessA... Microsoft-Windows...         4624 An account was su... server02
14013484 Aug 11 22:31  SuccessA... Microsoft-Windows...         4672 Special privilege... server02
14013483 Aug 11 22:31  SuccessA... HostIDS                       610 The description f... server02
14013482 Aug 11 22:31  SuccessA... HostIDS                       610 The description f... server02
14013481 Aug 11 22:31  SuccessA... Microsoft-Windows...         4624 An account was su... server02


PS D:\UserData\oe-vfrank\Scripts> Get-EventLog Security -Newest 5 -ComputerName server01,server02


   Index Time          EntryType   Source                 InstanceID Message
   ----- ----          ---------   ------                 ---------- -------
13312544 Aug 11 22:32  SuccessA... HostIDS                      1901 The description for Event ID '1901' i...
13312543 Aug 11 22:32  SuccessA... Microsoft-Windows...         4689 A process has exited....
13312542 Aug 11 22:32  SuccessA... Microsoft-Windows...         4689 A process has exited....
13312541 Aug 11 22:32  SuccessA... Microsoft-Windows...         4689 A process has exited....
13312540 Aug 11 22:32  SuccessA... Microsoft-Windows...         4662 An operation was performed on an obje...
14013508 Aug 11 22:32  FailureA... Microsoft-Windows...         5157 The Windows Filtering Platform has bl...
14013507 Aug 11 22:32  SuccessA... HostIDS                      1901 The description for Event ID '1901' i...
14013506 Aug 11 22:32  SuccessA... HostIDS                      1501 The description for Event ID '1501' i...
14013505 Aug 11 22:32  SuccessA... HostIDS                      1901 The description for Event ID '1901' i...
14013504 Aug 11 22:32  FailureA... Microsoft-Windows...         5157 The Windows Filtering Platform has bl...
```

不同点：
1.-ComputerName中的计算机被串行访问，而不会采用并行方式。
2.输出结果不包含PSComputerName属性，我们很难判断结果从哪台计算机得出。
3.不使用WinRM实现，使用.NET FrameWork决定的底层协议。有可能无法在本地和远程计算机之间通过防火墙而无法建立连接。
4.如果有进一步筛选，会先获取结果，再在本地筛选，Invoke会在远程筛选，再传回到本地
5.得到的是完整的对象，Invoke是序列化为XML然后再反序列化为相应对象，会限制本地计算机处理这些对象的方式。

一般来讲，使用Invoke-Command命令比Cmdlet的-ComputerName参数更有效率。

## 反序列化对象

```powershell
PS D:\UserData\oe-vfrank\Scripts> hostname
server1
PS D:\UserData\oe-vfrank\Scripts> get-service -ComputerName Server2 | gm -Type Method

   TypeName: System.ServiceProcess.ServiceController

Name                      MemberType Definition
----                      ---------- ----------
Close                     Method     void Close()
Continue                  Method     void Continue()
CreateObjRef              Method     System.Runtime.Remoting.ObjRef CreateObjRef(type requestedType)
Dispose                   Method     void Dispose(), void IDisposable.Dispose()
Equals                    Method     bool Equals(System.Object obj)
ExecuteCommand            Method     void ExecuteCommand(int command)
GetHashCode               Method     int GetHashCode()
GetLifetimeService        Method     System.Object GetLifetimeService()
GetType                   Method     type GetType()
InitializeLifetimeService Method     System.Object InitializeLifetimeService()
Pause                     Method     void Pause()
Refresh                   Method     void Refresh()
Start                     Method     void Start(), void Start(string[] args)
Stop                      Method     void Stop()
WaitForStatus             Method     void WaitForStatus(System.ServiceProcess.ServiceControllerStatus desi...


PS D:\UserData\oe-vfrank\Scripts> Invoke-Command -ComputerName server2 -Command {get-service} | gm
 -Type Method

   TypeName: Deserialized.System.ServiceProcess.ServiceController

Name     MemberType Definition
----     ---------- ----------
GetType  Method     type GetType()
ToString Method     string ToString(), string ToString(string format, System.IFormatProvider formatProvide...
```