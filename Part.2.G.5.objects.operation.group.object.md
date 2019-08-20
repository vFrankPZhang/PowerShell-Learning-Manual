# 对象的操作——Group-Object

Group-Object用于根据我们指定的属性来分组。

### 【例子】

1.服务中，状态为Stopped和Running分别是多少？

```powershell
PS C:\WINDOWS\system32> Get-Service | Group-Object -Property Status

Count Name                      Group
----- ----                      -----
  170 Stopped                   {AdobeARMservice, AJRouter, ALG, AppIDSvc...}
  112 Running                   {AlibabaProtect, Appinfo, AudioEndpointBuilder, Audiosrv...}
```

2.根据扩展名分组，统计数量

```powershell
PS C:\> Get-ChildItem | Group-Object -Property Extension

Count Name                      Group
----- ----                      -----
   12                           {hffntmp, JESIS-ID_Utility, Labs, MSOTraceLite...}
    5 .cab                      {Defender_c46be3dc-30a9-452f-a5fd-4bf9ca87a854_en-US_HelpContent.cab, NetEventPacketCapture_7e984f2f-35da-48a2-a3c...
    5 .xml                      {Defender_c46be3dc-30a9-452f-a5fd-4bf9ca87a854_HelpInfo.xml, NetEventPacketCapture_7e984f2f-35da-48a2-a3c1-40ce599...
    2 .log                      {DHPLtUpdate.log, OnKeyDetector.log}
    1 .txt                      {Eventlog.txt}
    1 .ini                      {MiFlashvcom.ini}
    1 .ps1                      {scope.ps1}
```

