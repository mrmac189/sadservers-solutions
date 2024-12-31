https://sadservers.com/scenario/st-paul

Join (merge) all the 338 files in /home/admin/polldayregistrations_enregistjourduscrutin?????.csv into one single /home/admin/all.csv file with the contents of all the CSV files in any order. There should be only one line with the names of the columns as a header.

cat polldayregistrations_* | sort | uniq > all.csv