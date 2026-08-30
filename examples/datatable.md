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

## Example: from a CSV attachment

A `src` pointing at a CSV attachment renders it as a datatable. This page has
`otters.csv` attached (delimiter `;`, with a header row, both defaults).

```
{{datatable
|src=otters.csv
|caption=Otters
}}
```

{{datatable
|src=otters.csv
|caption=Otters
}}

## Example: selecting columns with `column0`

`column0` picks which columns to show using 0-based indices, so `0,2` keeps the
first and third columns (*Species* and *Habitat*). It is the same as `columns`,
which counts from 1 instead. An absolute `src` reads the CSV from another page;
here it points back at this page's own attachment.

```
{{datatable
|src=/Examples/DataTable/otters.csv
|column0=0,2
|caption=Species and habitat
}}
```

{{datatable
|src=/Examples/DataTable/otters.csv
|column0=0,2
|caption=Species and habitat
}}
