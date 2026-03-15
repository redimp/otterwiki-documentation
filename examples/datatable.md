# DataTable

[DataTable Documentation](/-/help/plugins#datatable)

## Example: Animals

A datatable with default settings.

{{datatable
| Animal  | Habitat            | Diet                | Lifespan  | Fun Fact                                        |
| ------- | ------------------ | ------------------- | --------- | ----------------------------------------------- |
| Otter   | Rivers, lakes      | Fish, crustaceans   | 10-25 yrs | Known for sliding on their bellies!             |
| Eagle   | Mountains, forests | Small mammals, fish | 20-30 yrs | Sharpest eyesight of all birds.                 |
| Dolphin | Oceans             | Fish, squid         | 40-50 yrs | Sleep with one eye open (unihemispheric sleep). |
| Sloth   | Rainforests        | Leaves, fruits      | 12-20 yrs | Move slower than a growing grass!               |
| Penguin | Antarctica         | Fish, krill         | 20-30 yrs | Can’t fly but are excellent swimmers.           |
}}

## Example: Months

A datatable with caption, not searchable, with fixed height.

```
{{datatable
|searchable=false
|caption=Months
|perpage=5
|fixedheight=true

| No | Month     | Length |
| --:| --------- | ------:|
|  1 | January   |     31 |
|  2 | Feburary  |     28 |
|  3 | March     |     31 |
|  4 | April     |     30 |
|  5 | May       |     31 |
|  6 | June      |     30 |
|  7 | July      |     31 |
|  8 | August    |     31 |
|  9 | September |     30 |
| 10 | October   |     31 |
| 11 | November  |     30 |
| 12 | December  |     31 |
}}
```

{{datatable
|searchable=false
|caption=Months
|perpage=5
|fixedheight=true

| No | Month     | Length |
| --:| --------- | ------:|
|  1 | January   |     31 |
|  2 | Feburary  |     28 |
|  3 | March     |     31 |
|  4 | April     |     30 |
|  5 | May       |     31 |
|  6 | June      |     30 |
|  7 | July      |     31 |
|  8 | August    |     31 |
|  9 | September |     30 |
| 10 | October   |     31 |
| 11 | November  |     30 |
| 12 | December  |     31 |
}}
