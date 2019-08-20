
# 对象的操作——Sort-Object

获取到对象后，往往我们需要对内容进行排序，这时候我们就要用到Sort-Object这个命令。

### 【例子】

1.默认排序，根据名字排序

```powershell
PS C:\> Get-ChildItem | Sort-Object

    目录: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2018/5/23      9:57         130382 Defender_c46be3dc-30a9-452f-a5fd-4bf9ca87a854_en-US_HelpContent.cab
-a----        2018/5/23      9:57            391 Defender_c46be3dc-30a9-452f-a5fd-4bf9ca87a854_HelpInfo.xml
-a----         2019/8/9     16:25          12670 DHPLtUpdate.log
-a----         2019/5/9      7:05              0 Eventlog.txt
d-----         2019/3/4     20:41                hffntmp
d-----        2018/4/24     16:35                JESIS-ID_Utility
d-----        2018/6/14     18:51                Labs
-a----        2018/11/4     14:24              0 MiFlashvcom.ini
d-----        2018/4/14     20:10                MSOTraceLite
-a----        2018/5/23     10:10         181791 NetEventPacketCapture_7e984f2f-35da-48a2-a3c1-40ce59930a7c_en-US_HelpContent.cab
-a----        2018/5/23     10:10            391 NetEventPacketCapture_7e984f2f-35da-48a2-a3c1-40ce59930a7c_HelpInfo.xml
d-----         2018/4/8     17:49                NVIDIA
-a----         2018/9/6     11:22          53715 OnKeyDetector.log
d-----        2018/4/12      7:38                PerfLogs
-a----        2018/5/23     10:20           2121 ProcessMitigations_1e8f5a4c-f11b-41a5-bdbd-4695ca4a503e_en-US_HelpContent.cab
-a----        2018/5/23     10:20            392 ProcessMitigations_1e8f5a4c-f11b-41a5-bdbd-4695ca4a503e_HelpInfo.xml
d-r---        2019/6/19     18:59                Program Files
d-r---        2019/7/28     14:35                Program Files (x86)
-a----        2018/9/18     17:36             20 scope.ps1
d-----         2018/8/6     12:23                sharetest
d-----         2018/8/6     15:42                SnapPlugin
-a----        2018/5/23     10:25          10805 StartLayout_c361139b-c043-44a9-b94f-513fbaf1af0d_en-US_HelpContent.cab
-a----        2018/5/23     10:25            391 StartLayout_c361139b-c043-44a9-b94f-513fbaf1af0d_HelpInfo.xml
-a----        2018/5/23     10:26          11456 TLS_1e28c697-2370-42f2-ace1-5ac8777f8053_en-US_HelpContent.cab
-a----        2018/5/23     10:26            391 TLS_1e28c697-2370-42f2-ace1-5ac8777f8053_HelpInfo.xml
d-r---        2018/6/19     17:55                Users
d-----         2019/8/9     18:26                Windows
```

2.根据某个属性排序，如大小，最后修改时间等

```powershell
PS C:\> Get-ChildItem | Sort-Object -Property Length

    目录: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         2019/5/9      7:05              0 Eventlog.txt
-a----        2018/11/4     14:24              0 MiFlashvcom.ini
-a----        2018/9/18     17:36             20 scope.ps1
-a----        2018/5/23     10:26            391 TLS_1e28c697-2370-42f2-ace1-5ac8777f8053_HelpInfo.xml
-a----        2018/5/23     10:25            391 StartLayout_c361139b-c043-44a9-b94f-513fbaf1af0d_HelpInfo.xml
-a----        2018/5/23     10:10            391 NetEventPacketCapture_7e984f2f-35da-48a2-a3c1-40ce59930a7c_HelpInfo.xml
-a----        2018/5/23      9:57            391 Defender_c46be3dc-30a9-452f-a5fd-4bf9ca87a854_HelpInfo.xml
-a----        2018/5/23     10:20            392 ProcessMitigations_1e8f5a4c-f11b-41a5-bdbd-4695ca4a503e_HelpInfo.xml
-a----        2018/5/23     10:20           2121 ProcessMitigations_1e8f5a4c-f11b-41a5-bdbd-4695ca4a503e_en-US_HelpContent.cab
-a----        2018/5/23     10:25          10805 StartLayout_c361139b-c043-44a9-b94f-513fbaf1af0d_en-US_HelpContent.cab
-a----        2018/5/23     10:26          11456 TLS_1e28c697-2370-42f2-ace1-5ac8777f8053_en-US_HelpContent.cab
-a----         2019/8/9     16:25          12670 DHPLtUpdate.log
-a----         2018/9/6     11:22          53715 OnKeyDetector.log
-a----        2018/5/23      9:57         130382 Defender_c46be3dc-30a9-452f-a5fd-4bf9ca87a854_en-US_HelpContent.cab
-a----        2018/5/23     10:10         181791 NetEventPacketCapture_7e984f2f-35da-48a2-a3c1-40ce59930a7c_en-US_HelpContent.cab
d-----         2019/3/4     20:41                hffntmp
d-----        2018/6/14     18:51                Labs
d-----        2018/4/24     16:35                JESIS-ID_Utility
d-----         2018/8/6     12:23                sharetest
d-----         2018/8/6     15:42                SnapPlugin
d-r---        2019/6/19     18:59                Program Files
d-r---        2019/7/28     14:35                Program Files (x86)
d-r---        2018/6/19     17:55                Users
d-----         2018/4/8     17:49                NVIDIA
d-----        2018/4/14     20:10                MSOTraceLite
d-----         2019/8/9     18:26                Windows
d-----        2018/4/12      7:38                PerfLogs
```

3.按降序排序

```powershell
PS C:\> Get-ChildItem | Sort-Object -Property Length -Descending | Select-Object -First 5

    目录: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2018/5/23     10:10         181791 NetEventPacketCapture_7e984f2f-35da-48a2-a3c1-40ce59930a7c_en-US_HelpContent.cab
-a----        2018/5/23      9:57         130382 Defender_c46be3dc-30a9-452f-a5fd-4bf9ca87a854_en-US_HelpContent.cab
-a----         2018/9/6     11:22          53715 OnKeyDetector.log
-a----         2019/8/9     16:25          12670 DHPLtUpdate.log
-a----        2018/5/23     10:26          11456 TLS_1e28c697-2370-42f2-ace1-5ac8777f8053_en-US_HelpContent.cab
```
