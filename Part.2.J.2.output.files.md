
# 输出

## 导出为CSV文件

```powershell
Get-Process | Export-CSV process.csv
```

## 导出为XML文件

```powershell
Get-Process | Export-CliXML process.html
```

## 导出为一个文件

```powershell
Get-Process | Out-File process.txt
```

## Out-Default

`Out-Default`是一个特殊的`Out-`命令，如果你不特别指定一个`Out-`命令，就会默认使用Out-Default。

### 例子

比如你在执行`Dir`的时候，实际上等于`Dir | Out-Default`

实际上`Out-Default`没做什么，是直接将内容指向给了`Out-Host`，输出到屏幕上。

## 转化为HTML

如果你想生成一个HTML报告，可以将获取的内容转化为HTML。

```powershell
Get-Service | ConvertTo-HTML
```

### 思考

同样是生成HTML，`Export-CliXML`和`ConvertTo-HTML`有什么区别呢？

Export是输出了一个文件，而Convert只是转换了格式，并没有输出文件，如果要想通过Convert得到文件，还需要`Out-File`。

其他ConvertTo命令：

```powershell
PS C:\> gcm convertto-*

CommandType     Name                                               Version    Sourc
                                                                              e
-----------     ----                                               -------    -----
Cmdlet          ConvertTo-Csv                                      3.1.0.0    Mi...
Cmdlet          ConvertTo-Html                                     3.1.0.0    Mi...
Cmdlet          ConvertTo-Json                                     3.1.0.0    Mi...
Cmdlet          ConvertTo-ProcessMitigationPolicy                  1.0.11     Pr...
Cmdlet          ConvertTo-SecureString                             3.0.0.0    Mi...
Cmdlet          ConvertTo-SPOMigrationEncryptedPackage             16.0.86... Mi...
Cmdlet          ConvertTo-SPOMigrationTargetedPackage              16.0.86... Mi...
Cmdlet          ConvertTo-TpmOwnerAuth                             2.0.0.0    Tr...
Cmdlet          ConvertTo-Xml                                      3.1.0.0    Mi...
```

### 练习

1.通过ConvertTo-Csv导出Get-Service的结果为Csv文件。
