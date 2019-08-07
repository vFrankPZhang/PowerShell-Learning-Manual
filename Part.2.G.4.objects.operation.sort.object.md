
# 对象的操作——Sort-Object

获取到对象后，往往我们需要对内容进行排序，这时候我们就要用到Sort-Object这个命令。

## 基本排序

```bash
Get-ChildItem | Sort-Object -Property LastWriteTime, Name | Format-Table -Property LastWriteTime, Name
```

## 使用哈希表排序

```bash
Get-ChildItem | Sort-Object -Property @{ Expression = 'LastWriteTime'; Descending = $true }, @{ Expression = 'Name'; Ascending = $true } | Format-Table -Property LastWriteTime, Name
```

```bash
$order = @(
  @{ Expression = 'LastWriteTime'; Descending = $true }
  @{ Expression = 'Name'; Ascending = $true }
)

Get-ChildItem |
  Sort-Object $order |
  Format-Table LastWriteTime, Name
```


```python

```
