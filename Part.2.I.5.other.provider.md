
# 使用其他PSProvider

前面讲到的例子主要是以文件系统的操作为例，也是我们用到的最多的PSProvider。

不同的PSProvider有不同的功能，所以你需要用不同的方式来使用其他的provider，但是跟文件系统的provider使用方式是很像的。

**【例子】**

我们以注册表为例，来演示一下其他的provider的使用。

![](images\regedit.jpg)

```bash
Set-ItemProperty -Path dwn -PSPropert EnableAeroPeek -Value 0
```
