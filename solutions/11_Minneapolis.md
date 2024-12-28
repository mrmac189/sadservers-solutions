https://sadservers.com/newserver/minneapolis

Break the Comma Separated Valued (CSV) file data.csv in the /home/admin/ directory into exactly 10 smaller files of about the same size named data-00.csv, data-01.csv, ... , data-09.csv files in the same directory. All the files should have the same header (first line with column names) as data.csv. None of the smaller files should be bigger than 32KB.

```bash
#/bin/bash

# cut doesn't support field 0
for i in {1..10}
do
    head -1 data.csv > data-0$(($i-1)).csv
    cut -d "," -f $i data.csv >> data-0$(($i-1)).csv
    find "data-0$(($i-1)).csv" -size +32k -exec truncate --size=32KB {} \;
done
```

