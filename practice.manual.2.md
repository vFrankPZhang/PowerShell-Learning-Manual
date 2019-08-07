
# 获取WMI对象

WMI Windows Management Instrumentation

## 列出WMI的类

```bash
Get-WmiObject -List
```

Get-WmiObject默认使用root/cimv2 namespace，如果想用其他的namespace，使用参数`-Namespace`

## 显示WMI类的信息

```bash
Get-WmiObject -Class Win32_OperatingSystem
```

# Where-Object


