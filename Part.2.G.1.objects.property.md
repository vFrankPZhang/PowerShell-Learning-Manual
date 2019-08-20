# 对象的属性

对象总共由3部分组成，对象的类型，对象的属性、对象的方法。

前面，我们讲过了对象的类型，现在我们来讲一下对象的属性。

以`Get-Process`为例

```powershell
PS C:\Windows\system32> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    681      27    14384      21304       5.08  17696   2 AcroRd32
    876      85   159792     156352     149.69  21244   2 AcroRd32
    207      17     8312      12072     188.83  16480   2 adintermaster
    164       8     2452       5984       2.38   5024   0 AdminService
    164      11     2012       7192      42.63  20716   2 ApMsgFwd
    168      11     1908       6992       0.17  19792   2 ApntEx
    309      16     3260      12136       4.84  15412   2 Apoint
    542      31    23716      32340       3.02  17208   2 ApplicationFrameHost
     68       5      772       3276       0.05  20160   2 ApRemote
    155       9     1616       3480       0.33   5036   0 armsvc
    714      23    65100      54180      59.56   1560   0 audiodg
    308      29     9208      25824       0.13  14960   2 backgroundTaskHost
   1008      62    66816      81400     406.81  16848   2 BaiduNetdisk
    429      31    20128      26000       4.83  19380   2 BaiduNetdiskHost
    189      14    11944       6040       0.05  20844   2 BaiduNetdiskHost
   3061     147   160836     211808     204.42   1528   2 BingDict
   1563      50    37920      54068     226.14   4564   0 CcmExec
--More--
```

我们可以看到，进程们有哪些信息？包括：Handls、NPM(K)、PM(K)、WS(K)、CPU(s)、Id、SI、ProcessName。

从我们最容易理解的ProcessName、Id来说，我们知道，这是进程叫什么，以及进程在系统中的Id号。

这些信息是用来描述Process这个对象的，反过来讲，我们也就可以通过这些描述找到这个进程，比如，名字叫什么，ID是什么的进程。这，就是进程这个对象的属性。

对于Process这个对象，除了上面8个属性，其实还有很多，这8个是PowerShell定义好的，对于我们平时最可能用到的。

如果想获取所有的属性，那就是我们之前用过的一个命令`Get-Member`。

```powershell
PS C:\> (Get-Process | Get-Member -MemberType Properties).count
67
```

我们看到，有67个属性之多。

## 如何使用某个对象的属性

属性是所属于某个对象的，用符号点“.”来表示所属的前后，比如process对象的name属性。

```powershell
(Get-Process).name
```

后面我们会用到这个方法，再来进一步理解。
