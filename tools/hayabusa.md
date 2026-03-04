- https://www.appliedincidentresponse.com/files/Default-Windows-Processes-Quick-Reference.pdf

## COMMAND
### logon-summary
```bat
# hayabusa-3.8.1-win-x64.exe logon-summary -d "E:\c\Windows\System32\winevt\logs" -o test
.\hayabusa-3.8.1-win-x64.exe logon-summary -d "DIRECTORY" -o logon
```
### extract-base64
```bat
.\hayabusa-3.8.1-win-x64.exe extract-base64 -d "DIRECTORY" -o base64.csv
```
### csv-timeline
```
.\hayabusa-3.8.1-win-x64.exe csv-timeline -d "DIRECTORY" -w -m informational -o timeline.csv
```
