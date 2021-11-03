# Downloading Data on the Command Line

## Downloading data using curl

### curl

short for Client for URLs
transfers data to and from a server
used to download data http(s) and ftp servers

## To check curl installation

man curl

## To install curl

https://curl.haxx.se/download.html

## Curl syntax

curl [option flags] [URL]

## To save a file with its original name

curl -O https://websitename.com/datafilename.txt

### To rename the file use lower case -o + new file name

curl -O renameddatafilename.txt https://websitename.com/datafilename.txt

## Downloading Multiple files using Wildcards

### using wildcards (*)

curl -O https://websitename.com/datafilename*.txt

### using Globbing Parser

curl -O https://websitename.com/datafilename[001-100].txt

Increment through every files and download every Nth file

curl -O https://websitename.com/datafilename[001-100:10].txt

## Other useful option flags
-L Redirects the HTTP URL if a 300 error code occurs
-C Resumes a previous file transfer if it times out before completion

curl -L -O -C https://websitename.com/datafilename[001-100:10].txt

order of flags does not matter

## for more details
man curl or curl --help


## Downloading data using Wget

wget  derive its name from World Wide Web and get
used to download data from HTTP(s) and FTP
better than curl at downloading multple files reursively

## Check Wget installation

which wget

## Installation

### Linux

```sudo apt-get install wget```

### macos 

```brew install wget```

### Windows 

```download via gnuwin32```


## Wget syntax

wget [option flags] [URL]

* -b : Go to background immediately after startup
* -q : Turn off the Wget output
* -c : Resume broken download(continue getting a partial downloaded file)

> wget -bqc https://websitename.com/datafilename.txt


## Advanced downloading using Wget

### Multiple file downloading with Wget

> Save a list of file locations in a text file

```wget -i urls_list.txt```

> -i : download from the URL locations stored within the external or local file
other option flag should put before -i


### Setting download bandwidth limit for large file download

```
wget --limit-rate={rate}k {file_location}
wget --limit-rate=200k -i url_list.txt
```

### Set a mandatory pause time (in seconds) between file downloads with --wait

```
wget --wait={seconds} {file_location}
wget --wait=2.5 -i url_list.xt
```

2.5s pause bw downloading each file in urls saved in url_list txt file
For download smaller files, enforcing a mandatory wait time between file downloads makes sure we don't overload the server with too many requests.



## curl vs wget

curl advantages
can be used for uploading and downloading files from 20+ protocols
easier to install across all os

wget advantages
has built-in functionalities for handling multiple files downloads
Can handle various file formats for download (directory, html page)


# Data Cleaning and Munging on the Command Line

## Getting started with csvkit

is a suite of command line tools developed by Wireservice using python
offers data processing and cleaning capabilities on CSV files

### csvkit installation
```pip install csvkit```

### Upgrade csv to the latest version
```pip install --upgrade csvkit```

### Full instructions
> https://csvkit.readthedocs.io/en/latest/tutorial.html


## in2csv: converting files to CSV

### syntax
```in2csv SpotifyData.xlsx > SpotifyData.csv```


### To print all sheets in excelfile

use --names or -n

```in2csv -n filename.xlsx```

Use --sheet option followed be sheetname to be converted if excel file contain multiple sheets
```in2csv filename.xlsx --sheet "Sheetname" > newfilename.csv```


## data preview on the command line

csvlook prints csv files to commandline
```csvlook filename.csv```

```
csvlook manual
csvlook -h
csvlook --help
```

## Description stats on csv files
csvstat : prints descriptive summary statistics on all columns in csv
similar to describe() in Pythons' Panda library


## Filtering data using csvkit

### csvcut : filters data using column name or position

### csvgrep : filters data by row value through exact match, pattern matching or even regex

## csvcut uses --name or -n option to print all column names

```csvcut -n filename.csv```

csvcut -c {position} filename.csv
csvcut -c '{columnname}' filename.csv

### returns only the first column in data

```csvcut -c 1 filename.csv```
csvcut -c "track_id" filename.csv

### multiple columns

```
csvcut -c 2,3 filename.csv
csvcut -c "track_id","danceability" filename.csv
```


## csvgrep:

* -m : followed by the exact row value to filter
*  -r : followed with a regex pattern
* -f : followed by the path to a file

### syntax

```
csvgrep -c "track_id" -m "{searchvalue}" {filename.csv}
csvgrep -c 1 -m "{searchvalue}" {filename.csv}
```

```
csvcut -n Spotify_MusicAttributes.csv
csvgrep -c "danceability" -m 0.812 Spotify_MusicAttributes.csv
```


## Stacking data and chaining commands with csvkit

csvstack : stacks up the rows from two or more csv files
it used for csv files with same schema. it may downloaded in chunks duee to api download restrictions

### syntax

