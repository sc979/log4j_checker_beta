# log4j_checker_beta - Windows and Linux Scripts

If you want to use the windows/linux download cradles below, please review the scripts beforehand. Loading code from the internet and execute it is what brought as here with Log4Shell - right? ;-)

You can easily host the scripts on an internal https-webserver and point the scripts to this server for execution. 

## Linux Version

This script is used to perform a fast check if your server is possibly affected by CVE-2021-44228 (the log4j vulnerability).
It does not give a 100% proof, that you are not vulnerable, but it gives a hint if it is possible, that you could be vulnerable.

- scans files for occurrences of log4j
- checks for packages containing log4j and Solr ElasticSearch
- checks if Java is installed
- Analyzes JAR/WAR/EAR files
- Option of checking hashes of .class files in archives

### Run with:

    sudo updatedb && wget https://raw.githubusercontent.com/anlx-sw/log4j_checker_beta/main/log4j_checker_beta.sh -q -O - |bash

### Hash checking

The script can test .class files on the first level of JAR/WAR/EAR archives to see if they match with known sha256 hashes of vulnerable class files from log4j.
You have to provide a download of plain text file with sha256 hashes in HEX format, one per line, everything after first <space> is ignored.
The URL can be placed in variable download_file. Otherwise this feature will not operate.

### dependencies

The command `locate` has to to be installed, be sure to have locate up-to-date with:

    sudo updatedb
    
The command `unzip` also needs to be installed, to inspect the jar files.

### discussion

https://serverfault.com/questions/1086065/how-do-i-check-if-log4j-is-installed-on-my-server/1086132#1086132

    
## Check Windows Hosts
    
You can use the snippet
    
    IEX (New-Object Net.Webclient).downloadstring("https://raw.githubusercontent.com/anlx-sw/log4j_checker_beta/main/get-vulnerablejars.ps1")
    
to directly check for potentially vulnerable jar files on windows. Or you could download [this  script](get-vulnerablejars-win.ps1) and point the output to a writable fileshare.

Error handling or additional features where not added on purpose to keep it short and easy and compatible with Powershell 2.0 and older versions of Windows. This script just checks for jar files and searches for the string "JndiLookup.class" which indicates a vulnerable library. Use your tool of choice to run it with admin permissions (or just use it manually).
