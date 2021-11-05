# Introduction to Bash Scripting


## From Command-Line to Bash Script

* grep : filters input based on regex pattern matching
* cat : concatenates files contents line by line
* tail \ head : give only the last -n(flag) lines
* wc : does a word or line count with flags -w -l
* sed : does patter-matched string replacement

> To test your regex you can usehelpful sites like regex101.com

cat two_cities.txt | egrep 'Sydney Carton|Charles Darnay' | wc -l


## To check bash location

```which bash```

## Bash script has a extension .sh

```#!/usr/bash ```
is the first line in bash file, it is not technically needed, but a convention

## Run command

```bash script_name.sh```

> if first line is #!/usr/bash then script can run by

./script_name.sh

each line of bash script can be a shell command


## Create a sed pipe to a new file

change the team Cherno to Cherno City first, and then Arda to Arda United
pipe the output to a file 
cat soccer_scores.csv | sed 's/Cherno/Cherno City/g' | sed 's/Arda/Arda United/g' > soccer_scores_edited.csv

sed command used to find and replace strings
```sed 's/Cherno/Cherno City/g'```

flag g means all occurance
1 means first occurance
2 means second occurance
3g means every third occurance


## Standard streams & arguments

STDIN : standard input - a stream of data into the program
STDOUT : standard output - a stream of data out of the program
STDERR : standard error - error in your program

1 - stdout 
2 - stdin


## STDIN vs ARGV

bash script can take arguments to be used inside by adding a space after the sript execution call

ARGV is the array of all arguments given to the program

Arguments can be accessed by $. first one by $1 second by $2

> $@ and $* give all the arguments in ARGV
> $# gives the number of arguments

## example

args.sh file content

```
echo $1
echo $2
echo $@
echo "There are $# arguments"
```

```bash args.sh one two three four five```
will print
one
two
one two three four five



## Basic variables in Bash

## Assign variable

var1="Moon"

not use space around =


