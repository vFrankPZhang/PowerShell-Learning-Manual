# 扩展——PSModule

PowerShell支持的另一种扩展命令的方式就是module。

module不需要像snapin一样注册，放到特定目录下，PowerShell会自动查找。

## PSModulePath

这个特定的目录是一个定义好的环境变量，我们来看一下：

```powershell
Get-Content Env:\PSModulePath
```

## PowerShellGet

微软引入了一个名称为PowershellGet的模块，可以从在线仓库中搜索、下载、安装、升级模块。很想Linux中的RPM/YUM/APT-GET等。

微软还维护了一个在线源，PowerShell Gallery。

我们可以查看我们环境中添加的这个源。

```powershell
PS C:\Windows\system32> Get-PSRepository

Name                      InstallationPolicy   SourceLocation
----                      ------------------   --------------
PSGallery                 Untrusted            https://www.powershellgallery.com/api/v2
```

## 安装module

Find-Module
Install-Module
Update-Module
