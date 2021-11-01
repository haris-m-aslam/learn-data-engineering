# Introduction to Shell

## Get file content

```cat agarwal.txt```

## First content from file

```head agarwal.txt```

## Last content from file

```tail agarwal.txt```

## Read first 3 lines from file

```head n -3 path/filename```

## Read last 2 lines from file

```tail n -2 path/filename```

## Read first 3 lines from file write to a file

```head n -3 path/filename > newfile```


```cut -d : -f 2-4```
>-d delimiter
>-f columns

```cut -d , -f 2```
>content is comma seperated, second column

> grep bicuspid seasonal/winter.csv

* -c: print a count of matching lines rather than the lines themselves
* -h: do not print the names of files when searching multiple files
* -i: ignore case (e.g., treat "Regression" and "regression" as matches)
* -l: print the names of files that contain matches, not the matches
* -n: print line numbers for matching lines
* -v: invert the match, i.e., only show lines that don't match


## Use | to combine result to another command

```cut -d , -f 2 seasonal/summer.csv | grep -v Tooth```

## Write to file

```cut -d , -f 2 seasonal/summer.csv > file name```


Combine many commands

cut -d , -f 1 seasonal/spring.csv | grep -v Date | head -n 10

1. select the first column from the spring data;
2. remove the header line containing the word "Date"; and
3. select the first 10 lines of actual data.

cut -d , -f 2 seasonal/summer.csv | grep -v Tooth | head -n 1

1. select the second column from the spring data;
2. remove the header line containing the word "Tooth"; and
3. select first line


> wc (short for "word count") prints the number of characters, words, and lines in a file. You can make it print only one of these using -c, -w, or -l respectively.

### words count containing July 2017

```grep 2017-07 seasonal/spring.csv | wc -l```

1. first find 2017-07 using grep
2. second wc -l for line

### specify many files at once

``` cut -d , -f 1 seasonal/winter.csv seasonal/spring.csv seasonal/summer.csv seasonal/autumn.csv ```

``` cut -d , -f 1 seasonal/* ```

``` cut -d , -f 1 seasonal/*.csv ```

head to get the first three lines from both seasonal/spring.csv and seasonal/summer.csv, a total of six lines of data, but not from the autumn or winter data files. Use a wildcard instead of spelling out the files' names in full.

```head -n 3 seasonal/s*.csv```

* ? matches a single character, so 201?.txt will match 2017.txt or 2018.txt
* [...] matches any one of the characters inside the square brackets, so 201[78].txt matches 2017.txt or 2018.txt, but not 2016.txt.
* {...} matches any of the comma-separated patterns inside the curly brackets, so {*.txt, *.csv} matches any file whose name ends with .txt or .csv, but not files whose names end with .pdf

sort lines of text
sort 
-n and -r can be used to sort numerically and reverse the order of its output, while -b tells it to ignore leading blanks and -f tells it to fold case (i.e., be case-insensitive)


```cut -d , -f 2 seasonal/winter.csv | grep -v Tooth | sort -r```


remove duplicate lines
uniq it removes adjacent duplicated lines


get the second column from seasonal/winter.csv,
remove the word "Tooth" from the output so that only tooth names are displayed,
sort the output so that all occurrences of a particular tooth name are adjacent; and
display each tooth name once along with a count of how often it occurs.
The start of your pipeline is the same as the previous exercise:

Extend it with a sort command, and use uniq -c to display unique lines with a count of how often each occurs rather than using uniq and wc.

```cut -d , -f 2 seasonal/winter.csv | grep -v Tooth | sort | uniq -c```

save the output of a pipe
> result.txt head -n 3 seasonal/winter.csv

stop a running program
> ctrl + c

with appropriate parameters to list the number of lines in all of the seasonal data files. (Use a wildcard for the filenames instead of typing them all in by hand.)

> wc -l seasonal/*.csv

pipe to remove the line containing the word "total"

> wc -l seasonal/*.csv | grep -v total

> wc -l seasonal/*.csv | grep -v total | sort -n | head -n 1