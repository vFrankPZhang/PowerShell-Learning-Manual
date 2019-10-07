# WMI（Windows Management Instrumentation） Windows管理规范

WMI是微软提供给管理员使用的工具，是一个外部技术，PowerShell仅仅与其接口交互。

通过WMI可以易于访问Windows的信息。

### 例子

我们通过Get-Disk获取到磁盘的信息，但是获取不到分区的信息，或者逻辑磁盘的信息，我们可以通过WMI获取到磁盘的信息。

```powershell
PS C:\Users\zhang.peng5> Get-WmiObject win32_logicaldisk


DeviceID     : C:
DriveType    : 3
ProviderName :
FreeSpace    : 76375494656
Size         : 161061269504
VolumeName   :

DeviceID     : D:
DriveType    : 3
ProviderName :
FreeSpace    : 258940956672
Size         : 350402637824
VolumeName   :
```

root\CIMv2包含了Windows操作系统和计算机硬件信息。  
root\MicrosoftDNS包含了所有关于DNS服务器的信息。  
root\SecurityCenter包含了关于防火墙、杀毒软件和反流氓软件等工具的信息。

通过MMC上的WMI控制单元查看命名空间：

![wmic](images/wmic.png)

root下面的这些称为NameSpace，win32_logicaldisk称为class。  
可以理解为，class是一个一个接口，NameSpace是这类接口的容器、文件夹。

## 默认NameSpace

通过`-list`参数，可以查看到NameSpace里面的有哪些class。

```powershell
gwmi -List
```

我们从中筛选出win32_logicaldisk这个class

```powershell
PS C:\WINDOWS\system32> gwmi -List | ? name -eq win32_logicaldisk

   NameSpace:ROOT\cimv2

Name                                Methods              Properties
----                                -------              ----------
Win32_LogicalDisk                   {SetPowerState, R... {Access, Availability, BlockSize, Caption...}
```

我们可以看到有一行信息是：NameSpace：ROOT\cimv2。这就是Win32_LogicalDisk所在的NameSpace，也是我们不加任何NameSpace参数时，PowerShell默认去查找的NameSpace。

## 查看其它NameSpace下的class

通过指定NameSpace，我们当然可以看到其它NameSpace下的class，比如root\SecurityCenter

```powershell
PS C:\WINDOWS\system32> gwmi -Namespace root\securitycenter -List

   NameSpace:ROOT\securitycenter

Name                                Methods              Properties
----                                -------              ----------
__SystemClass                       {}                   {}
__thisNAMESPACE                     {}                   {SECURITY_DESCRIPTOR}
__Provider                          {}                   {Name}
__Win32Provider                     {}                   {ClientLoadableCLSID, CLSID, Concurrency, DefaultMachineName...}
__NAMESPACE                         {}                   {Name}
__ProviderRegistration              {}                   {provider}
__EventProviderRegistration         {}                   {EventQueryList, provider}
__ObjectProviderRegistration        {}                   {InteractionType, provider, QuerySupportLevels, SupportsBatching...}
__ClassProviderRegistration         {}                   {CacheRefreshInterval, InteractionType, PerUserSchema, provider...}
__InstanceProviderRegistration      {}                   {InteractionType, provider, QuerySupportLevels, SupportsBatching...}
```

我们可以查看一下我的系统中的杀毒软件的信息：

```powershell
PS C:\WINDOWS\system32> gwmi -Namespace root\securitycenter -Class AntiVirusProduct

__GENUS                      : 2
__CLASS                      : AntiVirusProduct
__SUPERCLASS                 :
__DYNASTY                    : AntiVirusProduct
__RELPATH                    : AntiVirusProduct.instanceGuid="{5EEE8B0C-BEB2-4f05-BA7E-5EF3A65B8ECC}"
__PROPERTY_COUNT             : 10
__DERIVATION                 : {}
__SERVER                     : DESKTOP-4V08ISJ
__NAMESPACE                  : ROOT\securitycenter
__PATH                       : \\DESKTOP-4V08ISJ\ROOT\securitycenter:AntiVirusProduct.instanceGuid="{5EEE8B0C-BEB2-4f05-BA7E-5EF3A65B8ECC}"
companyName                  : 360.cn
displayName                  : 360安全卫士
instanceGuid                 : {5EEE8B0C-BEB2-4f05-BA7E-5EF3A65B8ECC}
onAccessScanningEnabled      : False
pathToSignedProductExe       : C:\Program Files (x86)\360\360safe\safemon\360tray.exe
productHasNotifiedUser       : True
productState                 :
productUptoDate              : True
productWantsWscNotifications : False
versionNumber                : 12, 0, 0, 1091
PSComputerName               : DESKTOP-4V08ISJ
```

我们也可以验证一下，在NameSpace root\securitycenter下我们能找到 AntivirusProduct但是找不到win32_logicaldisk。

```powershell
PS C:\WINDOWS\system32> gwmi -Namespace root\securitycenter -list| ? name -eq AntiVirusProduct


   NameSpace:ROOT\securitycenter

Name                                Methods              Properties
----                                -------              ----------
AntiVirusProduct                    {}                   {companyName, displayName, instanceGuid, onAccessScanningEnabled...}

PS C:\WINDOWS\system32> gwmi -Namespace root\securitycenter -List | ? name -eq win32_logicaldisk
PS C:\WINDOWS\system32>

```
