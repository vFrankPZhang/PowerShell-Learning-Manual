# 同时处理多个对象

## 首选支持批处理的cmdlet

```powershell
Get-Service -Name BITS,Spooler,W32Time -Computer Server1,Server2,Server3 | Set-Service -StartupType Automatic
```

有一个问题，上面的命令执行的动作通常不会反悔表示运行状态的结果。这意味着上面两个命令都不会产生可视化结果，可以使用passThru参数，该参数用于打印出该命令所接受的对象。

```powershell
Get-Service -name BITS -computer Server1,Server2,Server3 | start-Service -passthru | Out-File NewServiceStatus.txt
```

该命令将会从3台计算机列表中获取指定的服务，然后通过管道将这些服务传递给Start-Service。该命令不仅会启动服务，而且会将涉及的服务对象打印在屏幕上，然后通过Out-File输出。

## CIM/WMI调用方法

为计算机所有Realtek网卡启用DHCP

```powershell
gwmi Win32_NetworkAdapterConfiguration -Filter "Description like 'realtek%'"
gwmi Win32_NetworkAdapterConfiguration | ? description -like 'realtek*'
```

gwmi中直接使用过滤，用%表示通配符*。

然后Get-Member查看方法，发现EnableDHCP方法。

然后使用Invoke-WmiMethod，来调用WMI的方法

```powershell
gwmi Win32_NetworkAdapterConfiguration -Filter "Description like 'realtek%'" | Invoke-WmiMethod -name EnableDHCP

__GENUS          : 2
__CLASS          : __PARAMETERS
__SUPERCLASS     :
__DYNASTY        : __PARAMETERS
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
ReturnValue      : 0
PSComputerName   :

__GENUS          : 2
__CLASS          : __PARAMETERS
__SUPERCLASS     :
__DYNASTY        : __PARAMETERS
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
ReturnValue      : 84
PSComputerName   :
```

搜索说明，查看ReturnValue的意思是什么。

## 枚举

无论是Get-Service还是Set-Service都无法显示或者是某个服务的登录密码。

win32_Service中有一个方法是change()

通过官网查到change()的语法：

```
uint32 Change(
  [in] string  DisplayName,
  [in] string  PathName,
  [in] uint32  ServiceType,
  [in] uint32  ErrorControl,
  [in] string  StartMode,
  [in] boolean DesktopInteract,
  [in] string  StartName,
  [in] string  StartPassword,
  [in] string  LoadOrderGroup,
  [in] string  LoadOrderGroupDependencies[],
  [in] string  ServiceDependencies[]
);
```

我们看到第8项是StartPassword。

```powershell
gwmi win32_service -filter "name =  'BITS'" | invoke-wmimethod -name change -arg $null,$null,$null,$null,$null,$null,$null,"P@ssw0rd"
```

会报错，没有办法这么用。

这时，我们可以通过枚举实现：

```powershell
gwmi-wmiobject win32_service -filter "name = 'BITS'" | foreach-object -process {$_.change ($null,$null,$null,$null,$null,$null,$null,"P@ssw0rd")}
```

4个不同的命令结束Notepad进程
1.cmdlet  `Get-Process notepad | Stop-Process`
2.method  `gwmi win32_process | ? Name -like '*notepad*' | Invoke-WmiMethod -Name Terminate`
3.foreach `Get-Process notepad | foreach {$_.Kill()}`