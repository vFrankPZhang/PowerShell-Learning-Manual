# PowerShell脚本语言

## 逻辑

脚本中的逻辑用于做决定然后根据决定执行相应的操作。

## 判断

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

用于把一个值和一堆值进行比较，然后执行相应的操作，很像一个If跟着一堆Elseif。你也可以就使用If，但是应该知道这个语法结构。

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

```powershell
$result = ""
$servername = 'DCFILE01'
Switch -wildcard ($servername) {
    "*DC*" { $result += ' Domain Controller ' }
    "*FILE*" { $result += ' File Server ' }
    "*SQL*" { $result += ' SQL Server ' }
    "*EXCH*" { $result += ' Exchange Server ' }
}
Write-Host $result

PS C:\> d:\script\test.ps1


 Domain Controller  File Server
```

可以看到，switch将会执行所有符合条件的命令，不同的是if只会执行第一次符合条件的需要执行的命令。

```powershell
$result = ""
Switch -wildcard ($servername) {
"*DC*" { $result += ' Domain Controller '; break }
"*FILE*" { $result += ' File Server '; break }
"*SQL*" { $result += ' SQL Server '; break }
"*EXCH*" { $result += ' Exchange Server '; break }
}
```
通过Break，就会跟If的效果一样了，后面我们会说Break。

## 循环

循环是用来执行一些反复的操作，直到满足某个条件为止。

### Do While

Do while是PowerShell主要的循环结构，用于直到一些条件为True，或者当条件为True时，重复执行一组命令。

基本用法是：
```powershell
Do {
    # commands
} While ($this -eq $that)
```

循环至少会执行一次，因为do后面才会进行判断。

你也可以把While放到前面，这样，满足条件才会执行。
```powershell
While (Test-Path $path) {
    # commands
}
```

### ForEach

ForEach 跟 ForEach-Object是非常像的，不同之处在于他们的语法。

ForEach的作用是，获取数组中的对象，并且枚举这些对象，一次处理一个。

比如说，我打开3个记事本，然后通过ForEach一个一个的结束他们的进程：

```powershell
PS C:\Users\vFrank> $processes = Get-Process notepad
PS C:\Users\vFrank> $processes

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    447      26     9844      38264       0.53   1320   5 notepad
    434      25     9280      37360       0.34   2564   5 notepad
    431      26     9532      36992       0.38   3652   5 notepad


PS C:\Users\vFrank> foreach ($process in $processes) {$process.kill()}
```

我们通过这个例子，来看一下ForEach的语法是怎样的。

首先，花括号｛｝里面的内容，是我们要执行的操作，我们要对$process这个变量的对象，使用kill()的方法。

那$process里面是什么呢？

在ForEach后面的括号里面，$processes，注意，是复数，代表的是3个notepad进程，而$process in $processes是指，$process会依次去从$processes中取1个process对象赋给自己，然后执行花括号里面的内容，赋值一次，执行一次再去赋值，直到从$processes中取完。

### For

用于执行特定次数的循环。

```powershell
For ($i=0;$i -lt 5;$i++) {
    #do something
}
```

上面的例子是说，一开始i=0,如果i小于5，就会执行下面的操作，执行完，给i上1,直到不满足i小于5这个条件。

## 打断（Break）和继续（Continue）

Break和Continue是两个特殊的关键字，用于控制。

### Break

前面Switch

Break会立即终止任何的结构，除了If。如果If被嵌套在别的结构里面，Break会直接跳出整个结构。

比如：

```powershell
$i = 0
do {
if ($i –eq 5) { break }
$i++
} while ($i –lt 100)
```

加入没有if那行，i会一直加，知道i=100，但是因为if的那行，当x=5的时候，整个循环就跳出去了，不会继续加了。

如果你使用Break但是，没有什么结构可以退出了，则整个脚本会结束。

### Continue



