## 0x00
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/e6e870ad-7037-4b79-ac54-4a42a1ba4561
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x00" -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.cmd -e smb.flags.response -e smb.signature -e smb.uid -e smb.mid -e smb.error_class -e smb.error_code -e smb.nt_status -e smb.path -e smb.dir_name -e smb.file -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x00.csv
```
```sql
memory D SELECT timezone('UTC', "frame.time_utc") AS time_utc, "ip.src", "ip.dst", "smb.cmd", "smb.flags.response", "smb.signature", "smb.uid", "smb.mid", hex("smb.error_class"), hex("smb.error_code"), hex("smb.nt_status"), "smb.path", "smb.dir_name", "smb.file", COUNT(*)
         FROM read_csv_auto('0x00.csv')
         GROUP BY ALL
         ORDER BY time_utc, "smb.uid", "smb.mid"
         LIMIT 10;
┌────────────────────────────┬─────────────────┬─────────────────┬─────────┬────────────────────┬──────────────────┬─────────┬─────────┬────────────────────────┬───────────────────────┬──────────────────────┬───────────────────────────┬─────────────────┬─────────────────┬──────────────┐
│          time_utc          │     ip.src      │     ip.dst      │ smb.cmd │ smb.flags.response │  smb.signature   │ smb.uid │ smb.mid │ hex("smb.error_class") │ hex("smb.error_code") │ hex("smb.nt_status") │         smb.path          │  smb.dir_name   │    smb.file     │ count_star() │
│         timestamp          │     varchar     │     varchar     │  int64  │      boolean       │     varchar      │  int64  │  int64  │        varchar         │        varchar        │       varchar        │          varchar          │     varchar     │     varchar     │    int64     │
├────────────────────────────┼─────────────────┼─────────────────┼─────────┼────────────────────┼──────────────────┼─────────┼─────────┼────────────────────────┼───────────────────────┼──────────────────────┼───────────────────────────┼─────────────────┼─────────────────┼──────────────┤
│ 2005-11-19 03:31:58.791492 │ 192.168.114.1   │ 192.168.114.129 │       0 │ false              │ 0000000000000000 │    2048 │      10 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsfileinfo │ NULL            │            1 │
│ 2005-11-19 03:31:58.804865 │ 192.168.114.129 │ 192.168.114.1   │       0 │ true               │ 0000000000000000 │    2048 │      10 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL            │ \\testsfileinfo │            1 │
│ 2005-11-19 03:32:01.278123 │ 192.168.114.1   │ 192.168.114.129 │       0 │ false              │ 0000000000000000 │    2048 │      43 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsearch    │ NULL            │            1 │
│ 2005-11-19 03:32:01.283016 │ 192.168.114.129 │ 192.168.114.1   │       0 │ true               │ 0000000000000000 │    2048 │      43 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL            │ \\testsearch    │            1 │
│ 2005-11-19 03:32:04.401795 │ 192.168.114.1   │ 192.168.114.129 │       0 │ false              │ 0000000000000000 │    2048 │    1590 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsearch    │ NULL            │            1 │
│ 2005-11-19 03:32:04.402836 │ 192.168.114.129 │ 192.168.114.1   │       0 │ true               │ 0000000000000000 │    2048 │    1590 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL            │ \\testsearch    │            1 │
│ 2005-11-19 03:32:07.665328 │ 192.168.114.1   │ 192.168.114.129 │       0 │ false              │ 0000000000000000 │    2048 │    3006 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsearch    │ NULL            │            1 │
│ 2005-11-19 03:32:07.667446 │ 192.168.114.129 │ 192.168.114.1   │       0 │ true               │ 0000000000000000 │    2048 │    3006 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL            │ \\testsearch    │            1 │
│ 2005-11-19 03:32:07.95141  │ 192.168.114.1   │ 192.168.114.129 │       0 │ false              │ 0000000000000000 │    2048 │    3071 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsearch    │ NULL            │            1 │
│ 2005-11-19 03:32:07.953721 │ 192.168.114.129 │ 192.168.114.1   │       0 │ true               │ 0000000000000000 │    2048 │    3071 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL            │ \\testsearch    │            1 │
└────────────────────────────┴─────────────────┴─────────────────┴─────────┴────────────────────┴──────────────────┴─────────┴─────────┴────────────────────────┴───────────────────────┴──────────────────────┴───────────────────────────┴─────────────────┴─────────────────┴──────────────┘
  10 rows                                                                                                                                                                                                                                                                          15 columns
