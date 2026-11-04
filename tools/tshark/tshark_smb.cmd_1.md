```
-e ip.src
-e ip.dst
-e smb.cmd
-e smb.flags.response
-e smb.nt_status
-e smb.path
-e smb.tid
-e smb.uid
-e smb.mid
-e smb.dir_name
-e smb.file
-e smb.file_attribute
-e smb.create.time
```

## 0x00 (SMB_COM_CREATE_DIRECTORY)
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x00" -T fields -e ip.src -e ip.dst -e smb.cmd -e smb.nt_status -e smb.flags.response -e smb.path -e smb.file -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x00.csv
```
```sql
memory D SELECT hex("smb.nt_status"), "smb.path", "smb.file", COUNT(*)
         FROM read_csv_auto('0x00.csv')
         WHERE "ip.src" = '192.168.114.129' AND "ip.dst" = '192.168.114.1'
         AND "smb.cmd" = '0x00' AND "smb.flags.response" = true
         GROUP BY ALL
         ORDER BY "smb.path", "smb.file"
         LIMIT 10;
┌──────────────────────┬───────────────────────────┬─────────────────────────────┬──────────────┐
│ hex("smb.nt_status") │         smb.path          │          smb.file           │ count_star() │
│       varchar        │          varchar          │           varchar           │    int64     │
├──────────────────────┼───────────────────────────┼─────────────────────────────┼──────────────┤
│ C000003B             │ \\\\192.168.114.129\\TEST │ ..\\..\\..                  │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\composite                 │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\mkdirtest                 │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir      │            1 │
│ C0000035             │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir      │            2 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath                │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt            │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt\\V S       │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt\\V S\\VB98 │            1 │
│ 0                    │ \\\\192.168.114.129\\TEST │ \\rawcontext                │            4 │
└──────────────────────┴───────────────────────────┴─────────────────────────────┴──────────────┘
  10 rows                                                                             4 columns
```



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
