
# 项目的操作

provider中所存储的内容都是一个一个的项目，项目都常是有属性的，比如一个文件的最近一次修改时间，是不是只读等等。

下面我们学习一下，我们能对这些项目做什么操作。

## 查看项目的属性

```
PS D:\ios> Get-ItemProperty .\ubuntu-19.04-desktop-amd64.iso

    Directory: D:\ios

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         8/1/2019  12:17 PM     2097152000 ubuntu-19.04-desktop-amd64.iso
```

有些对象是不包含属性的，比如环境变量。

```bash
PS D:\ios> Get-ItemProperty Env:\PSModulePath
Get-ItemProperty : Cannot use interface. The IPropertyCmdletProvider interface is not supported by this provider.
At line:1 char:1
+ Get-ItemProperty Env:\PSModulePath
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotImplemented: (:) [Get-ItemProperty], PSNotSupportedException
    + FullyQualifiedErrorId : NotSupported,Microsoft.PowerShell.Commands.GetItemPropertyCommand
```

## 新建一个项目

创建文件和文件夹

```bash
New-Item -Path 'C:\temp\New Folder' -ItemType Directory
New-Item -Path 'C:\temp\New Folder\file.txt' -ItemType File
```

## 获取子项目

显示一个文件夹的所有文件，包括隐藏的，但不含子目录。

```powershell
Get-ChildItem -Path C:\ -Force
```

如果需要包含子文件，需要加参数`-Recurse`

通过PowerShell进行复杂的筛选：

```bash
Get-ChildItem -Path $env:ProgramFiles -Recurse -Include *.exe | Where-Object -FilterScript {($_.LastWriteTime -gt '2005-10-01') -and ($_.Length -ge 1mb) -and ($_.Length -le 10mb)}
```

## 复制项目

```bash
Copy-Item -Path C:\boot.ini -Destination C:\boot.bak
```

对于已经存在的文件，进行覆盖，使用参数`-Force`,哪怕目标是只读。
```bash
Copy-Item -Path C:\boot.ini -Destination C:\boot.bak -Force
```

拷贝文件：

```bash
Copy-Item C:\temp\test1 -Recurse C:\temp\DeleteMe
```

可以对需要考的文件做过滤

```bash
Copy-Item -Filter *.txt -Path c:\data -Recurse -Destination C:\temp\text
```

**使用Scripting.FileSystem COM类进行备份**

```bash
(New-Object -ComObject Scripting.FileSystemObject).CopyFile('C:\boot.ini', 'C:\boot.bak')
```

## 删除项目

删除文件夹下的所有文件

```bash
Remove-Item -Path C:\temp\DeleteMe
```

当该文件下有内容时，会有提示，确认删除，如果使用参数`-Recurse`就可以直接删除了。