* Single quotes ('sometext') = Shell interprets what is bw literally
* Double  quotes ("sometext") = shell interprets literally except $ and backticks (`)
* Backticks (`sometext`) = shell runs the command and captures the STDOUT back into variable



Refer that variable $ notation
echo $var1
There are 5 arguments

```
rightnow_dbquote="The date is `date`"
rightnow_dbparant="The date is $(date)"
```

> Both will print current date

> if it is in single quote, it will not interpret the program, it will print
```The date is date```


## Numeric variables in Bash

1 + 4 will show error

## expr is program to do arithmetic operations

expr 1 + 4 will print 5

expr cannot handle decimal points

expr 1+ 2.5 will show error

## Introducing bc

bc : basic calculator is commandline program


```echo "2 + 3.5" | bc```
> 5.5

## scale flag : for how many decimal points

echo "scale=3; 10/3" | bc

```
expr 5 + 7
echo $((5 + 7))
```
both are same

## Create three variables from the data in the three files within temps by concatenating the content into a variable using a shell-within-a-shell.

### Create three variables from the temp data files' contents

```
temp_a=$(cat temps/region_A)
temp_b=$(cat temps/region_B)
temp_c=$(cat temps/region_C)
```

Print out the three variables
```echo "The three temperatures were $temp_a, $temp_b, and $temp_c"```



## Arrays in Bash

Two typs of arrays in bash
numerical indexed array and associative 

## Numerical array

```
declare -a my_first_array

or

my_first_array=(1 2 3)
```

> Bash seperates array elements with space not commas

## important array properties

Return all array elements

```
my_aaray=(1 3 5 2)
echo ${my_array[@]}
```

### Length of an array
```echo ${#my_array[@]}```

### To access array elements use square brackets

```echo ${my_array[2]}```
returns third element

### set array value
```
my_array[0]=999
echo ${my_array[0]}
```

## Slice array

```array[@]:N:M```
N : start index
M : how many elements

```echo ${my_array[@]:3:2}```
return two elements from 4 th element(4th and 5th)

## Appending to array

```array+=(elements)```

my_array+=(10)


## Associative arrays

it is avilable only in bash 4

bash --version

## Associative array declare using -A

```declare -A city_details```

```city_details=([city_name]="New york" [population]=1440000) # add elements```

```echo ${city_details[city_name]} # index using key to return a value```

## Return all kerys in associative array !

```echo ${!city_details[@]} # return all keys```


## Add an element in associative arry

```my_array[key]=value```


### Create a normal array with the mentioned elements using the declare method

```declare -a capital_cities```

### Add (append) the elements

```
capital_cities+=("Sydney")
capital_cities+=("Albany")
capital_cities+=("Paris")
```


### The array has been created for you

```capital_cities=("Sydney" "Albany" "Paris")```

### Print out the entire array

```
echo ${capital_cities[@]}

Print out the array length
echo ${#capital_cities[@]}


Create empty associative array
declare -A model_metrics

Add the key-value pairs
model_metrics[model_accuracy]=98
model_metrics[model_name]="knn"
model_metrics[model_f1]=0.82


Declare associative array with key-value pairs on one line
declare -A model_metrics=([model_accuracy]=98 [model_name]="knn" [model_f1]=0.82)

# Print out the entire array
echo ${model_metrics[@]}


# Print out just the keys
echo ${!model_metrics[@]}
```


### Create variables from the temperature data files
```
temp_b="$(cat temps/region_B)"
temp_c="$(cat temps/region_C)"

Create an array with these variables as elements
region_temps=(temp_b temp_c)

Call an external program to get average temperature
average_temp=$(echo "scale=2; (${region_temps[0]} + ${region_temps[1]}) / 2" | bc)

Append average temp to the array
region_temps+=($average_temp)

Print out the whole array
echo ${region_temps[@]}
```

## IF statements

syntax

```
if [condition]; then
  # some code
else
  # some other code
fi
```

```
x="Queen"
if [ $x == "King' ]; then
  echo "$x is a king !"
else 
  echo "$x is not a king!"
fi
```

## Arithmetic if statements

```
x=10
if (($x > 5 )); then
  echo "$x is more than 5!"
fi
```

* -eq equal to
* -ne not equal to
* -lt less than
* -le less than or equal to
* -gt greater than
* -ge greater than or equal to

```
if [ $x -gt 5]; then
  echo "$x is more than 5!"
fi
```

```
if [ $x -gt 5 ]; then
  echo "$x is more than 5!"
fi
```

* -e file exists
* -s file exists and size greater than zero
* -r file exists and readable
* -w file exist and readable

https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html

## Using AND and OR in Bash

* && for AND
* || for OR

## Multiple conditions

```
x=10
if [$x -gt 5] && [ $x -lt 11]; then
  echo " $x is more than 5 and less than 11"
fi
```

## or use double square bracket

```
x=10
if [[$x -gt 5 && $x -lt 11][; then
  echo " $x is more than 5 and less than 11"
fi
```


## if and command line programs by removing square brackets

```
if grep -g Hello file.txt; then
  echo "Hello is inside!"
fi
```

or

```
if $(grep -g Hello file.txt); then
  echo "Hello is inside!"
fi
```

```
if file contains SRVM_ and vpt

Create variable from first ARGV element
sfile=$1

Create an IF statement on sfile's contents
if grep -q 'SRVM_' $sfile && grep -q 'vpt' $sfile ; then
	Move file if matched
	mv $sfile good_logs/
fi
```

File structure:
Model Name: KNN
Accuracy: 89
F1: 0.87
Date: 2019-12-01
ModelID: 34598utjfddfgdg

## Extract Accuracy from first ARGV element

accuracy=$(grep Accuracy filename.txt | sed 's/.* //')

it will find value of accuracy = 89



## FOR loops & WHILE statements

```
for x in 1 2 3
do
  echo $x
done
```

```
for x in {start..stop..increment}
do
  echo $x
done
```

```
for x in {1..5..2}
do
  echo $x
done
```

1
3
5

## For loop three expression syntax

```
for ((x=2;x<=4;x+=2))
do
  echo $x
done
```

## Glob expansions

```
for book in books/*
do
  echo $book
done
```

## Shell with in a shell to FOR loop

```
for book in $(ls books | grep -i 'air')
do
  echo $book
done
```

## WHILE statement syntax

can use of same flags for numerical comparison in IF statements

```
x=1
while [$x -le 3];
do
  echo $x
  ((x+=1))
done
```

Use a FOR statement to loop through files that end in .R
```
for file in inherited_folder/*.R
do  
    Echo out each file
    echo $file
done
```


FOR statement to loop through files that end in .py
Use an IF statement and grep to check if 'RandomForestClassifier' is in the file
Move the Python files that contain RandomForestClassifier into the to_keep/ directory

```
for file in robs_files/*.py
do  
    Create IF statement using grep
    if grep -i 'RandomForestClassifier' $file ; then
        Move wanted files to to_keep/ folder
        mv $file to_keep/
    fi
done
```


## CASE statements

case statements can be more optimal than IF statements when you have multiple or complex conditionals

'esac' is 'case' backwards and needs to be placed at the end of the case statement.

Syntax

```
case 'STRINGVAR' in
  PATTERN1)
  COMMAND;;
  PATTERN2)
  COMMAND;;
  *)
  DEFAULT COMMAND;;
esac
```

example

```
case $(cat $1) in
  *sydney*)
  mv $1 sydney/ ;;
  *melbourne*|*brisbane*)
  rm $1 ;;
  *canberra*)
  mv $1 "Importanrt_$1" ;;
  *)
  echo "No cities found" ;;
esac
```

Build a CASE statement that matches on the first ARGV element
Create a match on each weekday such as Monday, Tuesday etc. using OR syntax on a single line, then a match on each weekend day (Saturday and Sunday)
Create a default match that prints out Not a day! if none of the above patterns are matched.

```
case $1 in
  Match on all weekdays
  Monday|Tuesday|Wednesday|Thursday|Friday)
  echo "It is a Weekday!";;
  Match on all weekend days
  Saturday|Sunday)
  echo "It is a Weekend!";;
  Create a default
  *) 
  echo "Not a day!";;
esac

bash script.sh Wednesday
bash script.sh Saturday
```

Use a FOR statement to loop through (using glob expansion) files in model_out
Use a CASE statement to match on the contents of the file
It must check if the text contains a tree-based model name and move to tree_models/, otherwise delete the file.
Create a default match that prints out Unknown model in FILE

```
for file in model_out/*
do
    Create a CASE statement for each file's contents
    case $(cat $file) in
      Match on tree and non-tree models
      *"Random Forest"*|*GBM*|*XGBoost*)
      mv $file tree_models/ ;;
      *KNN*|*Logistic*)
      rm $file ;;
      Create a default
      *) 
      echo "Unknown model in $file" ;;
    esac
done
bash script.sh
```

## Basic functions in Bash

Syntax
```
function_name () {
  function code
  return #something
}
```
or 
```
function function_name {
  function code
  return #something
}
```

## Calling a bash function

is simply writing the name of function

```
function print_hello (){
  echo "Hello World!"
}

print_hello # here we call the function
```

## create a function move file name contain result

```
function upload_to_cloud () {
  Loop through files with glob expansion#
  for file in output_dir/*results*
  do
    Echo that they are being uploaded#
    echo "Uploading $file to cloud"
  done
}


Call the function#
upload_to_cloud
```


## Create function to print current day from date program

```
function what_day_is_it {

  Parse the results of date#
  current_day=$(date | cut -d " " -f1)

  Echo the result#
  echo $current_day
}

Call the function
what_day_is_it
```

cut -d " " -f1 == will split the line by delimiter (-d flag, here space), 
-f will select the column (here first column)



## Arguments, return values,and scope

Bash variables has global scope by default

use restrict global scope of variable use local keyword

Passing arguments into functions is similar to how you pass arguments into a script using the $1 notation
```
$@ or $* give all the arguments in ARGV
$# gives the length of arguments
```

Example

```
function print_filename {
  echo "The first file was $1"
  for file in $@
  do
    echo "this file has name $file"
  done
}
print_filename "LOTR.txt" "mod.txt" "A.py"
```

## Scope in programming"

'Global' means something is accessible anywhere in the program
'Local' means something is accessible in a certain part of the program

If you try and access something that has only local scope - your program may fail with an error

## Scope in Bash functions

Unlike most programming languages, all variables in Bash are global by default

## Restricing scope in Bash functions

use 'local' keyword to restrict global variable scope

## Return values

The return option in Bash is only meant to determine if the function was a success (0) or failure(other values).
It is captured in the global variable $?

TO return value 
1. Assign to a global variable
2. 'echo' what we want back (last line in function) and capture using shell-within-a-shell


## A Return error

```
function function_2{
  echlo # an error of 'echo'
}

function_2 # call the function
echo $? # print the return value
```

## Return correctly

```
function convert_temp {
  echo $(echo "scale=2; ($1-32) * 5/9" | bc)
}
coverted=$(convert_temp 30)
echo "30F in Celsius is $converted C"
```

## A function for percentage calulcation of two numbers

```
function return_percentage () {

  # Calculate the percentage using bc
  percent=$(echo "scale=2; 100 * $1 / $2" | bc)

  # Return the calculated percentage
  echo $percent
}

Call the function with 456 and 632 and echo the result#
return_test=$(return_percentage 456 632)
echo "456 out of 632 as a percent is $return_test%"
```


## function for getting most winner from txt file

```
function get_number_wins () {
  Filter aggregate results by argument#
  win_stats=$(cat soccer_scores.csv | cut -d "," -f2 | egrep -v 'Winner'| sort | uniq -c | egrep "____")
}

Call the function with specified argument#
get_number_wins "Etar"

Print out the global variable#
echo "The aggregated stats are: $win_stats"
```

## function for finding sum of array of numbers

### Create a function with a local base variable

```
function sum_array () {
  local sum=0
  Loop through, adding to base variable#
  for number in "$@"
  do
    sum=$(echo "$sum + $number" | bc)
  done
  Echo back the result#
  echo $sum
}
Call function with array#
test_array=(14 12 23.5 16 19.34)
total=$(sum_array "${test_array[@]}")
echo "The total sum of the test array is $total"
```


## Scheduling your scripts with Cron

Cron has been part of unix-like systems since the 70's
The name comes from the Gree word for time, chronos

It is driven by something called a crontab, which is a file that contains
cronjobs, which each tell crontab what code to run and when

## To see cronjobs

crontab -l

## Cronjob structure

five * in structure
miute 
hour 
day of month 
month 
day of week

## example

5 1 * * * bash mysript.sh

run every day at 1:05 am

15 14 * * 7 bash my_script.sh

every sunday 2:15pm

## Advanced cronjob structure

use comma for specific intervals
15,30,45 * * * * will run at 15, 30, 45 minutes every hour

Use a slash for every X increment 
*/15 * * * * runs every 15 minutes

# Create a schedule for 30 minutes past 2am every day
30 2 * * * bash script1.sh

# Create a schedule for every 15, 30 and 45 minutes past the hour
15,30,45 * * * * bash script2.sh

# Create a schedule for 11.30pm on Sunday evening, every week
30 23 * * 0 bash script3.sh