```
csvstack {file1.csv} {file2.csv} > {filestacked.csv}
csvstack SpotifyData_PopularityRank6.csv SpotifyData_PopularityRank7.csv > SpotifyPopularity.csv
```

### options
* -g create new column based on original file source
* -n

> ; links commands and runs sequentially

```csvlook file.csv; csvstat file.csv```

> && links commands together, but only runs the 2nd command if the 1st succeeds

```csvlook Spotify_Popularity.csv && csvstat Spotify_Popularity.csv```

> > re-directs the output from the 1st command to the location indicated as the 2nd

> | uses the output of the 1st command as input to the 2nd
``` csvsort -c 2 Spotify_Popularity.csv | csvlook ```

> top 15 rows from sorted output and save to new file
``` csvsort -c 2 Spotify_Popularity.csv | head -n 15 > Spotify_Popularity_Top15.csv ```


# Convert the Spotify201809 sheet into its own csv file 

```in2csv Spotify_201809_201810.xlsx --sheet "Spotify201809" > Spotify201809.csv```

# Create a new csv with 2 columns: track_id and popularity

``` csvcut -c "track_id","popularity" Spotify201809.csv > Spotify201809_subset.csv```

# While stacking the 2 files, create a data source column

``` csvstack -g "Sep2018","Oct2018" Spotify201809_subset.csv Spotify201810_subset.csv > Spotify_all_rankings.csv```



# Database Operations on the Command Line

## Pulling data from database

### sql2csv

is a command in csvkit that allows to access databases.
it pull data from database and output the result to a CSV file

syntax
```
sql2csv --db "sqlite:///spotifydatabase.db" \
        --query "SELECT * FROM Spotify_popularity" \
        > Spotify_Popularity.csv
```

SQLite : starts with sqlite:/// and end with .db
Postgres & MySQL : starts with postgres:/// or mysql:/// and no .db


> Save query to new file Spotify_Popularity_5Rows.csv
sql2csv --db "sqlite:///SpotifyDatabase.db" \
        --query "SELECT * FROM Spotify_Popularity LIMIT 5" \
        > Spotify_Popularity_5Rows.csv


## Manipulating data using SQL syntax

csvsql : applies SQL statements to one or more csv files

csvsql --query "select * from a table" file_a.csv file_b.csv

## Apply SQL query to Spotify_MusicAttributes.csv

```csvsql --query "SELECT * FROM Spotify_MusicAttributes ORDER BY duration_ms LIMIT 1" Spotify_MusicAttributes.csv```

## output by piping the output to csvlook.

```
csvsql --query "SELECT * FROM Spotify_MusicAttributes ORDER BY duration_ms LIMIT 1" \
	Spotify_MusicAttributes.csv | csvlook
```

## Instead of printing to console, re-direct output and save as new file: LongestSong.csv

```
csvsql --query "SELECT * FROM Spotify_MusicAttributes ORDER BY duration_ms LIMIT 1" \
	Spotify_MusicAttributes.csv > LongestSong.csv
```

> we will frequently need to deal with line breaks while passing in SQL queries to csvkit commands.

> Store SQL query as shell variable
sqlquery="SELECT * FROM Spotify_MusicAttributes ORDER BY duration_ms LIMIT 1"

> Apply SQL query to Spotify_MusicAttributes.csv
csvsql --query "$sqlquery" Spotify_MusicAttributes.csv


## Pushing data back to database

```
csvsql --no-inference --no-constraints \
       --db "sqlite:///spotify.db" \
       --insert spotify.csv
```

--no-inference  disable type inference when parsing the input
--no-constraints  generate a schema without length limits or null checks


### Upload Spotify_MusicAttributes.csv to database

csvsql --db "sqlite:///SpotifyDatabase.db" --insert Spotify_MusicAttributes.csv

### Store SQL query as shell variable

sqlquery="SELECT * FROM Spotify_MusicAttributes"

### Apply SQL query to re-pull new table in database

sql2csv --db "sqlite:///SpotifyDatabase.db" --query "$sqlquery" 

### Store SQL for querying from SQLite database 

sqlquery_pull="SELECT * FROM SpotifyMostRecentData"

### Apply SQL to save table as local file 

sql2csv --db "sqlite:///SpotifyDatabase.db" --query "$sqlquery_pull" > SpotifyMostRecentData.csv

### Store SQL for UNION of the two local CSV files

sqlquery_union="SELECT * FROM SpotifyMostRecentData UNION ALL SELECT * FROM Spotify201812"

### Apply SQL to union the two local CSV files and save as local file

csvsql 	--query "$sqlquery_union" SpotifyMostRecentData.csv Spotify201812.csv > UnionedSpotifyData.csv


### Push UnionedSpotifyData.csv to database as a new table

csvsql --db "sqlite:///SpotifyDatabase.db" --insert UnionedSpotifyData.csv