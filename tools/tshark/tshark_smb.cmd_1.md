```
-e frame.time_utc
-e ip.src
-e ip.dst
-e smb.access.mode
-e smb.access.sharing
-e smb.cmd
-e smb.fid
-e smb.file
-e smb.file_attribute
-e smb.file_size
-e smb.flags.response
-e smb.last_write.time
-e smb.mid
-e smb.nt_status
-e smb.path
-e smb.tid
-e smb.uid
```

# File/directory access methods
## [0x00 (SMB_COM_CREATE_DIRECTORY)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/e6e870ad-7037-4b79-ac54-4a42a1ba4561)
This is an original Core Protocol command. This command is deprecated.  
Clients SHOULD use the TRANS2_CREATE_DIRECTORY subcommand.
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x00" `
>> -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.file -e smb.flags.response -e smb.nt_status -e smb.path `
>> -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\smb_cmd_0x00.csv
```
```sql
memory D SELECT timezone('UTC', "frame.time_utc"), "ip.dst", hex("smb.nt_status"), "smb.path", "smb.file"
         FROM read_csv_auto('smb_cmd_0x00.csv')
         WHERE "smb.flags.response" = true
         GROUP BY ALL
         ORDER BY "smb.path", "smb.file", timezone('UTC', "frame.time_utc")
         LIMIT 10;
┌───────────────────────────────────┬───────────────┬──────────────────────┬───────────────────────────┬─────────────────────────────┐
│ timezone('UTC', "frame.time_utc") │    ip.dst     │ hex("smb.nt_status") │         smb.path          │          smb.file           │
│             timestamp             │    varchar    │       varchar        │          varchar          │           varchar           │
├───────────────────────────────────┼───────────────┼──────────────────────┼───────────────────────────┼─────────────────────────────┤
│ 2005-11-19 03:32:19.50011         │ 192.168.114.1 │ C000003B             │ \\\\192.168.114.129\\TEST │ ..\\..\\..                  │
│ 2005-11-19 03:38:43.617968        │ 192.168.114.1 │ 0                    │ \\\\192.168.114.129\\TEST │ \\composite                 │
│ 2005-11-19 03:32:19.464982        │ 192.168.114.1 │ 0                    │ \\\\192.168.114.129\\TEST │ \\mkdirtest                 │
│ 2005-11-19 03:32:19.466924        │ 192.168.114.1 │ 0                    │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir      │
│ 2005-11-19 03:32:19.468621        │ 192.168.114.1 │ C0000035             │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir      │
│ 2005-11-19 03:32:19.497209        │ 192.168.114.1 │ C0000035             │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir      │
│ 2005-11-19 03:32:27.398186        │ 192.168.114.1 │ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath                │
│ 2005-11-19 03:32:27.400375        │ 192.168.114.1 │ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt            │
│ 2005-11-19 03:32:27.404397        │ 192.168.114.1 │ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt\\V S       │
│ 2005-11-19 03:32:27.407359        │ 192.168.114.1 │ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt\\V S\\VB98 │
└───────────────────────────────────┴───────────────┴──────────────────────┴───────────────────────────┴─────────────────────────────┘
  10 rows                                                                                                                  5 columns
```



## [0x02 (SMB_COM_OPEN)](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/ec064de8-6538-401e-8c73-b37231c36f2b)
This is an original Core Protocol command. This command has been deprecated. Client implementations SHOULD use SMB_COM_NT_CREATE_ANDX.  
This request is used to open an existing regular file. This command MUST NOT be used to open directories or named pipes.  
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
>> -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.access.mode -e smb.access.sharing -e smb.cmd -e smb.fid -e smb.file -e smb.file_attribute -e smb.file_size -e smb.flags.response -e smb.last_write.time -e smb.mid -e smb.nt_status -e smb.path -e smb.tid -e smb.uid `
>> -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\smb_cmd_0x02.csv
```
```sql
memory D SELECT timezone('UTC', A."frame.time_utc"), A."smb.path", A."smb.file",
                hex(B."smb.nt_status"), hex(B."smb.fid"), B."smb.file_attribute", timezone('UTC', B."smb.last_write.time"), B."smb.file_size", B."smb.access.sharing", B."smb.access.mode"
         FROM read_csv_auto('0x02.csv') AS A
         LEFT JOIN read_csv_auto('smb_cmd_0x02.csv') AS B
         ON B."smb.flags.response" = true AND A."smb.cmd" = B."smb.cmd" AND A."smb.tid" = B."smb.tid" AND A."smb.uid" = B."smb.uid" AND A."smb.mid" = B."smb.mid"
         WHERE A."smb.flags.response" = false
         GROUP BY ALL
         ORDER BY A."smb.path", A."smb.file", timezone('UTC', A."frame.time_utc") ;
┌─────────────────────────────────────┬───────────────────────────┬─────────────────────────────┬────────────────────────┬──────────────────┬────────────────────┬──────────────────────────────────────────┬───────────────┬────────────────────┬─────────────────┐
│ timezone('UTC', A."frame.time_utc") │         smb.path          │          smb.file           │ hex(B."smb.nt_status") │ hex(B."smb.fid") │ smb.file_attribute │ timezone('UTC', B."smb.last_write.time") │ smb.file_size │ smb.access.sharing │ smb.access.mode │
│              timestamp              │          varchar          │           varchar           │        varchar         │     varchar      │       int64        │                timestamp                 │     int64     │       int64        │      int64      │
├─────────────────────────────────────┼───────────────────────────┼─────────────────────────────┼────────────────────────┼──────────────────┼────────────────────┼──────────────────────────────────────────┼───────────────┼────────────────────┼─────────────────┤
│ 2005-11-19 03:32:14.673931          │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ C0000034               │ NULL             │               NULL │ NULL                                     │          NULL │               NULL │            NULL │
│ 2005-11-19 03:32:14.679815          │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4003             │                 32 │ 2006-02-17 05:32:14                      │             7 │                  7 │               7 │
│ 2005-11-19 03:32:14.70511           │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4003             │                 32 │ 2006-02-17 05:32:14                      │             7 │                  7 │               7 │
│ 2005-11-19 03:32:14.708386          │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4004             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  0 │               0 │
│ 2005-11-19 03:32:14.710849          │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4005             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  0 │               1 │
│ 2005-11-19 03:32:14.713179          │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4006             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  0 │               2 │
│ 2005-11-19 03:32:14.715195          │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4007             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  2 │               2 │
│ 2005-11-19 03:32:14.716214          │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ C0000043               │ NULL             │               NULL │ NULL                                     │          NULL │               NULL │            NULL │
│ 2005-11-19 03:32:15.707192          │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4008             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  4 │               0 │
│ 2005-11-19 03:32:15.710867          │ \\\\192.168.114.129\\TEST │ \\rawopen\\torture_open.txt │ 0                      │ 4009             │                 32 │ 2005-11-19 05:32:16                      │             7 │                  0 │               0 │
└─────────────────────────────────────┴───────────────────────────┴─────────────────────────────┴────────────────────────┴──────────────────┴────────────────────┴──────────────────────────────────────────┴───────────────┴────────────────────┴─────────────────┘
  10 rows                                                                                                                                                                                                                                               10 columns
```



# yet
## 0x01 (SMB_COM_DELETE_DIRECTORY)
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x01" -T fields -e ip.src -e ip.dst -e smb.cmd -e smb.nt_status -e smb.flags.response -e smb.path -e smb.file -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x01.csv
```
```sql
memory D SELECT hex("smb.nt_status"), "smb.path", "smb.file", COUNT(*)
         FROM read_csv_auto('0x01.csv')
         WHERE "ip.src" = '192.168.114.129' AND "ip.dst" = '192.168.114.1'
         AND "smb.cmd" = '0x01' AND "smb.flags.response" = true
         GROUP BY ALL
         ORDER BY "smb.path", "smb.file"
         LIMIT 10;
┌──────────────────────┬───────────────────────────┬─────────────────────────────┬──────────────┐
│ hex("smb.nt_status") │         smb.path          │          smb.file           │ count_star() │
│       varchar        │          varchar          │           varchar           │    int64     │
├──────────────────────┼───────────────────────────┼─────────────────────────────┼──────────────┤
│ 0                    │ \\\\192.168.114.129\\TEST │ \\mkdirtest                 │            1 │
│ C0000103             │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir      │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir      │            3 │
│ C0000034             │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir      │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath                │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt            │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt\\V S       │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt\\V S\\VB98 │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawcontext                │            3 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawioctl                  │            1 │
└──────────────────────┴───────────────────────────┴─────────────────────────────┴──────────────┘
  10 rows                                                                             4 columns
```



## 0x03 (SMB_COM_CREATE)
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x03" -T fields -e ip.src -e ip.dst -e smb.cmd -e smb.nt_status -e smb.flags.response -e smb.path -e smb.tid -e smb.uid -e smb.mid -e smb.file_attribute -e smb.create.time -e smb.file -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x03.csv
```
```sql
memory D SELECT hex(B."smb.nt_status"), A."smb.path", A."smb.file_attribute", A."smb.create.time", A."smb.file", COUNT(*)
         FROM read_csv_auto('0x03.csv') AS A
         LEFT JOIN read_csv_auto('0x03.csv') AS B
         ON B."smb.flags.response" = true AND A."smb.tid" = B."smb.tid" AND A."smb.uid" = B."smb.uid" AND A."smb.mid" = B."smb.mid"
         WHERE A."ip.src" = '192.168.114.1' AND A."ip.dst" = '192.168.114.129'
         AND A."smb.flags.response" = false
         GROUP BY ALL
         ORDER BY A."smb.path", A."smb.file";
┌────────────────────────┬───────────────────────────┬────────────────────┬──────────────────────────┬───────────────────────────────┬──────────────┐
│ hex(B."smb.nt_status") │         smb.path          │ smb.file_attribute │     smb.create.time      │           smb.file            │ count_star() │
│        varchar         │          varchar          │       int64        │ timestamp with time zone │            varchar            │    int64     │
├────────────────────────┼───────────────────────────┼────────────────────┼──────────────────────────┼───────────────────────────────┼──────────────┤
│ 0                      │ \\\\192.168.114.129\\TEST │                  0 │ 2005-11-22 14:32:18+09   │ \\rawopen\\torture_create.txt │            1 │
│ 0                      │ \\\\192.168.114.129\\TEST │                  2 │ 2005-11-22 14:32:18+09   │ \\rawopen\\torture_create.txt │            1 │
│ 0                      │ \\\\192.168.114.129\\TEST │                  0 │ 1970-01-01 09:00:00+09   │ \\rawopen\\torture_create.txt │            2 │
└────────────────────────┴───────────────────────────┴────────────────────┴──────────────────────────┴───────────────────────────────┴──────────────┘
```



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
