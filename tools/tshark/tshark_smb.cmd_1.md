## 0x00 (SMB_COM_CREATE_DIRECTORY)
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x00" -T fields -e ip.src -e ip.dst -e smb.cmd -e smb.flags.response -e smb.nt_status -e smb.path -e smb.dir_name -e smb.file -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x00.csv
```
```sql
memory D SELECT "smb.path", "smb.file", COUNT(*)
         FROM read_csv_auto('0x00.csv')
         WHERE "ip.src" = '192.168.114.129' AND "ip.dst" = '192.168.114.1'
         AND "smb.cmd" = '0x00' AND "smb.flags.response" = true AND "smb.nt_status" = '0'
         GROUP BY ALL
         ORDER BY ALL
         LIMIT 10;
┌───────────────────────────┬─────────────────────────────┬──────────────┐
│         smb.path          │          smb.file           │ count_star() │
│          varchar          │           varchar           │    int64     │
├───────────────────────────┼─────────────────────────────┼──────────────┤
│ \\\\192.168.114.129\\TEST │ \\composite                 │            1 │
│ \\\\192.168.114.129\\TEST │ \\mkdirtest                 │            1 │
│ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir      │            1 │
│ \\\\192.168.114.129\\TEST │ \\rawchkpath                │            1 │
│ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt            │            1 │
│ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt\\V S       │            1 │
│ \\\\192.168.114.129\\TEST │ \\rawchkpath\\nt\\V S\\VB98 │            1 │
│ \\\\192.168.114.129\\TEST │ \\rawcontext                │            4 │
│ \\\\192.168.114.129\\TEST │ \\rawioctl                  │            1 │
│ \\\\192.168.114.129\\TEST │ \\rawopen                   │            1 │
└───────────────────────────┴─────────────────────────────┴──────────────┘
  10 rows                                                      3 columns
```
```sql
memory D SELECT hex("smb.nt_status"), "smb.path", "smb.file", COUNT(*)
         FROM read_csv_auto('0x00.csv')
         WHERE "ip.src" = '192.168.114.129' AND "ip.dst" = '192.168.114.1'
         AND "smb.cmd" = '0x00' AND "smb.flags.response" = true AND "smb.nt_status" != '0'
         GROUP BY ALL
         ORDER BY ALL
         LIMIT 10;
┌──────────────────────┬───────────────────────────┬────────────────────────┬──────────────┐
│ hex("smb.nt_status") │         smb.path          │        smb.file        │ count_star() │
│       varchar        │          varchar          │        varchar         │    int64     │
├──────────────────────┼───────────────────────────┼────────────────────────┼──────────────┤
│ C0000035             │ \\\\192.168.114.129\\TEST │ \\mkdirtest\\mkdir.dir │            2 │
│ C000003B             │ \\\\192.168.114.129\\TEST │ ..\\..\\..             │            1 │
└──────────────────────┴───────────────────────────┴────────────────────────┴──────────────┘
```
