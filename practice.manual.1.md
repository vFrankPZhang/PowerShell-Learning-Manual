
# 改变计算机状态

## 注销

```bash
(Get-WmiObject -Class Win32_OperatingSystem -ComputerName .).Win32Shutdown(0)
```

> [Win32_OperatingSystem class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-operatingsystem#members)

## 关机或重启

```bash
Stop-Computer                         #关机
Restart-Computer                      #重启
Restart-Computer -Force               #强制重启
```

# 获取计算机信息

## 桌面设置

```bash
Get-CimInstance -ClassName Win32_Desktop -ComputerName .
```

## BIOS信息

```bash
Get-CimInstance -ClassName Win32_BIOS -ComputerName .
```

> [Win32_BIOS class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-bios)

## 处理器信息

```bash
Get-CimInstance -ClassName Win32_Processor -ComputerName .
```

## 计算机制造商和型号

```bash
Get-CimInstance -ClassName Win32_ComputerSystem
```

> [Win32_ComputerSystem class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-computersystem)

## 列出安装的热补丁

```bash
Get-CimInstance -ClassName Win32_QuickFixEngineering -ComputerName .
```

> [Win32_QuickFixEngineering class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-quickfixengineering)

## OS版本信息

```bash
Get-CimInstance -ClassName Win32_OperatingSystem -ComputerName . | Select-Object -Property BuildNumber,BuildType,OSType,ServicePackMajorVersion,ServicePackMinorVersion
```

> [Win32_OperatingSystem class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-operatingsystem#members)

## 可用磁盘空间

固定磁盘的类型是：DriveType 3

```powershell
Get-CimInstance -ClassName Win32_LogicalDisk -Filter "DriveType=3" -ComputerName .
```

求和：

```powershell
Get-CimInstance -ClassName Win32_LogicalDisk -Filter "DriveType=3" -ComputerName . | Measure-Object -Property FreeSpace,Size -Sum | Select-Object -Property Property,Sum
```

> [Win32_LogicalDisk class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-logicaldisk)

## 获取登陆到特定计算机上的用户

```powershell
Get-CimInstance -ClassName Win32_ComputerSystem -ComputerName . | Select-Object UserName
```

> [Win32_ComputerSystem class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-computersystem)

## 从一个机器上获取本地时间

```powershell
Get-CimInstance -ClassName Win32_LocalTime -ComputerName .
```

> [Win32_LocalTime class](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/wmitimepprov/win32-localtime)

# 通过`-FilterHashtable`创建Get-WinEvent查询

## 通过Porvider筛选

```powershell
Get-WinEvent -FilterHashtable @{
    LogName='Application'
    ProviderName='.NET Runtime'
}
```

## 通过ID筛选

```powershell
Get-WinEvent -FilterHashtable @{
    LogName='Application'
    ProviderName='.NET Runtime'
    ID=1023
}
```

# 操纵项目

```powershell
PS C:\Windows\system32> gcm -Noun Item

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Clear-Item                                         3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Copy-Item                                          3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Get-Item                                           3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Invoke-Item                                        3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Move-Item                                          3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          New-Item                                           3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Remove-Item                                        3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Rename-Item                                        3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Set-Item                                           3.1.0.0    Microsoft.PowerShell.Management
```

## 新建项目

```powershell
New-Item -Path -ItemType Directory/File
```

## 重命名项目

```powershell
Rename-Item -Path C:\temp\New.Directory\file1.txt fileOne.txt
```

## 移动项目

```powershell
Move-Item -Path C:\temp\New.Directory -Destination C:\ -PassThru
```

## 复制项目

```powershell
Copy-Item -Path C:\New.Directory -Destination C:\temp
```

## 删除项目

```powershell
Remove-Item C:\New.Directory
```

## 执行一个项目

对项目用默认的操作去执行，比如，目标是一个目录，打开它，目标是一个PPT文件，用PowerPoint打开它。

```powershell
Invoke-Item C:\WINDOWS
```

# 执行网络任务

## 列出某台机器上使用中的IP地址

```powershell
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=$true -ComputerName . | Format-Table -Property IPAddress
```

> [Win32_NetworkAdapterConfiguration class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-networkadapterconfiguration)

## 列出IP配置

列出每个网卡的IP配置信息：

```powershell
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=$true -ComputerName .
```


## Ping

```powershell
Get-WmiObject -Class Win32_PingStatus -Filter "Address='www.baidu.com'" -ComputerName . | Format-Table -Property Address,ResponseTime,StatusCode -Autosize
```

通过数组、ForEach-Object，ping多个地址：

```powershell
'127.0.0.1','localhost','research.microsoft.com' | ForEach-Object -Process {Get-WmiObject -Class Win32_PingStatus -Filter ("Address='" + $_ + "'") -ComputerName .} | Select-Object -Property Address,ResponseTime,StatusCode
```

测试某个子网网段的地址：

```powershell
1..254| ForEach-Object -Process {Get-WmiObject -Class Win32_PingStatus -Filter ("Address='192.168.1." + $_ + "'") -ComputerName .} | Select-Object -Property Address,ResponseTime,StatusCode
```



## 获取网卡属性

前面用到的Win32_NetworkAdapterConfiguration更多的是网卡的配置信息如IP地址之类的，要想获取MAC地址等网卡的属性信息：

```powershell
Get-WmiObject -Class Win32_NetworkAdapter -ComputerName .
```

> [Win32_NetworkAdapter class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-networkadapter)

## 为网卡指定一个DNS Domain

通过SetDNSDomain方法，为网卡指定DNS Domain

```bash
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=$true -ComputerName . | ForEach-Object -Process { $_. SetDNSDomain('fabrikam.com') }
```

> [Win32_NetworkAdapterConfiguration class](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-networkadapterconfiguration)

## 配置DHCP

**获取开启DHCP的网卡**

```bash
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "DHCPEnabled=$true" -ComputerName .
```

**获取只配了IP的网卡**

```bash
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "IPEnabled=$true and DHCPEnabled=$true" -ComputerName .
```

## 创建网络共享

使用Win32_Share Create方法来创建网络共享

```bash
(Get-WmiObject -List -ComputerName . | Where-Object -FilterScript {$_.Name -eq 'Win32_Share'}).Create('C:\temp','TempShare',0,25,'test share of the temp folder')
```

## 移除网络共享

```powershell
(Get-WmiObject -Class Win32_Share -ComputerName . -Filter "Name='TempShare'").Delete()
```

```bash
PS> net share tempshare /delete
```


## 创建Windows PowerShell网络驱动器
```powershell
(New-Object -ComObject WScript.Network).MapNetworkDrive('B:', '\\FPS01\users')
```

# 格式化输出

## Format-Wide

按列宽显示：

```powershell
Get-Command -Verb Format | Format-Wide
```

通过参数`-Column`可以指定显示列宽数

## Format-List

按列显示：

```powershell
Get-Process -Name powershell | Format-List
```

显示详情，等于把Get-Member里面的内容显示出来：

```powershell
Get-Process -Name powershell | Format-List -Property *
```

## Format-Table

```powershell
Get-Process -Name powershell | Format-Table
```

## AutoSize

自动换行

完整显示信息，直到屏幕末端，以...显示。越靠近开头的越重要，越完整显示。

```powershell
Get-Process -Name powershell | Format-Table -Property Path,Name,Id,Company -AutoSize
```

## Wrap自动换行

显示不完整，自动换行显示，结合Autosize一起使用效果会更好，如果不使用autosize，不同列显示间距可能会比较大，导致某一行没有足够的空间显示 ，就会换行成很多行。

使用Wrap跟使用AutoSize的一个区别是，前面都一样，只是最后一行，会换行。

```powershell
Get-Process -Name powershell | Format-Table -Wrap -AutoSize -Property Name,Id,Company,Path
```

## GroupBy

以某一属性分组：

```powershell
Get-Process -Name powershell | Format-Table -Wrap -AutoSize -Property Name,Id,Path -GroupBy Company
```

# 处理文件和文件夹

## 列出文件夹下的文件

显示一个文件夹的所有文件，包括隐藏的，但不含子目录。

```powershell
Get-ChildItem -Path C:\ -Force
```

如果需要包含子文件，需要加参数`-Recurse`

通过PowerShell进行复杂的筛选：

```bash
Get-ChildItem -Path $env:ProgramFiles -Recurse -Include *.exe | Where-Object -FilterScript {($_.LastWriteTime -gt '2005-10-01') -and ($_.Length -ge 1mb) -and ($_.Length -le 10mb)}
```

## 复制文件和文件夹

```bash
Copy-Item -Path C:\boot.ini -Destination C:\boot.bak
```

对于已经存在的文件，进行覆盖，使用参数`-Force`,哪怕目标是只读。
```bash
Copy-Item -Path C:\boot.ini -Destination C:\boot.bak -Force
```

拷贝文件：

```bash
Copy-Item C:\temp\test1 -Recurse C:\temp\DeleteMe
```

可以对需要考的文件做过滤

```bash
Copy-Item -Filter *.txt -Path c:\data -Recurse -Destination C:\temp\text
```

**使用Scripting.FileSystem COM类进行备份**

```bash
(New-Object -ComObject Scripting.FileSystemObject).CopyFile('C:\boot.ini', 'C:\boot.bak')
```

## 创建文件和文件夹

```bash
New-Item -Path 'C:\temp\New Folder' -ItemType Directory
New-Item -Path 'C:\temp\New Folder\file.txt' -ItemType File
```

## 删除文件夹下的所有文件

```bash
Remove-Item -Path C:\temp\DeleteMe
```

当该文件下有内容时，会有提示，确认删除，如果使用参数`-Recurse`就可以直接删除了。

## 将本地文件夹映射成一个Windows驱动器

```cmd
subst p: $env:programfiles
```




