```



## 0x01
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/0bca354c-42d9-46b7-a0ae-d8c6870242ca
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x01" -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.cmd -e smb.flags.response -e smb.signature -e smb.uid -e smb.mid -e smb.error_class -e smb.error_code -e smb.nt_status -e smb.path -e smb.dir_name -e smb.file -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x01.csv
```
```sql
memory D SELECT timezone('UTC', "frame.time_utc") AS time_utc, "ip.src", "ip.dst", "smb.cmd", "smb.flags.response", "smb.signature", "smb.uid", "smb.mid", hex("smb.error_class"), hex("smb.error_code"), hex("smb.nt_status"), "smb.path", "smb.dir_name", "smb.file", COUNT(*)
         FROM read_csv_auto('0x01.csv')
         GROUP BY ALL
         ORDER BY time_utc, "smb.uid", "smb.mid"
         LIMIT 10;
┌────────────────────────────┬─────────────────┬─────────────────┬─────────┬────────────────────┬──────────────────┬─────────┬─────────┬────────────────────────┬───────────────────────┬──────────────────────┬───────────────────────────┬──────────────────┬──────────────────┬──────────────┐
│          time_utc          │     ip.src      │     ip.dst      │ smb.cmd │ smb.flags.response │  smb.signature   │ smb.uid │ smb.mid │ hex("smb.error_class") │ hex("smb.error_code") │ hex("smb.nt_status") │         smb.path          │   smb.dir_name   │     smb.file     │ count_star() │
│         timestamp          │     varchar     │     varchar     │  int64  │      boolean       │     varchar      │  int64  │  int64  │        varchar         │        varchar        │       varchar        │          varchar          │     varchar      │     varchar      │    int64     │
├────────────────────────────┼─────────────────┼─────────────────┼─────────┼────────────────────┼──────────────────┼─────────┼─────────┼────────────────────────┼───────────────────────┼──────────────────────┼───────────────────────────┼──────────────────┼──────────────────┼──────────────┤
│ 2005-11-19 03:31:58.788975 │ 192.168.114.1   │ 192.168.114.129 │       1 │ false              │ 0000000000000000 │    2048 │       9 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsfileinfo  │ NULL             │            1 │
│ 2005-11-19 03:31:58.790701 │ 192.168.114.129 │ 192.168.114.1   │       1 │ true               │ 0000000000000000 │    2048 │       9 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL             │ \\testsfileinfo  │            1 │
│ 2005-11-19 03:32:04.393642 │ 192.168.114.1   │ 192.168.114.129 │       1 │ false              │ 0000000000000000 │    2048 │    1587 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsearch     │ NULL             │            1 │
│ 2005-11-19 03:32:04.400712 │ 192.168.114.129 │ 192.168.114.1   │       1 │ true               │ 0000000000000000 │    2048 │    1587 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL             │ \\testsearch     │            1 │
│ 2005-11-19 03:32:07.627161 │ 192.168.114.1   │ 192.168.114.129 │       1 │ false              │ 0000000000000000 │    2048 │    3003 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsearch     │ NULL             │            1 │
│ 2005-11-19 03:32:07.634918 │ 192.168.114.129 │ 192.168.114.1   │       1 │ true               │ 0000000000000000 │    2048 │    3003 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL             │ \\testsearch     │            1 │
│ 2005-11-19 03:32:07.944859 │ 192.168.114.1   │ 192.168.114.129 │       1 │ false              │ 0000000000000000 │    2048 │    3068 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsearch     │ NULL             │            1 │
│ 2005-11-19 03:32:07.950285 │ 192.168.114.129 │ 192.168.114.1   │       1 │ true               │ 0000000000000000 │    2048 │    3068 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL             │ \\testsearch     │            1 │
│ 2005-11-19 03:32:08.991434 │ 192.168.114.1   │ 192.168.114.129 │       1 │ false              │ 0000000000000000 │    2048 │    3979 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ \\testsearch\\d0 │ NULL             │            1 │
│ 2005-11-19 03:32:08.991983 │ 192.168.114.129 │ 192.168.114.1   │       1 │ true               │ 0000000000000000 │    2048 │    3979 │ NULL                   │ NULL                  │ 0                    │ \\\\192.168.114.129\\TEST │ NULL             │ \\testsearch\\d0 │            1 │
└────────────────────────────┴─────────────────┴─────────────────┴─────────┴────────────────────┴──────────────────┴─────────┴─────────┴────────────────────────┴───────────────────────┴──────────────────────┴───────────────────────────┴──────────────────┴──────────────────┴──────────────┘
  10 rows                                                                                                                                                                                                                                                                            15 columns
```
