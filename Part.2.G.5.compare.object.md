
```bash
PS D:\ios> Compare-Object -ReferenceObject 1 -DifferenceObject 2

InputObject SideIndicator
----------- -------------
          2 =>
          1 <=


PS D:\ios> Compare-Object -ReferenceObject 1 -DifferenceObject 1
PS D:\ios> Compare-Object -ReferenceObject 1 -DifferenceObject 1,2

InputObject SideIndicator
----------- -------------
          2 =>
```
