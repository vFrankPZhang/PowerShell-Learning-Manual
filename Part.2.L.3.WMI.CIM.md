# WMI还是CIM

## CIM是什么

我们在获取class的时候，经常能看到一些类命名很有规律，以CIM开头。

```powershell
PS C:\WINDOWS\system32> gwmi -List | ? name -like 'CIM*'

   NameSpace:ROOT\cimv2

Name                                Methods              Properties
----                                -------              ----------
CIM_ManagedSystemElement            {}                   {Caption, Description, InstallDate, Name...}
CIM_PhysicalElement                 {}                   {Caption, CreationClassName, Description, InstallDate...}
CIM_PhysicalPackage                 {IsCompatible}       {Caption, CreationClassName, Depth, Description...}
CIM_Card                            {IsCompatible}       {Caption, CreationClassName, Depth, Description...}
CIM_PhysicalFrame                   {IsCompatible}       {AudibleAlarm, BreachDescription, CableManagementStrategy, Caption...}
CIM_Chassis                         {IsCompatible}       {AudibleAlarm, BreachDescription, CableManagementStrategy, Caption...}
CIM_Rack                            {IsCompatible}       {AudibleAlarm, BreachDescription, CableManagementStrategy, Caption...}
CIM_PhysicalComponent               {}                   {Caption, CreationClassName, Description, HotSwappable...}
CIM_PhysicalMedia                   {}                   {Capacity, Caption, CleanerMedia, CreationClassName...}
CIM_Chip                            {}                   {Caption, CreationClassName, Description, FormFactor...}
CIM_PhysicalMemory                  {}                   {BankLabel, Capacity, Caption, CreationClassName...}
CIM_PhysicalConnector               {}                   {Caption, ConnectorPinout, ConnectorType, CreationClassName...}
CIM_Slot                            {}                   {Caption, ConnectorPinout, ConnectorType, CreationClassName...}
```

从初步对WMI的接触我们可以感受到，如何找到我们想要的class并不是那么方便，需要积累很多经验，对象的属性代表什么意思也并不直观，需要查。

在WMI的生命周期中，微软并没有画很大的精力控制其规范，这就使得WMI变得混乱。很少有类提供修改配置的方法，更多的是只读的属性，很多团队也把注意力集中在了PowerShell cmdlets上，其实很多cmdlets后台调用的也是WMI接口。

在PowerShell v3及以后，有了大量的CIM命令，你可以理解为是新版的WMI命令，这些命令是对WMI的某些部分进行了封装，从而提供类似PowerShell的交互方式。

另外，WMI Cmdlets他们与远程调用(RPC)交互，也就是说，只有在防火墙支持状态审查时才能通过防火墙。CIM通过WS-MAN交互，代替原有的RPCs。

后端同样是WMI，差异在于如何交互和如何被使用。

## 命令差别

命令上，类似`Get-WmiObject`的CIM命令是`Get-CimInstance`，使用`-ClassName`代替`-Class`，没有`-List`参数，使用`Get-CimClass`命令和`-NameSpace`参数获取Class。

`Get-CIMInstance`支持补全，无论是`Win32_LogicalDisk`还是`CIM_LogicalDisk`，得到的对象都是一样的。
