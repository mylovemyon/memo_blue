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
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 223" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x00",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "212"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "10"
          },
          "Create Directory Request (0x00)": {
            "smb.wct": "0",
            "smb.bcc": "31",
            "smb.buffer_format": "4",
            "smb.dir_name": "\\testsfileinfo"
          }
        }
      }
    }
  }
]
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

## 0x02
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/ec064de8-6538-401e-8c73-b37231c36f2b
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x02" -T fields -e frame.time_utc -e ip.src -e ip.dst -e smb.cmd -e smb.flags.response -e smb.signature -e smb.uid -e smb.mid -e smb.error_class -e smb.error_code -e smb.nt_status -e smb.file -e smb.access.sharing -e smb.access.mode -e smb.fid -e smb.last_write.time -e smb.file_size -E header=y -E separator="," -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x02.csv
```
```sql
memory D SELECT timezone('UTC', "frame.time_utc") AS time_utc, "ip.src", "ip.dst", "smb.cmd", "smb.flags.response", "smb.signature", "smb.uid", "smb.mid", hex("smb.error_class"), hex("smb.error_code"), hex("smb.nt_status"), "smb.file", "smb.access.sharing", "smb.access.mode", "smb.fid", "smb.last_write.time", "smb.file_size", COUNT(*)
         FROM read_csv_auto('0x02.csv')
         GROUP BY ALL
         ORDER BY time_utc;
┌────────────────────────────┬─────────────────┬─────────────────┬─────────┬────────────────────┬──────────────────┬─────────┬─────────┬────────────────────────┬───────────────────────┬──────────────────────┬─────────────────────────────┬────────────────────┬─────────────────┬─────────┬──────────────────────────┬───────────────┬──────────────┐
│          time_utc          │     ip.src      │     ip.dst      │ smb.cmd │ smb.flags.response │  smb.signature   │ smb.uid │ smb.mid │ hex("smb.error_class") │ hex("smb.error_code") │ hex("smb.nt_status") │          smb.file           │ smb.access.sharing │ smb.access.mode │ smb.fid │   smb.last_write.time    │ smb.file_size │ count_star() │
│         timestamp          │     varchar     │     varchar     │  int64  │      boolean       │     varchar      │  int64  │  int64  │        varchar         │        varchar        │       varchar        │           varchar           │       int64        │      int64      │  int64  │ timestamp with time zone │     int64     │    int64     │
├────────────────────────────┼─────────────────┼─────────────────┼─────────┼────────────────────┼──────────────────┼─────────┼─────────┼────────────────────────┼───────────────────────┼──────────────────────┼─────────────────────────────┼────────────────────┼─────────────────┼─────────┼──────────────────────────┼───────────────┼──────────────┤
│ 2005-11-19 03:32:14.673931 │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      20 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  7 │               7 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:14.675388 │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      20 │ NULL                   │ NULL                  │ C0000034             │ NULL                        │               NULL │            NULL │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:14.679815 │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      29 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  7 │               7 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:14.680764 │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      29 │ NULL                   │ NULL                  │ 0                    │ NULL                        │                  7 │               7 │   16387 │ 2006-02-17 14:32:14+09   │             7 │            1 │
│ 2005-11-19 03:32:14.70511  │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      32 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  7 │               7 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:14.705524 │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      32 │ NULL                   │ NULL                  │ 0                    │ NULL                        │                  7 │               7 │   16387 │ 2006-02-17 14:32:14+09   │             7 │            1 │
│ 2005-11-19 03:32:14.708386 │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      37 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  0 │               0 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:14.709424 │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      37 │ NULL                   │ NULL                  │ 0                    │ NULL                        │                  0 │               0 │   16388 │ 2005-11-19 14:32:16+09   │             7 │            1 │
│ 2005-11-19 03:32:14.710849 │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      41 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  0 │               1 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:14.71148  │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      41 │ NULL                   │ NULL                  │ 0                    │ NULL                        │                  0 │               1 │   16389 │ 2005-11-19 14:32:16+09   │             7 │            1 │
│ 2005-11-19 03:32:14.713179 │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      45 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  0 │               2 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:14.713483 │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      45 │ NULL                   │ NULL                  │ 0                    │ NULL                        │                  0 │               2 │   16390 │ 2005-11-19 14:32:16+09   │             7 │            1 │
│ 2005-11-19 03:32:14.715195 │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      49 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  2 │               2 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:14.715467 │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      49 │ NULL                   │ NULL                  │ 0                    │ NULL                        │                  2 │               2 │   16391 │ 2005-11-19 14:32:16+09   │             7 │            1 │
│ 2005-11-19 03:32:14.716214 │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      52 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  4 │               2 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:15.706766 │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      52 │ NULL                   │ NULL                  │ C0000043             │ NULL                        │               NULL │            NULL │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:15.707192 │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      53 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  4 │               0 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:15.707715 │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      53 │ NULL                   │ NULL                  │ 0                    │ NULL                        │                  4 │               0 │   16392 │ 2005-11-19 14:32:16+09   │             7 │            1 │
│ 2005-11-19 03:32:15.710867 │ 192.168.114.1   │ 192.168.114.129 │       2 │ false              │ 0000000000000000 │    2048 │      58 │ NULL                   │ NULL                  │ 0                    │ \\rawopen\\torture_open.txt │                  0 │               0 │    NULL │ NULL                     │          NULL │            1 │
│ 2005-11-19 03:32:15.711281 │ 192.168.114.129 │ 192.168.114.1   │       2 │ true               │ 0000000000000000 │    2048 │      58 │ NULL                   │ NULL                  │ 0                    │ NULL                        │                  0 │               0 │   16393 │ 2005-11-19 14:32:16+09   │             7 │            1 │
└────────────────────────────┴─────────────────┴─────────────────┴─────────┴────────────────────┴──────────────────┴─────────┴─────────┴────────────────────────┴───────────────────────┴──────────────────────┴─────────────────────────────┴────────────────────┴─────────────────┴─────────┴──────────────────────────┴───────────────┴──────────────┘
  20 rows                                                                                                                                                                                                                                                                                                                                    18 columns
```
