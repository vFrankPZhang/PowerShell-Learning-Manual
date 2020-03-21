
# 文件系统

## Windows文件系统

Windows文件系统主要有三种类型的对象：驱动器，文件夹和文件。

他们的嵌套关系是：驱动器包含文件夹，文件夹再包含文件。

驱动器和文件夹都是容器。  
驱动器可以包含文件夹和文件。
文件夹可以包含文件夹和文件。

# PS Drive

在PowerShell中，Drive并不仅仅是指磁盘类的文件系统，PSDrive可以指向注册表集。

而对于文件夹或者文件，对于PowerShell而言，都统称为是一个项目“Item”。

因此，无论是注册表还是磁盘，就都成了PSDrive，像容器一样，容纳着注册表信息、文件夹和文件等等这些项目。

通过命令`Get-PSProvider`可以查看有哪些PS Drive

## 将本地文件夹映射成一个Windows驱动器

```cmd
subst p: $env:programfiles
```


```python

```
