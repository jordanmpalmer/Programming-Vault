https://www.youtube.com/watch?v=MeKL_n6SiYY&list=PLoyECfvEFOjYYy54Wa9E83xycKilVMoHp&index=4
https://www.youtube.com/watch?v=tGHLa1TnhLc&list=PLoyECfvEFOjYYy54Wa9E83xycKilVMoHp&index=4


| Input                                                     | Description                                        |
| --------------------------------------------------------- | -------------------------------------------------- |
| `?Worksheets.Count`                                       | Ask how many worksheets there are in this workbook |
| `?Range("B2").Value`                                      | Current value in B2                                |
| `Selection.HorizontalAlignment = xlCenterAcrossSelection` | Center text in current selection                   |
| `Selection.HorizontalAlignment = xlLeft`                  | Left text in current selection                     |
| ```Sub MacroName()             End Sub```                 | Create Sub-Procedure                               |
| ```'this would be a comment```                            | Comment                                            |
| Step into tool bar                                        | Allows to run code line by line                    |

> [!Visual Aids]-
> ![[Object-Browser.png]]
> ![[Step-Through.png]]

| Objects                 | What we can do         |
| ----------------------- | ---------------------- |
| `Rows(1:1)`             | Rows object            |
| `Range(A1)`             | Cell object            |
| `Cells(3,3)`            |                        |
| `[A1]`                  | Short hand cell object |
| `Selection`             | Current selected cell  |
| `ActiveSheet`           | Current active sheet   |
| `Sheets(1)`             | Select sheet by index  |
| `Sheets("Sheet1")`      | Select sheet by name   |
| `.CurrentRegion.Select` | Selects current region |
| `Range("B" & x)  `      |                        |


| Methods                  | Description                          |
| ------------------------ | ------------------------------------ |
| `.Insert`                | Insert rows or columns               |
| `.Select`                |                                      |
| `.Select`                | Select an Object                     |
| `.Offset(Rows, Columns)` |                                      |
| `.Clear`                 | Clear all in cell                    |
| `isEmpty(Cells(x,x))`    | Logical test to see if cell is empty |
| `.Count`                 |                                      |
| `.Paste`                 |                                      |
| `.Address`                         |                                      |

| Properties                                 | Description                   |
| ------------------------------------------ | ----------------------------- |
| `.Value`                                   |                               |
| `.Font.Bold = True`                        |                               |
| `.Interior.ThemeColor = xlThemeColorDark1` |                               |
| `.Interior.Color = Blue`                   |                               |
| `.Interior.Color = RGB(0,0,0)`             |                               |
| `.Name`                                    | Can be used for name of sheet |
|                                            |                               |

| Variables              | Description                    |
| ---------------------- | ------------------------------ |
| Dim                    | Declared in memory             |
| Dim aNum As Integer    | Declare an integer called aNum |
| Dim aDate As Date      | Declare a date called aDate    |
| Dim aNum As Decimal    |                                |
| Dim aNum As Double     |                                |
| Dim aString As Variant | Pretty but expensive           |
| Dim aString As String  |                                |
| Dim aString As Char    |                                |
| Dim aBool As Boolean   |                                |
| Dim anObject As Object |                                |
| Dim aRange As Range    |                                |
| Dim x As Worksheet     |                                |

| Syntax                                     | Description                         |
| ------------------------------------------ | ----------------------------------- |
| `&`                                        | Used to concatenate                 |
| `With      End With`                       | Allows to set multiple properties   |
| `For x = 1 To 10         Next x`           | For loop                            |
| `For Each x In Worksheets          Next x` | For each loop                       |
| `Exit For`                                 | Allows you to prematurely exit loop |
| MsgBox "Message"                           |                                     |
| `Do While x < 10          Loop`            | Do While loop                       |
| `Do While Cells(x, 1).Value <> ""`         | <> means is not                     |
| `Do Until x > 1           Loop`            | Do until loop                       |
| `Do            Loop Until x>10`            | Do loop until                       |
| `Call SubName`                             | Call a function to run              |
| `If elseif else endif`                     | If statment                         | 


![[Screenshot 2023-12-10 at 1.12.03 PM.png]]
Option Explicit forces Dim As
Ctrl + A in excel selects whole table