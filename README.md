# GreenWood2023-Cheesy-Arena-Data

## Backup Teams CSV Web API

```
https://raw.githubusercontent.com/FRC-Scouting-Data/GreenWood2023-Cheesy-Arena-Data/main/Backup-Teams.csv
```

## FTA Report CSV Web API

```
https://raw.githubusercontent.com/FRC-Scouting-Data/GreenWood2023-Cheesy-Arena-Data/main/FTA-Report.csv
```

## Playoff-Match-Schedule CSV Web API

```
https://raw.githubusercontent.com/FRC-Scouting-Data/GreenWood2023-Cheesy-Arena-Data/main/Playoff-Match-Schedule.csv
```

## Qualification Match Schedule CSV Web API

```
https://raw.githubusercontent.com/FRC-Scouting-Data/GreenWood2023-Cheesy-Arena-Data/main/Qualification-Match-Schedule.csv
```

## Standings CSV Web API

```
https://raw.githubusercontent.com/FRC-Scouting-Data/GreenWood2023-Cheesy-Arena-Data/main/Standings.csv
```

## Teams List CSV Web API

```
https://raw.githubusercontent.com/FRC-Scouting-Data/GreenWood2023-Cheesy-Arena-Data/main/Team-List.csv
```


<details>

  <summary>Qualification Match Query</summary>

  # Qualification Match Query

  ```
let
    Source = Csv.Document(Web.Contents("https://raw.githubusercontent.com/FRC-Scouting-Data/GreenWood2023-Cheesy-Arena-Data/main/Qualification-Match-Schedule.csv"),[Delimiter=",", Columns=15, Encoding=65001, QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"Match", type text}, {"Type", type text}, {"Time", type text}, {"Red1", Int64.Type}, {"Red1IsSurrogate", type logical}, {"Red2", Int64.Type}, {"Red2IsSurrogate", type logical}, {"Red3", Int64.Type}, {"Red3IsSurrogate", type logical}, {"Blue1", Int64.Type}, {"Blue1IsSurrogate", type logical}, {"Blue2", Int64.Type}, {"Blue2IsSurrogate", type logical}, {"Blue3", Int64.Type}, {"Blue3IsSurrogate", type logical}}),
    #"Removed Columns" = Table.RemoveColumns(#"Changed Type",{"Red1IsSurrogate", "Red2IsSurrogate", "Red3IsSurrogate", "Blue1IsSurrogate", "Blue2IsSurrogate", "Blue3IsSurrogate"}),
    #"Reordered Columns" = Table.ReorderColumns(#"Removed Columns",{"Type", "Match", "Time", "Red1", "Red2", "Red3", "Blue1", "Blue2", "Blue3"}),
    #"Duplicated Column" = Table.DuplicateColumn(#"Reordered Columns", "Time", "Time - Copy"),
    #"Reordered Columns1" = Table.ReorderColumns(#"Duplicated Column",{"Type", "Match", "Time", "Time - Copy", "Red1", "Red2", "Red3", "Blue1", "Blue2", "Blue3"}),
    #"Renamed Columns" = Table.RenameColumns(#"Reordered Columns1",{{"Time", "Date"}, {"Time - Copy", "Time"}}),
    #"Extracted First Characters" = Table.TransformColumns(#"Renamed Columns", {{"Date", each Text.Start(_, 10), type text}}),
    #"Parsed Date" = Table.TransformColumns(#"Extracted First Characters",{{"Date", each Date.From(DateTimeZone.From(_)), type date}}),
    #"Extracted Text Between Delimiters" = Table.TransformColumns(#"Parsed Date", {{"Time", each Text.BetweenDelimiters(_, " ", " "), type text}}),
    #"Parsed Time" = Table.TransformColumns(#"Extracted Text Between Delimiters",{{"Time", each Time.From(DateTimeZone.From(_)), type time}}),
    #"Merged Columns" = Table.CombineColumns(Table.TransformColumnTypes(#"Parsed Time", {{"Date", type text}, {"Time", type text}}, "en-US"),{"Date", "Time"},Combiner.CombineTextByDelimiter(" ", QuoteStyle.None),"Merged"),
    #"Changed Type2" = Table.TransformColumnTypes(#"Merged Columns",{{"Merged", type datetime}}),
    #"Renamed Columns1" = Table.RenameColumns(#"Changed Type2",{{"Merged", "Date/Time"}})
in
    #"Renamed Columns1"
```

</details>
