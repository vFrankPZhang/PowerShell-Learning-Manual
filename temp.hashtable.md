
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