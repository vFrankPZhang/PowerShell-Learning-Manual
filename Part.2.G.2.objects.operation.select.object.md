# 对象的操纵

后面，我们会开始学习，如果通过属性，来操纵对象。

我们也先来看一下，我们能对对象做什么：

```powershell
PS C:\> gcm *-object*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Compare-Object                                     3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          ForEach-Object                                     3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Group-Object                                       3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Measure-Object                                     3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          New-Object                                         3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Register-ObjectEvent                               3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Select-Object                                      3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Sort-Object                                        3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Tee-Object                                         3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Where-Object                                       3.0.0.0    Microsoft.PowerShell.Core
```

# 选择对象——Select-Object

可以通过Select-Object，选择对象或者对象的属性。

###【例子】

1.根据需要的属性选择对象：

```powershell
Get-Process | Select-Object -Property ProcessName,Id,WS
```

2.列出前n个对象：

```powershell
PS C:\Windows\system32> Get-Process | select -First 5

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    681      27    14384      12640       5.16  17696   2 AcroRd32
    878      85   159760     139288     150.97  21244   2 AcroRd32
    207      17     8312       7996     304.83  16480   2 adintermaster
    164       8     2452       4280       2.42   5024   0 AdminService
    164      11     2012       4408      48.31  20716   2 ApMsgFwd

```

3.跳过前n个对象

```powershell
PS C:\Windows\system32> Get-Process | select -Skip 2

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    207      17     8312       8008     314.83  16480   2 adintermaster
    164       8     2452       4280       2.42   5024   0 AdminService
```

跳过首行会是比较多用到的。

## 提取属性的值

选择之后的对象，对象的属性是没有变的，但是有的时候，我们是需要对象的值本身。

### 【例子】

我现在想通过Stop-Process命令中的参数-ID来结束记事本的进程，我们知道，-ID的参数需要提供一个数值，类型为整形。

我们看一下哪怕我们选取出来ID属性，他是什么类型？

```powershell
PS C:\> Get-Process notepad | select id | gm


   TypeName:Selected.System.Diagnostics.Process

Name        MemberType   Definition
----        ----------   ----------
Equals      Method       bool Equals(System.Object obj)
GetHashCode Method       int GetHashCode()
GetType     Method       type GetType()
ToString    Method       string ToString()
Id          NoteProperty int Id=13688
```

我们发现类型还是process，所以我们需要把属性ID的值提取出来。

```powershell
PS C:\> Get-Process notepad | Select-Object -ExpandProperty id | gm

   TypeName:System.Int32

Name        MemberType Definition
----        ---------- ----------
CompareTo   Method     int CompareTo(System.Object value), int CompareTo(int value), int IComparable.Comp...
Equals      Method     bool Equals(System.Object obj), bool Equals(int obj), bool IEquatable[int].Equals(...
GetHashCode Method     int GetHashCode()
GetType     Method     type GetType()
GetTypeCode Method     System.TypeCode GetTypeCode(), System.TypeCode IConvertible.GetTypeCode()
ToBoolean   Method     bool IConvertible.ToBoolean(System.IFormatProvider provider)
ToByte      Method     byte IConvertible.ToByte(System.IFormatProvider provider)
ToChar      Method     char IConvertible.ToChar(System.IFormatProvider provider)
ToDateTime  Method     datetime IConvertible.ToDateTime(System.IFormatProvider provider)
ToDecimal   Method     decimal IConvertible.ToDecimal(System.IFormatProvider provider)
ToDouble    Method     double IConvertible.ToDouble(System.IFormatProvider provider)
ToInt16     Method     int16 IConvertible.ToInt16(System.IFormatProvider provider)
ToInt32     Method     int IConvertible.ToInt32(System.IFormatProvider provider)
ToInt64     Method     long IConvertible.ToInt64(System.IFormatProvider provider)
ToSByte     Method     sbyte IConvertible.ToSByte(System.IFormatProvider provider)
ToSingle    Method     float IConvertible.ToSingle(System.IFormatProvider provider)
ToString    Method     string ToString(), string ToString(string format), string ToString(System.IFormatP...
ToType      Method     System.Object IConvertible.ToType(type conversionType, System.IFormatProvider prov...
ToUInt16    Method     uint16 IConvertible.ToUInt16(System.IFormatProvider provider)
ToUInt32    Method     uint32 IConvertible.ToUInt32(System.IFormatProvider provider)
ToUInt64    Method     uint64 IConvertible.ToUInt64(System.IFormatProvider provider)
```
