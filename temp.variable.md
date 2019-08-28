# 变量是什么？

变量是内存中的一个带有名字的盒子，盒子中可以放一个计算机名称、一系列服务的集合、XML文档等。

通过名字去访问这个盒子。

# 赋值

比如：

```powershell
$var = "Hello"
```

$后面加一个名字表示这是一个变量及其名字

用=号把后面的内容赋值给前面的这个变量（把后面的内容装入前面的盒子）

# 变量的规则

* 变量的名称通常包含字母、数字和下划线，最常见的形式是以字母或下划线开头。  
* 变量的名称可以包含空格，但是名字必须被花括号包住。如：${My Name}，尽量不要这么用。  
* 关闭shell时，变量被清除。
* 除非有VBScript背景，PowerShell用户通常不用前缀名来标识变量类型，如“strComputerName”。
* 对相同变量名赋值，变量会被新的内容替换


# 查看变量的内容

$美元符号+变量名

如：

```powershell
$var
Hello
```

# 使用变量

## 单引号

PowerShell把所有包在单引号中的东西认为是一个文本字符串。

## 双引号

双引号中的变量会代表变量的内容。

```powershell
PS C:\WINDOWS\system32> $a = 'a'
PS C:\WINDOWS\system32> $b = "$a"
PS C:\WINDOWS\system32> $b
a
```

这种解析仅发生在初次解析。

```powershell
PS C:\WINDOWS\system32> $a = 'b'
PS C:\WINDOWS\system32> $b
a
```

## 转义字符`

```powershell
PS C:\WINDOWS\system32> $a = 'a'
PS C:\WINDOWS\system32> $b = "`$a is $a"
PS C:\WINDOWS\system32> $b
$a is a
```

`去掉了$代表变量的意义

```powershell
PS C:\Users\Frank> $a = 'a'
PS C:\Users\Frank> $b = "`$a `nis `n$a"
PS C:\Users\Frank> $b
$a
is
a
```

`n 换行符

`t tab
`a 机器发出声响 alert

# 在一个变量中存储多个对象

```powershell
PS C:\Users\Frank> $var = 'a',1,1.2
PS C:\Users\Frank> $var
a
1
1.2
```

用，号分割多个对象

# 访问包含多个对象的变量中的某一个对象

```powershell
PS C:\Users\Frank> $var[0]
a
PS C:\Users\Frank> $var[1]
1
PS C:\Users\Frank> $var[-1]
1.2
PS C:\Users\Frank> $var[-2]
1
```

从前往后0，1，2...
从后往前-1，-2...

如果一个变量包含了多个对象，也不能对这个变量直接使用对象的方法了，需要指定具体是哪一个变量。

```powershell
PS C:\Windows\system32> $var.toupper()
Method invocation failed because [System.Int32] does not contain a method named 'toupper'.
At line:1 char:1
+ $var.toupper()
+ ~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [], RuntimeException
    + FullyQualifiedErrorId : MethodNotFound

PS C:\Windows\system32> $var[0].toupper()
A
```

我们指定了$var[0]后，就可以使用方法toupper()了。

方法只是产生了新的结果，并不会改变原来的变量中的值。

```powershell
PS C:\Windows\system32> $var[0].toupper()
A
PS C:\Windows\system32> $var
a
1
1.2
```

如果想更改变量值，需要指定具体的变量中的位置。

```powershell
PS C:\Windows\system32> $var[0]='hello'
PS C:\Windows\system32> $var
hello
1
1.2
```

$():子表达式，$()中的所有内容都会被当成普通的PowerShell命令。

```powershell
PS C:\Windows\system32> $firstname = $service[0].name
PS C:\Windows\system32> $firstname
AdobeARMservice
PS C:\Windows\system32> $firstname = "The first name is $($service[0].name)"
PS C:\Windows\system32> $firstname
The first name is AdobeARMservice
```

# 声明变量类型

```powershell
PS C:\Users\Frank> $number = Read-host "Enter a number"
Enter a number: 100
PS C:\Users\Frank> $number * 10
100100100100100100100100100100
```

我们发现，结果不是1000，因为PowerShell把我们输入的100作为了字符串处理，而不是当作数字处理。

如何解决这个问题？我们可以通过[]符号及类型，告诉Shell，$number的类型应该是一个整型。

```powershell
PS C:\Users\Frank> [int]$number = Read-host "Enter a number"
Enter a number: 100
PS C:\Users\Frank> $number * 100
10000
```

另一个好处是，当我们输入的数据类型不对时，会报错：

```powershell
PS C:\Users\Frank> [int]$number = Read-host "Enter a number"
Enter a number: hello
无法将值“hello”转换为类型“System.Int32”。错误:“输入字符串的格式不正确。”
所在位置 行:1 字符: 1
+ [int]$number = Read-host "Enter a number"
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ArgumentTransformationMetadataException
    + FullyQualifiedErrorId : RuntimeException
```
