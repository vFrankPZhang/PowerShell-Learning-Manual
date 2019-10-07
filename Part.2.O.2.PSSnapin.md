# 扩展——PSSnapin

PowerShell支持两种方式来扩展命令，其中一种叫PSSnapin。

PSSnapin通过注册和安装的方式，让PowerShell知道它，然后你就可以使用PSSnapin中的命令了。

## 查看已注册PSSnapin

可以通过下面的命令查看机器上注册的PSSnapin，已注册是指安装了但是未加载的。

```powershell
Get-PSSnapin -Registered
```

## 加载PSSnapin

可以通过下面的命令加载已注册的PSSnapin，然后就能使用相关的命令了。

```powershell
Add-PSSnapin sqlservercmdletsnapin100
```

## 查看PSSnapin中相关的命令

可以通过下面的命令查看PSSnapin中的命令：

```powershell
Get-Command -PSSnapin sqlservercmdletsnapin100
```

## PSProvider中的PSSnapin

有的PSSnapin会创建PSProvider。

首先我们执行`Get-PSProvider`来查看一下当前的PSProvider，我们发现没有SqlServer相关的Provider。

接着我们执行一下：

```powershell
Add-PSSnapin sqlserverprovidersnapin100
Get-PSProvider
```

我们可以看到一个名为SqlServer的Provider被添加到了PSProvider中。

这时候我们就可以通过类似`cd sqlserver:`这样的命令，进入SQLSERVER：驱动器，研究数据库之类的东西。
