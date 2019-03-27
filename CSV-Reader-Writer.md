# CSV Reader / Writer

CSV (comma-separated values) is a standard for exporting and importing data across multiple formats, such as MySQL and
excel.

It stores numbers and text in plain text. Each row of the file is a data record and every record consists of one or more fields which
values are separated by commas. The use of the comma to separate every record’s fields is the source of the name given to this
standard.

Even with this very explicit name, there is no official standard CSVs, and it may denote some very similar delimiter-separated
values, which use a variety of field delimiters (such as spaces and tabs, which are both very popular), and are given the .csv
extension anyway. Such lack of strict terminology makes data exchange very difficult some times.

RFC 4180 provides some rules to this format:

• It’s plain text

• Consists of records

• Every record consists of fields separated by a single character delimiter

• Every record has the same sequence of fields

But unless there is additional information about the provided file (such as if the rules provided by RFC were followed), data
exchange through this format can be pretty annoying.

## The Basics

Python has native support for CSV readers, and it’s configurable (which, as we’ve seen, is necessary). There is a module csv
which holds everything you need to make a CSV reader/writer, and it follows RFC standards (unless your configuration overrides
them), so by default it should read and write valid CSV files. So, let’s see how it works:

csv-reader.py
```python
import csv
with open(’my.csv’, ’r+’, newline=’’) as csv_file:
    reader = csv.reader(csv_file)
    for row in reader:
        print(str(row))
```

Here, we are importing csv and opening a file called my.csv, then we call csv.reader passing our file as a parameter and then we
print each row in our reader.

If my.csv looks like this:

my.csv
```python
my first column,my second column,my third column
my first column 2,my second column 2,my third column 2
```

Then, when you run this script, you will see the following output:

>[’my first column’, ’my second column’, ’my third column’]
>[’my first column 2’, ’my second column 2’, ’my third column 2’]

And writing is just as simple as reading:

csv-reader.py
```python
import csv
rows = [[’1’, ’2’, ’3’], [’4’, ’5’, ’6’]]
with open(’my.csv’, ’w+’, newline=’’) as csv_file:
    writer = csv.writer(csv_file)
    for row in rows:
        writer.writerow(row)
        
with open(’my.csv’, ’r+’, newline=’’) as csv_file:
    reader = csv.reader(csv_file)
    for row in reader:
        print(str(row))
```
Then, in your csv file you’ll see:

my.csv
```python
1,2,3
4,5,6
```

And in your output:

>[’1’, ’2’, ’3’]
>[’4’, ’5’, ’6’]

It’s pretty easy to see what is going on in here. We are opening a file in write mode, getting our writer from csv giving our file to
it, and writing each row with it. Making it a little smarter:

csv-reader.py
```python
import csv

def read(file_location):
    with open(file_location, ’r+’, newline=’’) as csv_file:
        reader = csv.reader(csv_file)
        return [row for row in reader]

def write(file_location, rows):
    with open(file_location, ’w+’, newline=’’) as csv_file:
        writer = csv.writer(csv_file)
        for row in rows:
            writer.writerow(row)

def raw_test():
    columns = int(input("How many columns do you want to write? "))
    input_rows = []
    keep_going = True
    while keep_going:

        input_rows.append([input("column {}: ".format(i + 1)) for i in range(0, columns)])
        ui_keep_going = input("continue? (y/N): ")
        if ui_keep_going != "y":
            keep_going = False

    print(str(input_rows))

    write(’raw.csv’, input_rows)
    written_value = read(’raw.csv’)
    print(str(written_value))

raw_test()
```
We ask the user how many columns does he want to write for each row and then ask him for a row as long as he wants to continue,
then we print our raw input and write it to a file called raw.csv, then we read it again and print the data. When we run our script,
the output will look like this:


