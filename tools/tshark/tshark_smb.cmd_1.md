```
-e frame.time_utc
-e ip.src
-e ip.dst
-e smb.access.mode
-e smb.access.sharing
-e smb.access.time
-e smb.change.time
-e smb.cmd
-e smb.create.time
-e smb.dir_name
-e smb.fid
-e smb.file
-e smb.file_attribute
-e smb.file_size
-e smb.flags.response
-e smb.last_write.time
-e smb.mid
-e smb.nt_status
-e smb.open.action.open
-e smb.path
-e smb.tid
-e smb.uid
```

# File/directory access methods
## [SMB_COM_CREATE_DIRECTORY (0x00)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/e6e870ad-7037-4b79-ac54-4a42a1ba4561)
This is an original Core Protocol command. This command is deprecated.  
Clients SHOULD use the TRANS2_CREATE_DIRECTORY subcommand.
```powershell
PS C:\users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x00" `
>> -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.file -e smb.flags.response -e smb.mid `
>> -e smb.nt_status -e smb.path -e smb.tid -e smb.uid `
>> -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\smb_cmd_0x00.csv
```
```sql
memory D SELECT timezone('UTC', "frame.time_utc") AS "frame.time_utc",
                "ip.dst",
                hex("smb.nt_status") AS "nt_status",
                "smb.tid" AS "tid",
                "smb.uid" AS "uid",
                "smb.mid" AS "mid",
                "smb.path" AS "path",
                "smb.file" AS "file",
         FROM read_csv_auto('smb_cmd_0x00.csv')
         WHERE "smb.flags.response" = true
         GROUP BY ALL
         ORDER BY "tid", "mid", "frame.time_utc"
         LIMIT 10;
┌────────────────────────────┬───────────────┬───────────┬───────┬───────┬───────┬───────────────────────────┬───────────────┐
│       frame.time_utc       │    ip.dst     │ nt_status │  tid  │  uid  │  mid  │           path            │     file      │
│         timestamp          │    varchar    │  varchar  │ int64 │ int64 │ int64 │          varchar          │    varchar    │
├────────────────────────────┼───────────────┼───────────┼───────┼───────┼───────┼───────────────────────────┼───────────────┤
│ 2005-11-19 03:35:38.95636  │ 192.168.114.1 │ 0         │  2048 │  2048 │     6 │ \\\\192.168.114.129\\TEST │ \\rawcontext  │
│ 2005-11-19 03:32:19.464982 │ 192.168.114.1 │ 0         │  2048 │  2048 │     7 │ \\\\192.168.114.129\\TEST │ \\mkdirtest   │
│ 2005-11-19 03:32:23.133675 │ 192.168.114.1 │ 0         │  2048 │  2048 │     7 │ \\\\192.168.114.129\\TEST │ \\test_notify │
│ 2005-11-19 03:32:23.165435 │ 192.168.114.1 │ 0         │  2048 │  2048 │     7 │ \\\\192.168.114.129\\TEST │ \\test_mux    │
│ 2005-11-19 03:32:27.307791 │ 192.168.114.1 │ 0         │  2048 │  2048 │     7 │ \\\\192.168.114.129\\TEST │ \\rawioctl    │
│ 2005-11-19 03:32:27.398186 │ 192.168.114.1 │ 0         │  2048 │  2048 │     7 │ \\\\192.168.114.129\\TEST │ \\rawchkpath  │
│ 2005-11-19 03:32:27.514316 │ 192.168.114.1 │ 0         │  2048 │  2048 │     7 │ \\\\192.168.114.129\\TEST │ \\testunlink  │
│ 2005-11-19 03:32:27.861034 │ 192.168.114.1 │ 0         │  2048 │  2048 │     7 │ \\\\192.168.114.129\\TEST │ \\testread    │
│ 2005-11-19 03:35:23.177188 │ 192.168.114.1 │ 0         │  2048 │  2048 │     7 │ \\\\192.168.114.129\\TEST │ \\testwrite   │
│ 2005-11-19 03:35:36.212076 │ 192.168.114.1 │ 0         │  2048 │  2048 │     7 │ \\\\192.168.114.129\\TEST │ \\testlock    │
└────────────────────────────┴───────────────┴───────────┴───────┴───────┴───────┴───────────────────────────┴───────────────┘
  10 rows                                                                                                          8 columns
```



## [SMB_COM_DELETE_DIRECTORY (0x01)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/0bca354c-42d9-46b7-a0ae-d8c6870242ca)
This is an original Core Protocol command.  
This command is used to delete an empty directory.
```powershell
PS C:\users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x01" `
>> -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.file -e smb.flags.response -e smb.mid `
>> -e smb.nt_status -e smb.path -e smb.tid -e smb.uid `
>> -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\smb_cmd_0x01.csv
```
```sql
memory D SELECT timezone('UTC', "frame.time_utc") AS "frame.time_utc",
                "ip.dst",
                hex("smb.nt_status") AS "nt_status",
                "smb.tid" AS "tid",
                "smb.uid" AS "uid",
                "smb.mid" AS "mid",
                "smb.path" AS "path",
                "smb.file" AS "file",
         FROM read_csv_auto('smb_cmd_0x01.csv')
         WHERE "smb.flags.response" = true
         GROUP BY ALL
         ORDER BY "tid", "mid", "frame.time_utc"
         LIMIT 10;
┌────────────────────────────┬───────────────┬───────────┬───────┬───────┬───────┬───────────────────────────┬──────────────────────────────────┐
│       frame.time_utc       │    ip.dst     │ nt_status │  tid  │  uid  │  mid  │           path            │               file               │
│         timestamp          │    varchar    │  varchar  │ int64 │ int64 │ int64 │          varchar          │             varchar              │
├────────────────────────────┼───────────────┼───────────┼───────┼───────┼───────┼───────────────────────────┼──────────────────────────────────┤
│ 2005-11-19 03:31:58.790701 │ 192.168.114.1 │ 0         │  2048 │  2048 │     9 │ \\\\192.168.114.129\\TEST │ \\testsfileinfo                  │
│ 2005-11-19 03:38:39.747898 │ 192.168.114.1 │ 0         │  2048 │  2048 │     9 │ \\\\192.168.114.129\\TEST │ \\testeas                        │
│ 2005-11-19 03:32:19.469621 │ 192.168.114.1 │ 0         │  2048 │  2048 │    10 │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir           │
│ 2005-11-19 03:32:14.595014 │ 192.168.114.1 │ 0         │  2048 │  2048 │    11 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_ntcreatex.dir │
│ 2005-11-19 03:32:19.47027  │ 192.168.114.1 │ C0000034  │  2048 │  2048 │    11 │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir           │
│ 2005-11-19 03:32:14.597449 │ 192.168.114.1 │ 0         │  2048 │  2048 │    12 │ \\\\192.168.114.129\\TEST │ \\rawopen                        │
│ 2005-11-19 03:32:23.141089 │ 192.168.114.1 │ 0         │  2048 │  2048 │    12 │ \\\\192.168.114.129\\TEST │ \\test_notify\\subdir-name       │
│ 2005-11-19 03:32:19.497964 │ 192.168.114.1 │ C0000103  │  2048 │  2048 │    20 │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir           │
│ 2005-11-19 03:32:23.151934 │ 192.168.114.1 │ 0         │  2048 │  2048 │    22 │ \\\\192.168.114.129\\TEST │ \\test_notify\\subdir-name       │
│ 2005-11-19 03:32:23.153277 │ 192.168.114.1 │ 0         │  2048 │  2048 │    23 │ \\\\192.168.114.129\\TEST │ \\test_notify                    │
└────────────────────────────┴───────────────┴───────────┴───────┴───────┴───────┴───────────────────────────┴──────────────────────────────────┘
  10 rows                                                                                                                             8 columns
```



## [SMB_COM_OPEN (0x02)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/ec064de8-6538-401e-8c73-b37231c36f2b)
This is an original Core Protocol command. This command has been deprecated.  
Client implementations SHOULD use SMB_COM_NT_CREATE_ANDX.  
This request is used to open an existing regular file.  
This command MUST NOT be used to open directories or named pipes.  
| **Name and bitmask** | **Values** | **Meaning** |
| --- | --- | --- |
| AccessMode<br>0x0007 | 0 | Open for reading |
| | 1 | Open for writing |
| | 2 | Open for reading and writing |
| | 3 | Open for execution |
| | 4-7 | **Reserved**. For compatibility with older dialects, the server MUST return STATUS\_OS2\_INVALID\_ACCESS (ERRDOS/ERRbadaccess) if these values are requested. |
| 0x0008 | Reserved. MUST be ignored by the server. |
| SharingMode<br>0x0070 | 0 | Compatibility mode |
| | 1 | Deny read/write/execute to others (exclusive use requested) |
| | 2 | Deny write to others |
| | 3 | Deny read/execute to others |
| | 4 | Deny nothing to others |
| | 0x0080 | **Reserved** |
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x02" `
>> -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.access.mode -e smb.access.sharing -e smb.fid -e smb.file -e smb.file_attribute -e smb.file_size -e smb.flags.response -e smb.last_write.time -e smb.mid -e smb.nt_status -e smb.path -e smb.tid -e smb.uid `
>> -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\smb_cmd_0x02.csv
```
```sql
memory D SELECT timezone('UTC', B."frame.time_utc"), B."ip.dst", A."smb.path", A."smb.file",
                hex(B."smb.nt_status"), hex(B."smb.fid"), B."smb.file_attribute", timezone('UTC', B."smb.last_write.time"), B."smb.file_size", B."smb.access.sharing", B."smb.access.mode"
         FROM read_csv_auto('smb_cmd_0x02.csv') AS A
         LEFT JOIN read_csv_auto('smb_cmd_0x02.csv') AS B
         ON B."smb.flags.response" = true AND A."smb.tid" = B."smb.tid" AND A."smb.uid" = B."smb.uid" AND A."smb.mid" = B."smb.mid"
         WHERE A."smb.flags.response" = false
         GROUP BY ALL
         ORDER BY A."smb.path", A."smb.file", timezone('UTC', B."frame.time_utc");
┌─────────────────────────────────────┬───────────────┬───────────────────────────┬─────────────────────────────┬────────────────────────┬──────────────────┬────────────────────┬──────────────────────────────────────────┬───────────────┬────────────────────┬─────────────────┐
│ timezone('UTC', B."frame.time_utc") │    ip.dst     │         smb.path          │          smb.file           │ hex(B."smb.nt_status") │ hex(B."smb.fid") │ smb.file_attribute │ timezone('UTC', B."smb.last_write.time") │ smb.file_size │ smb.access.sharing │ smb.access.mode │
│              timestamp              │    varchar    │          varchar          │           varchar           │        varchar         │     varchar      │       int64        │                timestamp                 │     int64     │       int64        │      int64      │
├─────────────────────────────────────┼───────────────┼───────────────────────────┼─────────────────────────────┼────────────────────────┼──────────────────┼────────────────────┼──────────────────────────────────────────┼───────────────┼────────────────────┼─────────────────┤
│ 2005-11-19 03:32:14.675388          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ C0000034               │ NULL             │               NULL │ NULL                                     │          NULL │               NULL │            NULL │
│ 2005-11-19 03:32:14.680764          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4003             │                 32 │ 2006-02-17 05:32:14                      │             7 │                  7 │               7 │
│ 2005-11-19 03:32:14.705524          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4003             │                 32 │ 2006-02-17 05:32:14                      │             7 │                  7 │               7 │
│ 2005-11-19 03:32:14.709424          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4004             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  0 │               0 │
│ 2005-11-19 03:32:14.71148           │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4005             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  0 │               1 │
│ 2005-11-19 03:32:14.713483          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4006             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  0 │               2 │
│ 2005-11-19 03:32:14.715467          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4007             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  2 │               2 │
│ 2005-11-19 03:32:15.706766          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ C0000043               │ NULL             │               NULL │ NULL                                     │          NULL │               NULL │            NULL │
│ 2005-11-19 03:32:15.707715          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4008             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  4 │               0 │
│ 2005-11-19 03:32:15.711281          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4009             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  0 │               0 │
└─────────────────────────────────────┴───────────────┴───────────────────────────┴─────────────────────────────┴────────────────────────┴──────────────────┴────────────────────┴──────────────────────────────────────────┴───────────────┴────────────────────┴─────────────────┘
  10 rows                                                                                                                                                                                                                                                               11 columns
```



## [SMB_COM_OPEN_ANDX (0x2D)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/49a0f97d-c4a7-48a3-bf50-46d816825729)
This command is used to create and open a new file or open an existing regular file and chain additional messages along with the request.  
The following are the commands that can follow an SMB_COM_OPEN_ANDX in an AndX chain:  
- SMB_COM_READ
- SMB_COM_READ_ANDX
- SMB_COM_IOCTL
- SMB_COM_NO_ANDX_COMMAND

| Name and <br>bitmask | Values | Meaning |
| --- | --- | --- |
| OpenResult<br>0x0003 | 1 | The file existed and was opened. |
| | 2 | The file did not exist and was therefore created. |
| | 3 | The file existed and was truncated. |
| | Other | **Reserved** |
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x2D" `
>> -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.access.mode -e smb.access.sharing -e smb.fid -e smb.file -e smb.file_attribute -e smb.file_size -e smb.flags.response -e smb.last_write.time -e smb.mid -e smb.nt_status -e smb.open.action.open -e smb.path -e smb.tid -e smb.uid `
>> -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\smb_cmd_0x2D.csv
```
```sql
memory D SELECT timezone('UTC', B."frame.time_utc"), B."ip.dst", A."smb.path", A."smb.file",
                hex(B."smb.nt_status"), hex(B."smb.fid"), B."smb.file_attribute", timezone('UTC', B."smb.last_write.time"), B."smb.file_size", B."smb.access.sharing", B."smb.access.mode", B."smb.open.action.open"
         FROM read_csv_auto('smb_cmd_0x2D.csv') AS A
         LEFT JOIN read_csv_auto('smb_cmd_0x2D.csv') AS B
         ON B."smb.flags.response" = true AND A."smb.tid" = B."smb.tid" AND A."smb.uid" = B."smb.uid" AND A."smb.mid" = B."smb.mid"
         WHERE A."smb.flags.response" = false
         GROUP BY ALL
         ORDER BY A."smb.path", A."smb.file", timezone('UTC', B."frame.time_utc")
         LIMIT 10;
┌─────────────────────────────────────┬───────────────┬───────────────────────────┬──────────────────────────────────┬────────────────────────┬──────────────────┬────────────────────┬──────────────────────────────────────────┬───────────────┬────────────────────┬─────────────────┬──────────────────────┐
│ timezone('UTC', B."frame.time_utc") │    ip.dst     │         smb.path          │             smb.file             │ hex(B."smb.nt_status") │ hex(B."smb.fid") │ smb.file_attribute │ timezone('UTC', B."smb.last_write.time") │ smb.file_size │ smb.access.sharing │ smb.access.mode │ smb.open.action.open │
│              timestamp              │    varchar    │          varchar          │             varchar              │        varchar         │     varchar      │       int64        │                timestamp                 │     int64     │       int64        │      int64      │        int64         │
├─────────────────────────────────────┼───────────────┼───────────────────────────┼──────────────────────────────────┼────────────────────────┼──────────────────┼────────────────────┼──────────────────────────────────────────┼───────────────┼────────────────────┼─────────────────┼──────────────────────┤
│ 2005-11-19 03:32:01.608824          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\A.+,;=[].B                     │ 0                      │ 4001             │                  0 │ 1970-01-01 00:00:00                      │             0 │                  0 │               0 │                    2 │
│ 2005-11-19 03:32:16.961129          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\A.+,;=[].B                     │ C0000034               │ NULL             │               NULL │ NULL                                     │          NULL │               NULL │            NULL │                 NULL │
│ 2005-11-19 03:32:01.935945          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_chained.txt   │ 0                      │ 8001             │                  0 │ 1970-01-01 00:00:00                      │             0 │                  0 │               0 │                    2 │
│ 2005-11-19 03:32:19.41718           │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_chained.txt   │ 0                      │ 800F             │                 32 │ 2006-02-17 05:32:18                      │             7 │                  0 │               2 │                    1 │
│ 2005-11-19 03:32:01.739517          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_ntcreatex.txt │ 0                      │ 8                │                  0 │ 1970-01-01 00:00:00                      │             0 │                  0 │               0 │                    2 │
│ 2005-11-19 03:32:01.749439          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_ntcreatex.txt │ 0                      │ C                │                  0 │ 1970-01-01 00:00:00                      │             0 │                  0 │               0 │                    2 │
│ 2005-11-19 03:32:01.754578          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_ntcreatex.txt │ 0                      │ F                │                  0 │ 1970-01-01 00:00:00                      │             0 │                  0 │               0 │                    2 │
│ 2005-11-19 03:32:01.760656          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_ntcreatex.txt │ 0                      │ 4003             │                  0 │ 1970-01-01 00:00:00                      │             0 │                  0 │               0 │                    2 │
│ 2005-11-19 03:32:01.767721          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_ntcreatex.txt │ 0                      │ 4007             │                  0 │ 1970-01-01 00:00:00                      │             0 │                  0 │               0 │                    2 │
│ 2005-11-19 03:32:01.775083          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_ntcreatex.txt │ 0                      │ 400A             │                  0 │ 1970-01-01 00:00:00                      │             0 │                  0 │               0 │                    2 │
└─────────────────────────────────────┴───────────────┴───────────────────────────┴──────────────────────────────────┴────────────────────────┴──────────────────┴────────────────────┴──────────────────────────────────────────┴───────────────┴────────────────────┴─────────────────┴──────────────────────┘
  10 rows                                                                                                                                                                                                                                                                                           12 columns
```



## [SMB_COM_CREATE (0x03)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/87622f43-3758-4bf9-b1fb-35109f0e5c15)
This is an original Core Protocol command. This command is deprecated.  
Implementations SHOULD use SMB_COM_NT_CREATE_ANDX.
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x03" `
>> -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.create.time -e smb.fid -e smb.file -e smb.file_attribute -e smb.flags.response -e smb.mid -e smb.nt_status -e smb.path -e smb.tid -e smb.uid `
>> -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\smb_cmd_0x03.csv
```
```sql
memory D SELECT timezone('UTC', B."frame.time_utc"), B."ip.dst", A."smb.path", A."smb.file", A."smb.file_attribute", timezone('UTC', A."smb.create.time"),
                hex(B."smb.nt_status"), hex(B."smb.fid")
         FROM read_csv_auto('smb_cmd_0x03.csv') AS A
         LEFT JOIN read_csv_auto('smb_cmd_0x03.csv') AS B
         ON B."smb.flags.response" = true AND A."smb.tid" = B."smb.tid" AND A."smb.uid" = B."smb.uid" AND A."smb.mid" = B."smb.mid"
         WHERE A."smb.flags.response" = false
         GROUP BY ALL
         ORDER BY A."smb.path", A."smb.file", timezone('UTC', B."frame.time_utc");
┌─────────────────────────────────────┬───────────────┬───────────────────────────┬───────────────────────────────┬────────────────────┬──────────────────────────────────────┬────────────────────────┬──────────────────┐
│ timezone('UTC', B."frame.time_utc") │    ip.dst     │         smb.path          │           smb.file            │ smb.file_attribute │ timezone('UTC', A."smb.create.time") │ hex(B."smb.nt_status") │ hex(B."smb.fid") │
│              timestamp              │    varchar    │          varchar          │            varchar            │       int64        │              timestamp               │        varchar         │     varchar      │
├─────────────────────────────────────┼───────────────┼───────────────────────────┼───────────────────────────────┼────────────────────┼──────────────────────────────────────┼────────────────────────┼──────────────────┤
│ 2005-11-19 03:32:19.3596            │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_create.txt │                  0 │ 1970-01-01 00:00:00                  │ 0                      │ 8009             │
│ 2005-11-19 03:32:19.361263          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_create.txt │                  0 │ 1970-01-01 00:00:00                  │ 0                      │ 800A             │
│ 2005-11-19 03:32:19.369521          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_create.txt │                  0 │ 2005-11-22 05:32:18                  │ 0                      │ 800B             │
│ 2005-11-19 03:32:19.385549          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_create.txt │                  2 │ 2005-11-22 05:32:18                  │ 0                      │ 800C             │
└─────────────────────────────────────┴───────────────┴───────────────────────────┴───────────────────────────────┴────────────────────┴──────────────────────────────────────┴────────────────────────┴──────────────────┘
```



## [SMB_COM_CREATE_NEW (0x0F)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/161fa213-ba9d-4bad-9483-29e8b5872dca)
This is an original Core Protocol command. This command is deprecated.  
Implementations SHOULD use SMB_COM_NT_CREATE_ANDX.  
This command is used to create a new file. It MUST NOT truncate or overwrite an existing file.  
If a file with the requested pathname already exits, the command MUST fail.  
This command MUST NOT be used to create directories or named pipes.
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x0F" `
>> -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.create.time -e smb.fid -e smb.file -e smb.file_attribute -e smb.flags.response -e smb.mid -e smb.nt_status -e smb.path -e smb.tid -e smb.uid `
>> -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\smb_cmd_0x0F.csv
```
```sql
memory D SELECT timezone('UTC', B."frame.time_utc"), B."ip.dst", A."smb.path", A."smb.file", A."smb.file_attribute", timezone('UTC', A."smb.create.time"),
                hex(B."smb.nt_status"), hex(B."smb.fid")
         FROM read_csv_auto('smb_cmd_0x0F.csv') AS A
         LEFT JOIN read_csv_auto('smb_cmd_0x0F.csv') AS B
         ON B."smb.flags.response" = true AND A."smb.tid" = B."smb.tid" AND A."smb.uid" = B."smb.uid" AND A."smb.mid" = B."smb.mid"
         WHERE A."smb.flags.response" = false
         GROUP BY ALL
         ORDER BY A."smb.path", A."smb.file", timezone('UTC', B."frame.time_utc");
┌─────────────────────────────────────┬───────────────┬───────────────────────────┬──────────────────────────────┬────────────────────┬──────────────────────────────────────┬────────────────────────┬──────────────────┐
│ timezone('UTC', B."frame.time_utc") │    ip.dst     │         smb.path          │           smb.file           │ smb.file_attribute │ timezone('UTC', A."smb.create.time") │ hex(B."smb.nt_status") │ hex(B."smb.fid") │
│              timestamp              │    varchar    │          varchar          │           varchar            │       int64        │              timestamp               │        varchar         │     varchar      │
├─────────────────────────────────────┼───────────────┼───────────────────────────┼──────────────────────────────┼────────────────────┼──────────────────────────────────────┼────────────────────────┼──────────────────┤
│ 2005-11-19 03:32:19.325342          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_mknew.txt │                  0 │ 1970-01-01 00:00:00                  │ 0                      │ 8006             │
│ 2005-11-19 03:32:19.326266          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_mknew.txt │                  0 │ 1970-01-01 00:00:00                  │ C0000035               │ NULL             │
│ 2005-11-19 03:32:19.329692          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_mknew.txt │                  0 │ 2005-11-22 05:32:18                  │ 0                      │ 8007             │
│ 2005-11-19 03:32:19.350652          │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_mknew.txt │                  2 │ 2005-11-22 05:32:18                  │ 0                      │ 8008             │
└─────────────────────────────────────┴───────────────┴───────────────────────────┴──────────────────────────────┴────────────────────┴──────────────────────────────────────┴────────────────────────┴──────────────────┘
```



## [SMB_COM_CREATE_TEMPORARY (0x0E)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/6ea3a4b2-2a9b-4749-a4a4-41efebdf4015)
This is an original Core Protocol command. This command is obsolescent.
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x0E" `
>> -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.create.time -e smb.dir_name -e smb.fid -e smb.file -e smb.flags.response -e smb.mid -e smb.nt_status -e smb.path -e smb.tid -e smb.uid `
>> -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\smb_cmd_0x0E.csv
```
```sql
memory D SELECT timezone('UTC', B."frame.time_utc"), B."ip.dst", A."smb.path", A."smb.dir_name", timezone('UTC', A."smb.create.time"),
                hex(B."smb.nt_status"), hex(B."smb.fid"), B."smb.file"
         FROM read_csv_auto('smb_cmd_0x0E.csv') AS A
         LEFT JOIN read_csv_auto('smb_cmd_0x0E.csv') AS B
         ON B."smb.flags.response" = true AND A."smb.tid" = B."smb.tid" AND A."smb.uid" = B."smb.uid" AND A."smb.mid" = B."smb.mid"
         WHERE A."smb.flags.response" = false
         GROUP BY ALL
         ORDER BY A."smb.path", B."smb.file", timezone('UTC', B."frame.time_utc");
┌─────────────────────────────────────┬───────────────┬───────────────────────────┬──────────────┬──────────────────────────────────────┬────────────────────────┬──────────────────┬──────────┐
│ timezone('UTC', A."frame.time_utc") │    ip.dst     │         smb.path          │ smb.dir_name │ timezone('UTC', A."smb.create.time") │ hex(B."smb.nt_status") │ hex(B."smb.fid") │ smb.file │
│              timestamp              │    varchar    │          varchar          │   varchar    │              timestamp               │        varchar         │     varchar      │ varchar  │
├─────────────────────────────────────┼───────────────┼───────────────────────────┼──────────────┼──────────────────────────────────────┼────────────────────────┼──────────────────┼──────────┤
│ 2005-11-19 03:32:19.39454           │ 192.168.114.1 │ \\\\192.168.114.129\\TEST │ \\rawopen    │ 2005-11-22 05:32:18                  │ 0                      │ 800D             │ SRVB9D82 │
└─────────────────────────────────────┴───────────────┴───────────────────────────┴──────────────┴──────────────────────────────────────┴────────────────────────┴──────────────────┴──────────┘
```



## [SMB_COM_NT_CREATE_ANDX (0xA2)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/d3f83a7e-493b-4d29-b21c-55768b93e144)




# yet
## 0x06 (SMB_COM_DELETE)
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x06" -T fields -e ip.src -e ip.dst -e smb.cmd -e smb.nt_status -e smb.flags.response -e smb.path -e smb.file -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x06.csv
```
```sql
memory D SELECT hex("smb.nt_status"), "smb.path", "smb.file", COUNT(*)
         FROM read_csv_auto('0x06.csv')
         WHERE "ip.src" = '192.168.114.129' AND "ip.dst" = '192.168.114.1'
         AND "smb.cmd" = '0x06' AND "smb.flags.response" = 'true'
         GROUP BY ALL
         ORDER BY "smb.path", "smb.file"
         LIMIT 10;
┌──────────────────────┬───────────────────────────┬────────────────────────┬──────────────┐
│ hex("smb.nt_status") │         smb.path          │        smb.file        │ count_star() │
│       varchar        │          varchar          │        varchar         │    int64     │
├──────────────────────┼───────────────────────────┼────────────────────────┼──────────────┤
│ C000003B             │ \\\\192.168.114.129\\TEST │ ..                     │            1 │
│ C000003B             │ \\\\192.168.114.129\\TEST │ \\..                   │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\SRVB9D82             │            1 │
│ C0000034             │ \\\\192.168.114.129\\TEST │ \\composite            │            1 │
│ C00000BA             │ \\\\192.168.114.129\\TEST │ \\mkdirtest            │            1 │
│ C0000034             │ \\\\192.168.114.129\\TEST │ \\mkdirtest            │            1 │
│ C000000F             │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\*         │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir │            1 │
│ C0000034             │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir │            1 │
│ C00000BA             │ \\\\192.168.114.129\\TEST │ \\rawchkpath           │            1 │
└──────────────────────┴───────────────────────────┴────────────────────────┴──────────────┘
  10 rows                                                                        4 columns
```



## 0x07 (SMB_COM_RENAME)
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x07" -T fields -e ip.src -e ip.dst -e smb.cmd -e smb.nt_status -e smb.flags.response -e smb.path -e smb.old_file -e smb.file -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x07.csv
```
```sql
memory D SELECT hex("smb.nt_status"), "smb.path", "smb.old_file", "smb.file", COUNT(*)
         FROM read_csv_auto('0x07.csv')
         WHERE "ip.src" = '192.168.114.129' AND "ip.dst" = '192.168.114.1'
         AND "smb.cmd" = '0x07' AND "smb.flags.response" = 'true'
         GROUP BY ALL
         ORDER BY "smb.path", "smb.old_file";
┌──────────────────────┬───────────────────────────┬─────────────────────────┬─────────────────────────┬──────────────┐
│ hex("smb.nt_status") │         smb.path          │      smb.old_file       │        smb.file         │ count_star() │
│       varchar        │          varchar          │         varchar         │         varchar         │    int64     │
├──────────────────────┼───────────────────────────┼─────────────────────────┼─────────────────────────┼──────────────┤
│ C000000F             │ \\\\192.168.114.129\\TEST │ \\testrename\\*.txt     │ \\testrename\\*.bak     │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\testrename\\*.txt     │ \\testrename\\*.bak     │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\testrename\\*.txt     │ \\testrename\\test1.txt │            2 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\testrename\\test1.bak │ \\testrename\\*.txt     │            1 │
│ C000000F             │ \\\\192.168.114.129\\TEST │ \\testrename\\test1.bak │ \\testrename\\*.txt     │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\testrename\\test1.txt │ \\testrename\\test2.txt │            2 │
│ C0000034             │ \\\\192.168.114.129\\TEST │ \\testrename\\test1.txt │ \\testrename\\test1.txt │            1 │
│ C0000043             │ \\\\192.168.114.129\\TEST │ \\testrename\\test1.txt │ \\testrename\\test2.txt │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\testrename\\test2.txt │ \\testrename\\test1.txt │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\testrename\\test2.txt │ \\testrename\\test2.txt │            1 │
└──────────────────────┴───────────────────────────┴─────────────────────────┴─────────────────────────┴──────────────┘
  10 rows                                                                                                   5 columns
```



