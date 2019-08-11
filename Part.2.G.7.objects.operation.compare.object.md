# 对象的操作——Compare-Object

```powershell
PS D:\ios> Compare-Object -ReferenceObject 1 -DifferenceObject 1 #相同

PS D:\ios> Compare-Object -ReferenceObject 1 -DifferenceObject 2 #不相同

InputObject SideIndicator
----------- -------------
          2 =>
          1 <=



PS D:\ios> Compare-Object -ReferenceObject 1 -DifferenceObject 1,2 #有相同的部分

InputObject SideIndicator
----------- -------------
          2 =>

PS C:\> Compare-Object -ReferenceObject 1,2 -DifferenceObject 1    #有相同的部分

InputObject SideIndicator
----------- -------------
          2 <=
```

箭头指哪边，就是哪边多那个内容。
