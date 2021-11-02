# Introduction to Shell

## Get the path

```pwd```

## list files and directories

```ls```

## move to another directory

```cd
cd foldername
cd /../..
cd ~
cd /
```

## copy files

creates a copy of original.txt called duplicate.txt
```cp original.txt duplicate.txt```


## move a file

```mv autumn.csv winter.csv```

## Rename files

```mv course.txt old-course.txt```

## delete files

removes both thesis.txt and backup/thesis-2017-08.txt
```rm thesis.txt backup/thesis-2017-08.txt```

## delete directories

```rmdir foldername```
it only works when the directory is empty


## view a file's contents

```cat agarwal.txt```


## view a file's contents piece by piece

```less seasonal/spring.csv```
Press spacebar to page down, :n to go to the second file, and :q to quit


## to look at the start of a file

```head seasonal/summer.csv```

## view n rows

use flag n
``` head -n 3 filename ```

## list everything below a directory
``` ls -R -F ```
-R = recursive

## get help for a command

use the man command (short for "manual")

```man head```


## select columns from a file

``` cut -f 2-5,8 -d , values.csv ```
which means "select columns 2 through 5 and columns 8, using comma as the separator". 
cut uses -f (meaning "fields") to specify columns and -d (meaning "delimiter") to specify the separator

## select lines containing specific values

grep selects lines according to what they contain

```grep bicuspid seasonal/winter.csv```
prints lines from winter.csv that contain "bicuspid"

### grep's more common flags

* -c: print a count of matching lines rather than the lines themselves
* -h: do not print the names of files when searching multiple files
* -i: ignore case (e.g., treat "Regression" and "regression" as matches)
* -l: print the names of files that contain matches, not the matches
* -n: print line numbers for matching lines
* -v: invert the match, i.e., only show lines that don't match


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



## Use | to combine result to another command

```cut -d , -f 2 seasonal/summer.csv | grep -v Tooth```

## Write to file

```cut -d , -f 2 seasonal/summer.csv > file name```


### Combine many commands

```cut -d , -f 1 seasonal/spring.csv | grep -v Date | head -n 10```

1. select the first column from the spring data;
2. remove the header line containing the word "Date"; and
3. select the first 10 lines of actual data.

```cut -d , -f 2 seasonal/summer.csv | grep -v Tooth | head -n 1```

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


## remove duplicate lines
uniq it removes adjacent duplicated lines
``` head filename | sort | uniq ```


> get the second column from seasonal/winter.csv,
remove the word "Tooth" from the output so that only tooth names are displayed,
sort the output so that all occurrences of a particular tooth name are adjacent; and
display each tooth name once along with a count of how often it occurs.
The start of your pipeline is the same as the previous exercise:

Extend it with a sort command, and use uniq -c to display unique lines with a count of how often each occurs rather than using uniq and wc.

```cut -d , -f 2 seasonal/winter.csv | grep -v Tooth | sort | uniq -c```

## save the output of a pipe

> result.txt head -n 3 seasonal/winter.csv

## stop a running program

> ctrl + c

with appropriate parameters to list the number of lines in all of the seasonal data files. 

(Use a wildcard for the filenames instead of typing them all in by hand.)

```wc -l seasonal/*.csv```

### pipe to remove the line containing the word "total"

```wc -l seasonal/*.csv | grep -v total```

```wc -l seasonal/*.csv | grep -v total | sort -n | head -n 1```


## Batch processing

### How does the shell store information?

### Environment variables

> the shell stores information in variables. Some of these, called environment variables, are available all the time. Environment variables' names are conventionally written in upper case, and a few of the more commonly-used ones are shown below.

HOME	User's home directory	/home/repl
PWD	    Present working directory	Same as pwd command
SHELL	Which shell program is being used	/bin/bash
USER	User's ID	repl

### To get a complete list (which is quite long), you can type set in the shell

> Use set and grep with a pipe to display the value of HISTFILESIZE, which determines how many old commands are stored in your command history

```set | grep HISTFILESIZE```

### print a variable's value

> To get the variable's value, you must put a dollar sign $ in front of it
```echo $USER```

```echo USER```
> it will print the variable's name

> OSTYPE holds the value of os using. to print its value
```echo $OSTYPE```

### Shell variable

> shell variable, which is like a local variable in a programming language.

#### To create a shell variable, you simply assign a value to a name:

```training=seasonal/summer.csv```
without any spaces before or after the = sign.

```
testing=seasonal/winter.csv
head -n 1 $testing
```

### Repeat a command

```
for filetype in gif jpg png; do echo $filetype; done
```

> The structure is for …variable… in …list… ; do …body… ; done

### repeat a command once for each file

```
for filename in seasonal/*.csv; do echo $filename; done

for name in people/*; do echo $name; done
```

### record the names of a set of files

```
datasets=seasonal/*.csv
for filename in $datasets; do echo $filename; done
```

### many commands in a single loop

> use | in body of for loop

This loop prints the second line of each data file

```
for file in seasonal/*.csv; do head -n 2 $file | tail -n 1; done
```

Write a loop that prints the last entry from July 2017 (2017-07) in every seasonal file
```
for file in seasonal/*.csv; do grep 2017-07 $file | tail -n 1; done
```

### Many things in a loop

use ; in the body(do) of loop
```
for f in seasonal/*.csv; do echo $f; head -n 2 $f | tail -n 1; done
```
> it will first print filename
read the second line by running two commands together(head - read first two line. then tail read last line from head output)

### edit a file

```
nano filename
```

* Ctrl + K: delete a line.
* Ctrl + U: un-delete a line.
* Ctrl + O: save the file ('O' stands for 'output'). You will also need to press Enter to confirm the filename!
* Ctrl + X: exit the editor.

### record what I just did

1. Run history.
2. Pipe its output to tail -n 10 (or however many recent steps you want to save).
3. Redirect that to a file called something like figure-5.history

```
history | tail -n 10 > figure-5.history
```

Copy the files seasonal/spring.csv and seasonal/summer.csv to your home directory.
```
cp seasonal/spring.csv seasonal/summer.csv .
```

Use grep with the -h flag (to stop it from printing filenames) and -v Tooth (to select lines that don't match the header line) to select the data records from spring.csv and summer.csv in that order and redirect the output to temp.csv
```
grep -h -v Tooth spring.csv summer.csv > temp.csv
```

### save commands to re-run later

put the commands in .sh file
```nano filename.sh```
add commands in filename.sh and save (ctrl+o)
and run by
```bash filename.sh```

### Pass filenames to scripts

passing arguments to bash scripts
To support this, you can use the special expression $@

For example, if unique-lines.sh contains sort $@ | uniq

when you run 
```bash unique-lines.sh seasonal/summer.csv```
the shell replaces $@ with seasonal/summer.csv and processes one file


As well as $@, the shell lets you use $1, $2

```
column.sh
cut -d , -f $2 $1
```

```bash column.sh seasonal/autumn.csv 1```

wc -l | grep -v total | sort -n | head -n 1

wc -l == number of lines in each files
wc -c == number of character


### loops in a shell script

loops bash files
You can write loops using semi-colons, or split them across lines without semi-colons to make them more readable

```
# Print the first and last data records of each file.
for filename in $@
do
    head -n 2 $filename | tail -n 1
    tail -n 1 $filename
done
```

date-range.sh content
```
for filename in $@
do
  cut -d , -f 1 $filename | grep -v Date | sort | head -n 1
  cut -d , -f 1 $filename | grep -v Date | sort | tail -n 1
done
```

bash date-range.sh