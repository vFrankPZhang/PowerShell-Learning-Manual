
* 点号(.)代表本地计算机 ```Get-CimInstance -ClassName Win32_Desktop -ComputerName .```



# 为离线计算机更新帮助

对于不能联网的计算机需要更新帮助信息，我们可以从已经更新了帮助信息的计算机中将帮助文件导出，然后再导入到需要更新帮助信息的离线计算机中。

**导出现有帮助信息：**

```bash
Save-Help
```

**导入离线帮助文件：**

```bash
Update-Help -Source
```
