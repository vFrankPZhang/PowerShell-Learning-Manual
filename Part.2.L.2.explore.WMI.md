# 探索WMI

我们通过WMI Control能够看到有哪些NameSpace，但是看不到下面的Class，也不知道怎么使用。

我们可以借助微软的官方文档，来了解class的用法及属性的意义。

### 【例子】

比如对于磁盘，我们前面提到过，root\CIM包含了Windows操作系统和计算机硬件信息，所以我们才从这个NameSpace里面去找。

```powershell
PS C:\WINDOWS\system32> gwmi -List | ? name -like *disk*

   NameSpace:ROOT\cimv2

Name                                Methods              Properties
----                                -------              ----------
CIM_LogicalDisk                     {SetPowerState, R... {Access, Availability, BlockSize, Caption...}
Win32_LogicalDisk                   {SetPowerState, R... {Access, Availability, BlockSize, Caption...}
Win32_MappedLogicalDisk             {SetPowerState, R... {Access, Availability, BlockSize, Caption...}
CIM_DiskPartition                   {SetPowerState, R... {Access, Availability, BlockSize, Bootable...}
Win32_DiskPartition                 {SetPowerState, R... {Access, Availability, BlockSize, Bootable...}
CIM_DiskDrive                       {SetPowerState, R... {Availability, Capabilities, CapabilityDescriptions, Caption...}
Win32_DiskDrive                     {SetPowerState, R... {Availability, BytesPerSector, Capabilities, CapabilityDescriptions...}
CIM_DisketteDrive                   {SetPowerState, R... {Availability, Capabilities, CapabilityDescriptions, Caption...}
Win32_LogicalDiskRootDirectory      {}                   {GroupComponent, PartComponent}
CIM_LogicalDiskBasedOnPartition     {}                   {Antecedent, Dependent, EndingAddress, StartingAddress}
Win32_LogicalDiskToPartition        {}                   {Antecedent, Dependent, EndingAddress, StartingAddress}
CIM_LogicalDiskBasedOnVolumeSet     {}                   {Antecedent, Dependent, EndingAddress, StartingAddress}
Win32_DiskDriveToDiskPartition      {}                   {Antecedent, Dependent}
CIM_RealizesDiskPartition           {}                   {Antecedent, Dependent, StartingAddress}
Win32_DiskDrivePhysicalMedia        {}                   {Antecedent, Dependent}
Win32_LogonSessionMappedDisk        {}                   {Antecedent, Dependent}
Win32_DiskQuota                     {}                   {DiskSpaceUsed, Limit, QuotaVolume, Status...}
CIM_DiskSpaceCheck                  {Invoke}             {AvailableDiskSpace, Caption, CheckID, CheckMode...}
Win32_OfflineFilesDiskSpaceLimit    {}                   {AutoCacheSizeInMB, TotalSizeInMB}
Win32_PerfFormattedData_Counters... {}                   {Caption, Description, FileSystemBytesRead, FileSystemBytesWritten...}
Win32_PerfRawData_Counters_FileS... {}                   {Caption, Description, FileSystemBytesRead, FileSystemBytesWritten...}
Win32_PerfFormattedData_Counters... {}                   {Caption, Description, Frequency_Object, Frequency_PerfTime...}
Win32_PerfRawData_Counters_Stora... {}                   {Caption, Description, Frequency_Object, Frequency_PerfTime...}
Win32_PerfFormattedData_PerfDisk... {}                   {AvgDiskBytesPerRead, AvgDiskBytesPerTransfer, AvgDiskBytesPerWrite, AvgDiskQueueLength...}
Win32_PerfRawData_PerfDisk_Logic... {}                   {AvgDiskBytesPerRead, AvgDiskBytesPerRead_Base, AvgDiskBytesPerTransfer, AvgDiskBytesPerT...
Win32_PerfFormattedData_PerfDisk... {}                   {AvgDiskBytesPerRead, AvgDiskBytesPerTransfer, AvgDiskBytesPerWrite, AvgDiskQueueLength...}
Win32_PerfRawData_PerfDisk_Physi... {}                   {AvgDiskBytesPerRead, AvgDiskBytesPerRead_Base, AvgDiskBytesPerTra
```

接着我们看一下这个对象里面都有哪些属性和方法。

```powershell
PS C:\WINDOWS\system32> gwmi win32_logicaldisk | gm


   TypeName:System.Management.ManagementObject#root\cimv2\Win32_LogicalDisk

Name                         MemberType    Definition
----                         ----------    ----------
PSComputerName               AliasProperty PSComputerName = __SERVER
Chkdsk                       Method        System.Management.ManagementBaseObject Chkdsk(System.Boolean FixErrors, System.Boolean VigorousIndexChe...
Reset                        Method        System.Management.ManagementBaseObject Reset()
SetPowerState                Method        System.Management.ManagementBaseObject SetPowerState(System.UInt16 PowerState, System.String Time)
Access                       Property      uint16 Access {get;set;}
Availability                 Property      uint16 Availability {get;set;}
BlockSize                    Property      uint64 BlockSize {get;set;}
Caption                      Property      string Caption {get;set;}
Compressed                   Property      bool Compressed {get;set;}
ConfigManagerErrorCode       Property      uint32 ConfigManagerErrorCode {get;set;}
ConfigManagerUserConfig      Property      bool ConfigManagerUserConfig {get;set;}
CreationClassName            Property      string CreationClassName {get;set;}
Description                  Property      string Description {get;set;}
DeviceID                     Property      string DeviceID {get;set;}
DriveType                    Property      uint32 DriveType {get;set;}
```

有一个属性是DriveType，我们看看这个属性是什么内容？

```powershell
PS C:\WINDOWS\system32> gwmi win32_logicaldisk -Property DriveType


__GENUS          : 2
__CLASS          : Win32_LogicalDisk
__SUPERCLASS     :
__DYNASTY        :
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
DriveType        : 3
PSComputerName   :
```

DriveType是3，这是什么意思呢？

这个时候我们可以借助搜索引擎，通过关键字doc wmi win32_logicaldisk我们比较容易定位到这个类的官方说明。

> DriveType
> 
> Data type: uint32
> 
> Access type: Read-only
> 
> Qualifiers: MappingStrings ("Win32API|FileFunctions|GetDriveType")
> 
> Numeric value that corresponds to the type of disk drive this logical disk represents.
> 
> Unknown (0)
> 
> No Root Directory (1)
> 
> Removable Disk (2)
> 
> Local Disk (3)
> 
> Network Drive (4)
> 
> Compact Disc (5)
> 
> RAM Disk (6)

从信息中我们看到，原来3代表的是Local Disk。