# 比较运算符

| 比较运算符 | 意思 | 例子（返回True） |
| ---------- | ------ | ---------------- |
| -eq        | 等于   | 1 -eq 1          |
| -ne        | 不等于  | 1 -ne 2          |
| -lt        | 少于    | 1 -lt 2         |
| -le        | 少于等于| 1 -le 2          |
| -gt        | 大于    | 2 -gt 1          |
| -ge        | 大于等于| 2 -ge 1          |
| -like      | 相似（文字，支持通配符,不区分大小写）       | "file.doc" -like "f*.do?" |
| -notlike      | 不相似（文字，支持通配符）       | "file.doc" -notlike "p*.doc" |
| -contains      | 包含       | 1,2,3 -contains 1 |
| -notcontains      |  不包含      | 1,2,3 -notcontains 4 |

```powershell
“powershell” -eq "p"
“powershell” -eq "p*"
“powershell” -like "p"
“powershell” -like "p*"
“powershell” -match "p"
“powershell” -match "p*"
“powershell” -contains "p"
“powershell” -contains "p*"
```

eq 完全等于  
like 算上通配符等于  
match 前对后从前往后匹配，后是前的子集，返回true
上面的，多个对象匹配，返回true的对象

contains 全文检索，包含了就返回true，不返回比较的对象
