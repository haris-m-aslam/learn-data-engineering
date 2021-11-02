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
