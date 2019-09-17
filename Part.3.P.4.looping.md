# 循环

循环是用来执行一些反复的操作，直到满足某个条件为止。

## Do While

Do while是PowerShell主要的循环结构，用于直到一些条件为True，或者当条件为True时，重复执行一组命令。

基本用法是：
```powershell
Do {
    # commands
} While ($this -eq $that)
```

循环至少会执行一次，因为do后面才会进行判断。

你也可以把While放到前面，这样，满足条件才会执行。
```powershell
While (Test-Path $path) {
    # commands
}

