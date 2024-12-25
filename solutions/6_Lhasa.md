https://sadservers.com/scenario/lhasa

There's a file /home/admin/scores.txt with two columns (imagine the first number is a counter and the second one is a test score for example).

Find the average (more precisely; the arithmetic mean: sum of numbers divided by how many numbers are there) of the numbers in the second column (find the average score).

Use exactly two digits to the right of the decimal point. i. e., use exaclty two "decimal digits" without any rounding. Eg: if average = 21.349 , the solution is 21.34. If average = 33.1 , the solution is 33.10.

Tip: There's bc, Python3, Golang and sqlite3 installed in this VM.

```bash
SUMM=0.0
for i in $(cut -d " " -f2 scores.txt)
do 
    SUMM=$(echo "scale=2; $SUMM + $i" | bc )
    echo $SUMM
done
COUNT=$(wc -l scores.txt | cut -d " " -f 1)
MEAN=$(echo "scale=2; $SUMM / $COUNT" | bc)
echo $MEAN > ~/solution 
```



