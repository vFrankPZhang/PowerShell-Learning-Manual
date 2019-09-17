# PowerShell脚本语言

## 逻辑

脚本中的逻辑用于做决定然后根据决定执行相应的操作。

### If

If是PowerShell中主要的用于做条件判断决策的结构，类似于下面这样：

```powershell
If ($this -eq $that) {
    # commands
} elseif ($those -ne them) {
    # commands
} elseif ($we -gt $they) {
    # commands
} else {
    # commands
}
```

首先，If这个词是必须要有的，然后后面会跟一个括号()，对括号里面的内容进行判断，如果判断的结果为$true，就会执行后面的花括号的内容{}，如果不是$true，就会跳过后面的花括号，接着进行判断依次类推，最后一个判断用else连接，前面的判断用elseif。

注意，只要括号里面的结果是$true，就会执行后面的内容，不一定非要是一个判断的表达式在里面，比如：

```powershell
if ($true) {
    Write-Host 'hello'
}

PS C:\Users\vFrank\Desktop> .\test.ps1
hello
```

上面用的`# commands`是不会被执行的，`#`是注释符，其后面的内容不会被PowerShell执行，直至一个换行符。

花括号放到哪里也不重要，也是可以执行的，比如：

```powershell
if ($true) {Write-Host 'hello'}
PS C:\Users\vFrank\Desktop> C:\Users\vFrank\Desktop\test.ps1
hello
```

但是，要保持必要的书写习惯，以方便阅读，比如花括号里面的每一行进行缩进，使用`Tab`键，默认缩进4个字符。

### Switch

用于把一个值和一堆值进行比较，然后执行响应的操作，很像一个If跟着一堆Elseif。你也可以就使用If，但是应该知道这个语法结构。

```powershell
Switch ($status) {
    0 { $status_test = 'ok' }
    1 { $status_test = 'error' }
    2 { $status_test = 'jammed' }
    3 { $status_test = 'overheated' }
    4 { $status_test = 'empty' }
    default { $status_test = 'unknown' }
}
```
