
# 对象的操作——Select-Object

可以通过Select-Object来选择你想显示的对象属性。

**【例子】**

```bash
Get-WmiObject -Class Win32_LogicalDisk | Select-Object -Property Name,FreeSpace

```