## 0x08 (SMB_COM_QUERY_INFORMATION)
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x08" -T fields -e ip.src -e ip.dst -e smb.cmd -e smb.nt_status -e smb.flags.response -e smb.path -e smb.tid -e smb.uid -e smb.mid -e smb.file -e smb.file_attribute -e smb.last_write.time -e smb.file_size -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x08.csv
```
```sql
memory D SELECT hex(B."smb.nt_status"), A."smb.path", A."smb.file", B."smb.file_attribute", B."smb.last_write.time", B."smb.file_size", COUNT(*)
         FROM read_csv_auto('0x08.csv') AS A
         LEFT JOIN read_csv_auto('0x08.csv') AS B
         ON B."smb.flags.response" = true AND A."smb.tid" = B."smb.tid" AND A."smb.uid" = B."smb.uid" AND A."smb.mid" = B."smb.mid"
         WHERE A."ip.src" = '192.168.114.1' AND A."ip.dst" = '192.168.114.129'
         AND A."smb.cmd" = '0x08' AND A."smb.flags.response" = 'false'
         GROUP BY ALL
         ORDER BY A."smb.path", A."smb.file";
┌────────────────────────┬───────────────────────────┬─────────────────────────┬────────────────────┬──────────────────────────┬───────────────┬──────────────┐
│ hex(B."smb.nt_status") │         smb.path          │        smb.file         │ smb.file_attribute │   smb.last_write.time    │ smb.file_size │ count_star() │
│        varchar         │          varchar          │         varchar         │       int64        │ timestamp with time zone │     int64     │    int64     │
├────────────────────────┼───────────────────────────┼─────────────────────────┼────────────────────┼──────────────────────────┼───────────────┼──────────────┤
│ 0                      │ \\\\192.168.114.129\\TEST │ \\torture_qfileinfo.txt │                 32 │ 2006-02-17 14:31:58+09   │             7 │            1 │
└────────────────────────┴───────────────────────────┴─────────────────────────┴────────────────────┴──────────────────────────┴───────────────┴──────────────┘
```



## 0x09 (SMB_COM_SET_INFORMATION)
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x09" -T fields -e ip.src -e ip.dst -e smb.cmd -e smb.nt_status -e smb.flags.response -e smb.path -e smb.tid -e smb.uid -e smb.mid -e smb.file -e smb.file_attribute -e smb.last_write.time -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x09.csv
```
```sql
memory D SELECT hex(B."smb.nt_status"), A."smb.path", A."smb.file", A."smb.file_attribute", A."smb.last_write.time", COUNT(*)
         FROM read_csv_auto('0x09.csv') AS A
         LEFT JOIN read_csv_auto('0x09.csv') AS B
         ON B."smb.flags.response" = true AND A."smb.tid" = B."smb.tid" AND A."smb.uid" = B."smb.uid" AND A."smb.mid" = B."smb.mid"
         WHERE A."ip.src" = '192.168.114.1' AND A."ip.dst" = '192.168.114.129'
         AND A."smb.cmd" = '0x09' AND A."smb.flags.response" = 'false'
         GROUP BY ALL
         ORDER BY A."smb.path", A."smb.file";
┌────────────────────────┬───────────────────────────┬────────────────────────────────────┬────────────────────┬──────────────────────────┬──────────────┐
│ hex(B."smb.nt_status") │         smb.path          │              smb.file              │ smb.file_attribute │   smb.last_write.time    │ count_star() │
│        varchar         │          varchar          │              varchar               │       int64        │ timestamp with time zone │    int64     │
├────────────────────────┼───────────────────────────┼────────────────────────────────────┼────────────────────┼──────────────────────────┼──────────────┤
│ 0                      │ \\\\192.168.114.129\\TEST │ \\testsfileinfo\\fname_test_18.txt │                  1 │ 2005-11-18 14:31:58+09   │            1 │
│ 0                      │ \\\\192.168.114.129\\TEST │ \\testsfileinfo\\fname_test_18.txt │                  0 │ 1970-01-01 09:00:00+09   │            1 │
│ 0                      │ \\\\192.168.114.129\\TEST │ \\testsfileinfo\\fname_test_18.txt │                128 │ 1970-01-01 09:00:00+09   │            1 │
└────────────────────────┴───────────────────────────┴────────────────────────────────────┴────────────────────┴──────────────────────────┴──────────────┘
```



## 0x0A (SMB_COM_READ)
