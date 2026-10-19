```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f security.json C:\Windows\System32\winevt\Logs\Security.evtx
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off"
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
memory D
```



## Computer
```sql
memory D SELECT Event.System.Computer, COUNT(*)
         FROM read_json('.\security.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌───────────────────────────┬──────────────┐
│         Computer          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ admin01.shieldbase.com    │       117595 │
│ dc01.shieldbase.com       │       823171 │
│ dev01.shieldbase.com      │         2125 │
│ elf01.shieldbase.com      │       899092 │
│ exchange01.shieldbase.com │       753154 │
│ file01.shieldbase.com     │        85011 │
│ ftp01                     │        32917 │
│ hunt01.shieldbase.com     │       140358 │
│ rd01.shieldbase.com       │        75333 │
│ rd02.shieldbase.com       │       127932 │
│ rd03.shieldbase.com       │       114829 │
│ rd04.shieldbase.com       │        97991 │
│ rd05.shieldbase.com       │       119049 │
│ rd06.shieldbase.com       │       101638 │
│ rd07.shieldbase.com       │       101079 │
│ rd08.shieldbase.com       │       131827 │
│ rd09.shieldbase.com       │        77059 │
│ rd10.shieldbase.com       │        86740 │
│ sql01.shieldbase.com      │        12712 │
│ wac01.shieldbase.com      │        18074 │
│ wkstn01.shieldbase.com    │        55704 │
│ wkstn02.shieldbase.com    │        82585 │
│ wkstn03.shieldbase.com    │        81738 │
│ wkstn04.shieldbase.com    │       110226 │
│ wkstn05.shieldbase.com    │        60512 │
│ wkstn06.shieldbase.com    │       102831 │
│ wkstn07.shieldbase.com    │        92470 │
│ wkstn08.shieldbase.com    │        64720 │
│ wkstn09.shieldbase.com    │        56311 │
│ wkstn10.shieldbase.com    │        73677 │
└───────────────────────────┴──────────────┘
  30 rows                        2 columns
```



## EventId
```sql
memory D SELECT Event.System.EventID, COUNT(*)
         FROM read_json('.\security.json', sample_size = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌──────────────────────────────────────────────┬──────────────┐
│                   EventID                    │ count_star() │
│                     json                     │    int64     │
├──────────────────────────────────────────────┼──────────────┤
│ 1100                                         │           88 │
│ 1101                                         │            2 │
│ 1102                                         │            1 │
│ 1108                                         │          439 │
│ 4608                                         │           90 │
│ 4610                                         │           90 │
│ 4611                                         │        67654 │
│ 4614                                         │          194 │
│ 4616                                         │          101 │
│ 4622                                         │          900 │
│ 4624                                         │      1182359 │
│ 4625                                         │        13258 │
│ 4634                                         │      1047980 │
│ 4647                                         │          139 │
│ 4648                                         │        82733 │
│ 4662                                         │          297 │
│ 4672                                         │       407910 │
│ 4688                                         │        41327 │
│ 4692                                         │           11 │
│ 4693                                         │            9 │
│ 4694                                         │           26 │
│ 4695                                         │          318 │
│ 4696                                         │           90 │
│ 4697                                         │         2818 │
│ 4724                                         │           24 │
│ 4725                                         │           18 │
│ 4726                                         │            7 │
│ 4729                                         │            7 │
│ 4732                                         │            1 │
│ 4733                                         │           13 │
│ 4738                                         │           42 │
│ 4755                                         │            1 │
│ 4756                                         │            3 │
│ 4757                                         │            2 │
│ 4768                                         │        11962 │
│ 4769                                         │        27488 │
│ 4770                                         │         2624 │
│ 4771                                         │            4 │
│ 4776                                         │        45709 │
│ 4778                                         │           99 │
│ 4779                                         │          109 │
│ 4793                                         │           22 │
│ 4797                                         │          207 │
│ 4798                                         │         2188 │
│ 4799                                         │        34190 │
│ 4800                                         │          109 │
│ 4801                                         │           16 │
│ 4826                                         │           90 │
│ 4902                                         │           90 │
│ 4904                                         │          238 │
│ 4905                                         │          238 │
│ 4907                                         │            5 │
│ 4944                                         │           90 │
│ 4945                                         │        19028 │
│ 4946                                         │         4278 │
│ 4947                                         │          750 │
│ 4948                                         │         2093 │
│ 4954                                         │          304 │
│ 4956                                         │           90 │
│ 5061                                         │          533 │
│ 5136                                         │           63 │
│ 5137                                         │            1 │
│ 5140                                         │        83128 │
│ 5142                                         │          375 │
│ 5144                                         │           81 │
│ 5379                                         │      1611505 │
│ 5381                                         │           12 │
│ 5382                                         │         1653 │
│ 5478                                         │           90 │
│ 6281                                         │            2 │
│ {"#attributes":{"Qualifiers":0},"#text":521} │           44 │
└──────────────────────────────────────────────┴──────────────┘
  71 rows                                           2 columns
```



## 1100 (The event logging service has shut down)
```sql
security-rd01 D SELECT Event.System.Computer, Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 1100
                GROUP BY ALL
                ORDER BY ALL;
┌─────────────────────┬────────────────────────────┬──────────────┐
│      Computer       │         SystemTime         │ count_star() │
│       varchar       │         timestamp          │    int64     │
├─────────────────────┼────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-02 18:24:07.697244 │            1 │
│ rd01.shieldbase.com │ 2023-01-23 14:50:54.295957 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:18:51.764149 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:33:34.463867 │            1 │
└─────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '1100' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────┬──────────────┐
│ fullkey │ count_star() │
│ varchar │    int64     │
└─────────┴──────────────┘
          0 rows
```



## 1102 (Event log cleared)
```sql
security-dev01 D SELECT Event.System.Computer, Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
                 FROM read_json('C:/Users/SANSDFIR/security-dev01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                 WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                 AND Event.System.EventID = 1102
                 GROUP BY ALL
                 ORDER BY ALL;
┌──────────────────────┬────────────────────────────┬──────────────┐
│       Computer       │         SystemTime         │ count_star() │
│       varchar        │         timestamp          │    int64     │
├──────────────────────┼────────────────────────────┼──────────────┤
│ dev01.shieldbase.com │ 2023-01-25 14:39:34.199899 │            1 │
└──────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
security-dev01 D SELECT je.fullkey, COUNT(*)
                 FROM read_json_objects('C:/Users/SANSDFIR/security-dev01.json') AS e, json_tree(e.json) AS je
                 WHERE e.json->>'$.Event.System.EventID' = '1102' AND je.fullkey LIKE '$.Event.EventData.%'
                 GROUP BY je.fullkey
                 ORDER BY je.fullkey;
┌─────────┬──────────────┐
│ fullkey │ count_star() │
│ varchar │    int64     │
└─────────┴──────────────┘
          0 rows
```



## 4608 (Windows is starting up)
```sql
security-rd01 D SELECT Event.System.Computer, Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4608
                GROUP BY ALL
                ORDER BY ALL;
┌─────────────────────┬────────────────────────────┬──────────────┐
│      Computer       │         SystemTime         │ count_star() │
│       varchar       │         timestamp          │    int64     │
├─────────────────────┼────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-02 18:24:26.685938 │            1 │
│ rd01.shieldbase.com │ 2023-01-23 14:51:16.557653 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:19:06.817008 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:38:28.383932 │            1 │
└─────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4608' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────┬──────────────┐
│ fullkey │ count_star() │
│ varchar │    int64     │
└─────────┴──────────────┘
          0 rows
```




## 4611 (A trusted logon process has been registered with the Local Security Authority)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.LogonProcessName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4611
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.LogonProcessName;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬─────────────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │    LogonProcessName     │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │         varchar         │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼─────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ ConsentUI               │           34 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ CredProvConsent         │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ HTTP.SYS                │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ IKE                     │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ Schannel                │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ Secondary Logon Service │            2 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ Smb2Srv                 │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ UserManager             │           64 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ Winlogon                │           29 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴─────────────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4611' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.LogonProcessName  │          346 │
│ $.Event.EventData.SubjectDomainName │          346 │
│ $.Event.EventData.SubjectLogonId    │          346 │
│ $.Event.EventData.SubjectUserName   │          346 │
│ $.Event.EventData.SubjectUserSid    │          346 │
└─────────────────────────────────────┴──────────────┘
```



## 4616 (The system time was changed)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ProcessName, Event.EventData.PreviousTime, Event.EventData.NewTime, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4616
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.PreviousTime;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬─────────────────────────────────┬────────────────────────────┬────────────────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │           ProcessName           │        PreviousTime        │          NewTime           │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │             varchar             │         timestamp          │         timestamp          │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼─────────────────────────────────┼────────────────────────────┼────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-19       │ NT AUTHORITY      │ LOCAL SERVICE   │ C:\Windows\System32\svchost.exe │ 2023-01-02 18:24:07.586327 │ 2023-01-02 18:24:07.590471 │            1 │
│ rd01.shieldbase.com │ S-1-5-19       │ NT AUTHORITY      │ LOCAL SERVICE   │ C:\Windows\System32\svchost.exe │ 2023-01-23 14:50:54.138366 │ 2023-01-23 14:50:54.152033 │            1 │
│ rd01.shieldbase.com │ S-1-5-19       │ NT AUTHORITY      │ LOCAL SERVICE   │ C:\Windows\System32\svchost.exe │ 2023-01-23 14:51:51.381957 │ 2023-01-23 14:51:51.382336 │            1 │
│ rd01.shieldbase.com │ S-1-5-19       │ NT AUTHORITY      │ LOCAL SERVICE   │ C:\Windows\System32\svchost.exe │ 2023-01-25 14:18:51.681411 │ 2023-01-25 14:18:51.681409 │            1 │
│ rd01.shieldbase.com │ S-1-5-19       │ NT AUTHORITY      │ LOCAL SERVICE   │ C:\Windows\System32\svchost.exe │ 2023-01-25 14:33:34.369995 │ 2023-01-25 14:33:34.371775 │            1 │
│ rd01.shieldbase.com │ S-1-5-19       │ NT AUTHORITY      │ LOCAL SERVICE   │ C:\Windows\System32\svchost.exe │ 2023-01-25 14:39:21.612376 │ 2023-01-25 14:39:21.612623 │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴─────────────────────────────────┴────────────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4616' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.NewTime           │           24 │
│ $.Event.EventData.PreviousTime      │           24 │
│ $.Event.EventData.ProcessId         │           24 │
│ $.Event.EventData.ProcessName       │           24 │
│ $.Event.EventData.SubjectDomainName │           24 │
│ $.Event.EventData.SubjectLogonId    │           24 │
│ $.Event.EventData.SubjectUserName   │           24 │
│ $.Event.EventData.SubjectUserSid    │           24 │
└─────────────────────────────────────┴──────────────┘
```



## 4624 (Successful logon)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.LogonType, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TargetOutboundDomainName, Event.EventData.TargetOutboundUserName, Event.EventData.VirtualAccount, Event.EventData.ImpersonationLevel, Event.EventData.WorkstationName, Event.EventData.IpAddress, Event.EventData.ProcessName, Event.EventData.AuthenticationPackageName, Event.EventData.LogonProcessName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4624
                GROUP BY ALL
                ORDER BY Event.EventData.LogonType, Event.EventData.SubjectUserSid , Event.EventData.TargetUserSid, Event.EventData.IpAddress;
┌─────────────────────┬───────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬────────────────────────────────────────────────┬──────────────────┬─────────────────┬──────────────────────────┬────────────────────────┬────────────────┬────────────────────┬─────────────────┬───────────────────────────┬──────────────────────────────────────────────────────────────┬───────────────────────────┬──────────────────┬──────────────┐
│      Computer       │ LogonType │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │                 TargetUserSid                  │ TargetDomainName │ TargetUserName  │ TargetOutboundDomainName │ TargetOutboundUserName │ VirtualAccount │ ImpersonationLevel │ WorkstationName │         IpAddress         │                         ProcessName                          │ AuthenticationPackageName │ LogonProcessName │ count_star() │
│       varchar       │   int64   │                    varchar                     │      varchar      │     varchar     │                    varchar                     │     varchar      │     varchar     │         varchar          │        varchar         │    varchar     │      varchar       │     varchar     │          varchar          │                           varchar                            │          varchar          │     varchar      │    int64     │
├─────────────────────┼───────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────┼──────────────────┼─────────────────┼──────────────────────────┼────────────────────────┼────────────────┼────────────────────┼─────────────────┼───────────────────────────┼──────────────────────────────────────────────────────────────┼───────────────────────────┼──────────────────┼──────────────┤
│ rd01.shieldbase.com │         0 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-18                                       │ NT AUTHORITY     │ SYSTEM          │ -                        │ -                      │ %%1843         │ -                  │ -               │ -                         │                                                              │ -                         │ -                │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-1                                   │ Window Manager   │ DWM-1           │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            8 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-10                                  │ Window Manager   │ DWM-10          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-11                                  │ Window Manager   │ DWM-11          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            8 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-12                                  │ Window Manager   │ DWM-12          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-13                                  │ Window Manager   │ DWM-13          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-14                                  │ Window Manager   │ DWM-14          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │           20 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-15                                  │ Window Manager   │ DWM-15          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │           10 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-2                                   │ Window Manager   │ DWM-2           │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            8 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-3                                   │ Window Manager   │ DWM-3           │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-4                                   │ Window Manager   │ DWM-4           │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-5                                   │ Window Manager   │ DWM-5           │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-6                                   │ Window Manager   │ DWM-6           │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-7                                   │ Window Manager   │ DWM-7           │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-8                                   │ Window Manager   │ DWM-8           │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-9                                   │ Window Manager   │ DWM-9           │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-0                                   │ Font Driver Host │ UMFD-0          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\wininit.exe                              │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-1                                   │ Font Driver Host │ UMFD-1          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-10                                  │ Font Driver Host │ UMFD-10         │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-11                                  │ Font Driver Host │ UMFD-11         │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-12                                  │ Font Driver Host │ UMFD-12         │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-13                                  │ Font Driver Host │ UMFD-13         │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-14                                  │ Font Driver Host │ UMFD-14         │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │           10 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-15                                  │ Font Driver Host │ UMFD-15         │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            5 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-2                                   │ Font Driver Host │ UMFD-2          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-3                                   │ Font Driver Host │ UMFD-3          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-4                                   │ Font Driver Host │ UMFD-4          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-5                                   │ Font Driver Host │ UMFD-5          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-6                                   │ Font Driver Host │ UMFD-6          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-7                                   │ Font Driver Host │ UMFD-7          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-8                                   │ Font Driver Host │ UMFD-8          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-9                                   │ Font Driver Host │ UMFD-9          │ -                        │ -                      │ %%1842         │ %%1833             │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-18                                       │ SHIELDBASE.COM   │ RD01$           │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ -                         │ -                                                            │ Kerberos                  │ Kerberos         │           81 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-18                                       │ SHIELDBASE.COM   │ RD01$           │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ ::1                       │ -                                                            │ Kerberos                  │ Kerberos         │          736 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ SHIELDBASE.COM   │ cbarton-a       │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ 172.16.5.25               │ -                                                            │ Kerberos                  │ Kerberos         │           26 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a        │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ 172.16.4.4                │ -                                                            │ Kerberos                  │ Kerberos         │           25 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a        │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ 172.16.4.7                │ -                                                            │ Kerberos                  │ Kerberos         │           48 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a        │ -                        │ -                      │ %%1843         │ %%1833             │ WAC01           │ 172.16.4.7                │ -                                                            │ NTLM                      │ NtLmSsp          │            4 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a        │ -                        │ -                      │ %%1843         │ %%1833             │ RD08            │ 172.16.6.18               │ -                                                            │ NTLM                      │ NtLmSsp          │            3 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a        │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ 172.16.6.18               │ -                                                            │ Kerberos                  │ Kerberos         │           82 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ DUNGANATOR      │ 172.16.30.14              │ -                                                            │ NTLM                      │ NtLmSsp          │            6 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ DUNGANATOR      │ 172.16.30.20              │ -                                                            │ NTLM                      │ NtLmSsp          │           14 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ DUNGANATOR      │ 172.16.30.23              │ -                                                            │ NTLM                      │ NtLmSsp          │            5 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ DUNGANATOR      │ 172.16.30.3               │ -                                                            │ NTLM                      │ NtLmSsp          │            8 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ DUNGANATOR      │ 172.16.30.8               │ -                                                            │ NTLM                      │ NtLmSsp          │           14 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1159 │ SHIELDBASE.COM   │ HUNT01$         │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ 172.16.5.25               │ -                                                            │ Kerberos                  │ Kerberos         │            2 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ SHIELDBASE.COM   │ slevine         │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ 172.16.6.18               │ -                                                            │ Kerberos                  │ Kerberos         │            1 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ shieldbase       │ DEV01$          │ -                        │ -                      │ %%1843         │ %%1833             │ DEV01           │ 172.16.4.9                │ -                                                            │ NTLM                      │ NtLmSsp          │            1 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ SHIELDBASE.COM   │ DEV01$          │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ 172.16.4.9                │ -                                                            │ Kerberos                  │ Kerberos         │            3 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc          │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ 172.16.6.18               │ -                                                            │ Kerberos                  │ Kerberos         │            1 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc          │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ fe80::7e6b:763c:b405:22b4 │ -                                                            │ Kerberos                  │ Kerberos         │            1 │
│ rd01.shieldbase.com │         5 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-18                                       │ NT AUTHORITY     │ SYSTEM          │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ -                         │ C:\Windows\System32\services.exe                             │ Negotiate                 │ Advapi           │         4217 │
│ rd01.shieldbase.com │         5 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-19                                       │ NT AUTHORITY     │ LOCAL SERVICE   │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ -                         │ C:\Windows\System32\services.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         5 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-20                                       │ NT AUTHORITY     │ NETWORK SERVICE │ -                        │ -                      │ %%1843         │ %%1833             │ -               │ -                         │ C:\Windows\System32\services.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ shieldbase               │ wacsvc                 │ %%1843         │ %%1833             │ -               │ -                         │ C:\Windows\explorer.exe                                      │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ shieldbase               │ wacsvc                 │ %%1843         │ %%1833             │ -               │ -                         │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ shieldbase               │ wacsvc                 │ %%1843         │ %%1833             │ -               │ ::1                       │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ seclogo          │           17 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ RD01            │ 172.16.30.14              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            3 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ RD01            │ 172.16.30.20              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            5 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ RD01            │ 172.16.30.23              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            2 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ RD01            │ 172.16.30.3               │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            4 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ RD01            │ 172.16.30.8               │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            7 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc          │ -                        │ -                      │ %%1843         │ %%1833             │ RD01            │ 172.16.4.9                │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            2 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc          │ -                        │ -                      │ %%1843         │ %%1833             │ RD01            │ 172.16.6.18               │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │           12 │
│ rd01.shieldbase.com │        11 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ RD01            │ ::1                       │ C:\Windows\System32\consent.exe                              │ Negotiate                 │ CredPro          │            1 │
│ rd01.shieldbase.com │        12 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ %%1843         │ %%1833             │ RD01            │ 172.16.30.20              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            1 │
└─────────────────────┴───────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴────────────────────────────────────────────────┴──────────────────┴─────────────────┴──────────────────────────┴────────────────────────┴────────────────┴────────────────────┴─────────────────┴───────────────────────────┴──────────────────────────────────────────────────────────────┴───────────────────────────┴──────────────────┴──────────────┘
  66 rows                                                                                                                                                                                                                                                                                                                                                                                                                                                                18 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4624' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────────────┬──────────────┐
│                   fullkey                   │ count_star() │
│                   varchar                   │    int64     │
├─────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AuthenticationPackageName │        17544 │
│ $.Event.EventData.ElevatedToken             │        17544 │
│ $.Event.EventData.ImpersonationLevel        │        17544 │
│ $.Event.EventData.IpAddress                 │        17544 │
│ $.Event.EventData.IpPort                    │        17544 │
│ $.Event.EventData.KeyLength                 │        17544 │
│ $.Event.EventData.LmPackageName             │        17544 │
│ $.Event.EventData.LogonGuid                 │        17544 │
│ $.Event.EventData.LogonProcessName          │        17544 │
│ $.Event.EventData.LogonType                 │        17544 │
│ $.Event.EventData.ProcessId                 │        17544 │
│ $.Event.EventData.ProcessName               │        17544 │
│ $.Event.EventData.RestrictedAdminMode       │        17544 │
│ $.Event.EventData.SubjectDomainName         │        17544 │
│ $.Event.EventData.SubjectLogonId            │        17544 │
│ $.Event.EventData.SubjectUserName           │        17544 │
│ $.Event.EventData.SubjectUserSid            │        17544 │
│ $.Event.EventData.TargetDomainName          │        17544 │
│ $.Event.EventData.TargetLinkedLogonId       │        17544 │
│ $.Event.EventData.TargetLogonId             │        17544 │
│ $.Event.EventData.TargetOutboundDomainName  │        17544 │
│ $.Event.EventData.TargetOutboundUserName    │        17544 │
│ $.Event.EventData.TargetUserName            │        17544 │
│ $.Event.EventData.TargetUserSid             │        17544 │
│ $.Event.EventData.TransmittedServices       │        17544 │
│ $.Event.EventData.VirtualAccount            │        17544 │
│ $.Event.EventData.WorkstationName           │        17544 │
└─────────────────────────────────────────────┴──────────────┘
  27 rows                                          2 columns
```



## 4625 (Failed logon)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.LogonType, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.WorkstationName, Event.EventData.IpAddress, Event.EventData.ProcessName, Event.EventData.AuthenticationPackageName, Event.EventData.LogonProcessName, Event.EventData.Status, Event.EventData.SubStatus, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4625
                GROUP BY ALL
                ORDER BY Event.EventData.LogonType, Event.EventData.SubjectUserSid , Event.EventData.TargetUserSid, Event.EventData.IpAddress;
┌─────────────────────┬───────────┬────────────────┬───────────────────┬─────────────────┬───────────────┬──────────────────┬────────────────┬─────────────────┬──────────────┬─────────────────────────────────┬───────────────────────────────────────┬──────────────────┬────────────┬────────────┬──────────────┐
│      Computer       │ LogonType │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │ TargetUserSid │ TargetDomainName │ TargetUserName │ WorkstationName │  IpAddress   │           ProcessName           │       AuthenticationPackageName       │ LogonProcessName │   Status   │ SubStatus  │ count_star() │
│       varchar       │   int64   │    varchar     │      varchar      │     varchar     │    varchar    │     varchar      │    varchar     │     varchar     │   varchar    │             varchar             │                varchar                │     varchar      │  varchar   │  varchar   │    int64     │
├─────────────────────┼───────────┼────────────────┼───────────────────┼─────────────────┼───────────────┼──────────────────┼────────────────┼─────────────────┼──────────────┼─────────────────────────────────┼───────────────────────────────────────┼──────────────────┼────────────┼────────────┼──────────────┤
│ rd01.shieldbase.com │         3 │ S-1-0-0        │ -                 │ -               │ S-1-0-0       │ shieldbase       │ tdungan        │ DUNGANATOR      │ 172.16.30.20 │ -                               │ NTLM                                  │ NtLmSsp          │ 0xc000006d │ 0xc000006a │            2 │
│ rd01.shieldbase.com │         3 │ S-1-0-0        │ -                 │ -               │ S-1-0-0       │ shieldbase       │ tdungan        │ DUNGANATOR      │ 172.16.30.20 │ -                               │ NTLM                                  │ NtLmSsp          │ 0xc000005e │ 0x0        │            2 │
│ rd01.shieldbase.com │         3 │ S-1-5-20       │ shieldbase        │ RD01$           │ S-1-0-0       │ shieldbase       │ tdungan        │ RD01            │ -            │ C:\Windows\System32\svchost.exe │ Negotiate                             │ Advapi           │ 0xc000005e │ 0x0        │            1 │
│ rd01.shieldbase.com │         3 │ S-1-5-20       │ shieldbase        │ RD01$           │ S-1-0-0       │                  │ sprx           │ RD01            │ -            │ C:\Windows\System32\svchost.exe │ MICROSOFT_AUTHENTICATION_PACKAGE_V1_0 │ Advapi           │ 0xc000006d │ 0xc0000064 │           14 │
│ rd01.shieldbase.com │        10 │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-0-0       │ shieldbase       │ tdungan        │ RD01            │ 172.16.30.20 │ C:\Windows\System32\svchost.exe │ Negotiate                             │ User32           │ 0xc000006d │ 0xc000006a │            1 │
└─────────────────────┴───────────┴────────────────┴───────────────────┴─────────────────┴───────────────┴──────────────────┴────────────────┴─────────────────┴──────────────┴─────────────────────────────────┴───────────────────────────────────────┴──────────────────┴────────────┴────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4625' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────────────┬──────────────┐
│                   fullkey                   │ count_star() │
│                   varchar                   │    int64     │
├─────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AuthenticationPackageName │           25 │
│ $.Event.EventData.FailureReason             │           25 │
│ $.Event.EventData.IpAddress                 │           25 │
│ $.Event.EventData.IpPort                    │           25 │
│ $.Event.EventData.KeyLength                 │           25 │
│ $.Event.EventData.LmPackageName             │           25 │
│ $.Event.EventData.LogonProcessName          │           25 │
│ $.Event.EventData.LogonType                 │           25 │
│ $.Event.EventData.ProcessId                 │           25 │
│ $.Event.EventData.ProcessName               │           25 │
│ $.Event.EventData.Status                    │           25 │
│ $.Event.EventData.SubStatus                 │           25 │
│ $.Event.EventData.SubjectDomainName         │           25 │
│ $.Event.EventData.SubjectLogonId            │           25 │
│ $.Event.EventData.SubjectUserName           │           25 │
│ $.Event.EventData.SubjectUserSid            │           25 │
│ $.Event.EventData.TargetDomainName          │           25 │
│ $.Event.EventData.TargetUserName            │           25 │
│ $.Event.EventData.TargetUserSid             │           25 │
│ $.Event.EventData.TransmittedServices       │           25 │
│ $.Event.EventData.WorkstationName           │           25 │
└─────────────────────────────────────────────┴──────────────┘
  21 rows                                          2 columns
  ```
status
```
0xC000005E: there are currently no logon servers available to service the logon request
0xC0000064: user name does not exist
0xC000006A: user name is correct but the password is wrong
0xC000006D: the cause is either a bad username or authentication information
0xC000006E: referenced user name and authentication information are valid, but some user account restriction has prevented successful authentication (such as time-of-day restrictions)
0xC000006F: user logon outside authorized hours
0xC0000070: user logon from unauthorized workstation
0xC0000071: user logon with expired password
0xC0000072: user logon to account disabled by administrator
0xC00000DC: indicates the Sam Server was in the wrong state to perform the desired operation
0xC0000133: clocks between DC and other computer too far out of sync
0xC000015B: the user has not been granted the requested logon type (also called the logon right) at this machine
0xC000018C: the logon request failed because the trust relationship between the primary domain and the trusted domain failed
0xC0000192: an attempt was made to logon, but the Netlogon service was not started.
0xC0000193: user logon with expired account
0xC0000224: user is required to change password at next logon
0xC0000225: evidently a bug in Windows and not a risk
0xC0000234: user is currently locked out
0xC00002EE: an error occurred during logon
0xC0000413: logon failure , the machine you are logging on to is protected by an authentication firewall, the specified account is not allowed to authenticate to the machine
0x0: Status OK
```
SubStatus
```
0xC000005E: there are currently no logon servers available to service the logon request
0xC0000064: user name does not exist
0xC000006A: user name is correct but the password is wrong
0xC000006D: the cause is either a bad username or authentication information
0xC000006E: referenced user name and authentication information are valid, but some user account restriction has prevented successful authentication (such as time-of-day restrictions)
0xC000006F: user logon outside authorized hours
0xC0000070: user logon from unauthorized workstation
0xC0000071: user logon with expired password
0xC0000072: user logon to account disabled by administrator
0xC00000DC: indicates the Sam Server was in the wrong state to perform the desired operation
0xC0000133: clocks between DC and other computer too far out of sync
0xC000015B: the user has not been granted the requested logon type (also called the logon right) at this machine
0xC000018C: the logon request failed because the trust relationship between the primary domain and the trusted domain failed
0xC0000192: an attempt was made to logon, but the Netlogon service was not started.
0xC0000193: user logon with expired account
0xC0000224: user is required to change password at next logon
0xC0000225: evidently a bug in Windows and not a risk
0xC0000234: user is currently locked out
0xC00002EE: an error occurred during logon
0xC0000413: logon failure , the machine you are logging on to is protected by an authentication firewall, the specified account is not allowed to authenticate to the machine
```



## 4634 (An account was logged off)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.LogonType, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4634
                GROUP BY ALL
                ORDER BY Event.EventData.LogonType, Event.EventData.TargetUserSid;
┌─────────────────────┬───────────┬────────────────────────────────────────────────┬──────────────────┬────────────────┬──────────────┐
│      Computer       │ LogonType │                 TargetUserSid                  │ TargetDomainName │ TargetUserName │ count_star() │
│       varchar       │   int64   │                    varchar                     │     varchar      │    varchar     │    int64     │
├─────────────────────┼───────────┼────────────────────────────────────────────────┼──────────────────┼────────────────┼──────────────┤
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-10                                  │ Window Manager   │ DWM-10         │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-11                                  │ Window Manager   │ DWM-11         │            6 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-12                                  │ Window Manager   │ DWM-12         │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-13                                  │ Window Manager   │ DWM-13         │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-14                                  │ Window Manager   │ DWM-14         │           20 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-15                                  │ Window Manager   │ DWM-15         │           10 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-2                                   │ Window Manager   │ DWM-2          │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-3                                   │ Window Manager   │ DWM-3          │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-4                                   │ Window Manager   │ DWM-4          │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-5                                   │ Window Manager   │ DWM-5          │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-6                                   │ Window Manager   │ DWM-6          │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-7                                   │ Window Manager   │ DWM-7          │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-8                                   │ Window Manager   │ DWM-8          │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-90-0-9                                   │ Window Manager   │ DWM-9          │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-10                                  │ Font Driver Host │ UMFD-10        │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-11                                  │ Font Driver Host │ UMFD-11        │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-12                                  │ Font Driver Host │ UMFD-12        │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-13                                  │ Font Driver Host │ UMFD-13        │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-14                                  │ Font Driver Host │ UMFD-14        │           10 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-15                                  │ Font Driver Host │ UMFD-15        │            5 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-2                                   │ Font Driver Host │ UMFD-2         │            3 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-3                                   │ Font Driver Host │ UMFD-3         │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-4                                   │ Font Driver Host │ UMFD-4         │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-5                                   │ Font Driver Host │ UMFD-5         │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-6                                   │ Font Driver Host │ UMFD-6         │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-7                                   │ Font Driver Host │ UMFD-7         │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-8                                   │ Font Driver Host │ UMFD-8         │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-96-0-9                                   │ Font Driver Host │ UMFD-9         │            1 │
│ rd01.shieldbase.com │         3 │ S-1-5-18                                       │ shieldbase       │ RD01$          │          817 │
│ rd01.shieldbase.com │         3 │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase       │ cbarton-a      │           26 │
│ rd01.shieldbase.com │         3 │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a       │          159 │
│ rd01.shieldbase.com │         3 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan        │           46 │
│ rd01.shieldbase.com │         3 │ S-1-5-21-2838623409-1327563992-2591358621-1159 │ shieldbase       │ HUNT01$        │            2 │
│ rd01.shieldbase.com │         3 │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ shieldbase       │ DEV01$         │            4 │
│ rd01.shieldbase.com │         3 │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc         │            1 │
│ rd01.shieldbase.com │         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan        │            9 │
│ rd01.shieldbase.com │        10 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan        │            5 │
│ rd01.shieldbase.com │        10 │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc         │           10 │
│ rd01.shieldbase.com │        11 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan        │            1 │
└─────────────────────┴───────────┴────────────────────────────────────────────────┴──────────────────┴────────────────┴──────────────┘
  39 rows                                                                                                                   6 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4634' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.LogonType        │         5024 │
│ $.Event.EventData.TargetDomainName │         5024 │
│ $.Event.EventData.TargetLogonId    │         5024 │
│ $.Event.EventData.TargetUserName   │         5024 │
│ $.Event.EventData.TargetUserSid    │         5024 │
└────────────────────────────────────┴──────────────┘
```



## 4647 (User initiated logoff)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4647
                GROUP BY ALL
                ORDER BY Event.EventData.TargetUserSid;
┌─────────────────────┬────────────────────────────────────────────────┬──────────────────┬────────────────┬──────────────┐
│      Computer       │                 TargetUserSid                  │ TargetDomainName │ TargetUserName │ count_star() │
│       varchar       │                    varchar                     │     varchar      │    varchar     │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼──────────────────┼────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan        │           16 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc         │            2 │
└─────────────────────┴────────────────────────────────────────────────┴──────────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4647' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.TargetDomainName │           43 │
│ $.Event.EventData.TargetLogonId    │           43 │
│ $.Event.EventData.TargetUserName   │           43 │
│ $.Event.EventData.TargetUserSid    │           43 │
└────────────────────────────────────┴──────────────┘
```



## 4648 (A logon was attempted using explicit credentials)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TargetServerName, Event.EventData.TargetInfo, Event.EventData.ProcessName, Event.EventData.IpAddress, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4648
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetUserName, Event.EventData.IpAddress;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬──────────────────┬────────────────┬────────────────────────┬──────────────────────────────┬───────────────────────────────────┬───────────────────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │ TargetDomainName │ TargetUserName │    TargetServerName    │          TargetInfo          │            ProcessName            │         IpAddress         │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │     varchar      │    varchar     │        varchar         │           varchar            │              varchar              │          varchar          │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-1          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-10         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-11         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-12         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-13         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-14         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │           10 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-15         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            5 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-2          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-3          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-4          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-5          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-6          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-7          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-8          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Window Manager   │ DWM-9          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ SHIELDBASE.COM   │ RD01$          │ rd01$                  │ rd01$                        │ C:\Windows\System32\taskhostw.exe │ -                         │           81 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-0         │ localhost              │ localhost                    │ C:\Windows\System32\wininit.exe   │ -                         │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-1         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-10        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-11        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-12        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-13        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-14        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │           10 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-15        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            5 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-2         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-3         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-4         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-5         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-6         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-7         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-8         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-9         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.14              │            3 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.20              │            6 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.23              │            2 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.3               │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.8               │            7 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\consent.exe   │ ::1                       │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ shieldbase       │ wacsvc         │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.4.9                │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ shieldbase       │ wacsvc         │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.6.18               │            6 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ shieldbase       │ wacsvc         │ file01.shieldbase.com  │ file01.shieldbase.com        │                                   │ 172.16.4.5                │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ dev01.shieldbase.com   │ cifs/dev01.shieldbase.com    │                                   │ 172.16.4.9                │            4 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ shieldbase       │ wacsvc         │ rd02.shieldbase.com    │ rd02.shieldbase.com          │                                   │ 172.16.6.12               │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ rd04.shieldbase.com    │ cifs/rd04.shieldbase.com     │                                   │ 172.16.6.14               │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ rd10.shieldbase.com    │ cifs/rd10.shieldbase.com     │                                   │ 172.16.6.20               │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ shieldbase       │ wacsvc         │ wkstn01.shieldbase.com │ wkstn01.shieldbase.com       │ C:\Windows\System32\wbem\WMIC.exe │ 172.16.7.11               │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ wkstn01.shieldbase.com │ RPCSS/wkstn01.shieldbase.com │ C:\Windows\System32\svchost.exe   │ 172.16.7.11               │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ wkstn01.shieldbase.com │ cifs/wkstn01.shieldbase.com  │                                   │ 172.16.7.11               │            4 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ wkstn01.shieldbase.com │ host/wkstn01.shieldbase.com  │ C:\Windows\System32\wbem\WMIC.exe │ 172.16.7.11               │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ rd01                   │ cifs/rd01                    │                                   │ fe80::7e6b:763c:b405:22b4 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ SHIELDBASE.COM   │ wacsvc         │ dev01.shieldbase.com   │ TERMSRV/dev01.shieldbase.com │ C:\Windows\System32\lsass.exe     │ -                         │            2 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴──────────────────┴────────────────┴────────────────────────┴──────────────────────────────┴───────────────────────────────────┴───────────────────────────┴──────────────┘
  51 rows                                                                                                                                                                                                                                                                    11 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4648' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.IpAddress         │         1315 │
│ $.Event.EventData.IpPort            │         1315 │
│ $.Event.EventData.LogonGuid         │         1315 │
│ $.Event.EventData.ProcessId         │         1315 │
│ $.Event.EventData.ProcessName       │         1315 │
│ $.Event.EventData.SubjectDomainName │         1315 │
│ $.Event.EventData.SubjectLogonId    │         1315 │
│ $.Event.EventData.SubjectUserName   │         1315 │
│ $.Event.EventData.SubjectUserSid    │         1315 │
│ $.Event.EventData.TargetDomainName  │         1315 │
│ $.Event.EventData.TargetInfo        │         1315 │
│ $.Event.EventData.TargetLogonGuid   │         1315 │
│ $.Event.EventData.TargetServerName  │         1315 │
│ $.Event.EventData.TargetUserName    │         1315 │
└─────────────────────────────────────┴──────────────┘
  14 rows                                  2 columns
```



## 4656 (A handle to an object was requested)



## 4657 (A registry value was modified)



## 4658 (The handle to an object was closed)



## 4661 (Handle requested to an object)



## 4662 (Operation performed on an object)



## 4663 (Attempt was made to access an object)



## 4672 (Administrative logon)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.PrivilegeList, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4672
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.PrivilegeList;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │                                                                                                                                                                                      PrivilegeList                                                                                                                                                                                      │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │                                                                                                                                                                                         varchar                                                                                                                                                                                         │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18                                       │ NT AUTHORITY      │ SYSTEM          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeTcbPrivilege\r\n\t\t\tSeSecurityPrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeSystemEnvironmentPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege │         4217 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ SeSecurityPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSeSystemEnvironmentPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege                                                                                          │          817 │
│ rd01.shieldbase.com │ S-1-5-19                                       │ NT AUTHORITY      │ LOCAL SERVICE   │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            4 │
│ rd01.shieldbase.com │ S-1-5-20                                       │ NT AUTHORITY      │ NETWORK SERVICE │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            4 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase        │ cbarton-a       │ SeSecurityPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSeSystemEnvironmentPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege                                                                                          │           26 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase        │ rsydow-a        │ SeSecurityPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSeSystemEnvironmentPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege                                                                                          │          162 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ SeSecurityPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSeSystemEnvironmentPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege                                                                                          │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ SeSecurityPrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSeSystemEnvironmentPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege                                                                                          │            7 │
│ rd01.shieldbase.com │ S-1-5-90-0-1                                   │ Window Manager    │ DWM-1           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            4 │
│ rd01.shieldbase.com │ S-1-5-90-0-1                                   │ Window Manager    │ DWM-1           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            4 │
│ rd01.shieldbase.com │ S-1-5-90-0-10                                  │ Window Manager    │ DWM-10          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-10                                  │ Window Manager    │ DWM-10          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-11                                  │ Window Manager    │ DWM-11          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            4 │
│ rd01.shieldbase.com │ S-1-5-90-0-11                                  │ Window Manager    │ DWM-11          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            4 │
│ rd01.shieldbase.com │ S-1-5-90-0-12                                  │ Window Manager    │ DWM-12          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-12                                  │ Window Manager    │ DWM-12          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-13                                  │ Window Manager    │ DWM-13          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-13                                  │ Window Manager    │ DWM-13          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-14                                  │ Window Manager    │ DWM-14          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │           10 │
│ rd01.shieldbase.com │ S-1-5-90-0-14                                  │ Window Manager    │ DWM-14          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │           10 │
│ rd01.shieldbase.com │ S-1-5-90-0-15                                  │ Window Manager    │ DWM-15          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            5 │
│ rd01.shieldbase.com │ S-1-5-90-0-15                                  │ Window Manager    │ DWM-15          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            5 │
│ rd01.shieldbase.com │ S-1-5-90-0-2                                   │ Window Manager    │ DWM-2           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            4 │
│ rd01.shieldbase.com │ S-1-5-90-0-2                                   │ Window Manager    │ DWM-2           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            4 │
│ rd01.shieldbase.com │ S-1-5-90-0-3                                   │ Window Manager    │ DWM-3           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            2 │
│ rd01.shieldbase.com │ S-1-5-90-0-3                                   │ Window Manager    │ DWM-3           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            2 │
│ rd01.shieldbase.com │ S-1-5-90-0-4                                   │ Window Manager    │ DWM-4           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            2 │
│ rd01.shieldbase.com │ S-1-5-90-0-4                                   │ Window Manager    │ DWM-4           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            2 │
│ rd01.shieldbase.com │ S-1-5-90-0-5                                   │ Window Manager    │ DWM-5           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            2 │
│ rd01.shieldbase.com │ S-1-5-90-0-5                                   │ Window Manager    │ DWM-5           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            2 │
│ rd01.shieldbase.com │ S-1-5-90-0-6                                   │ Window Manager    │ DWM-6           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-6                                   │ Window Manager    │ DWM-6           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-7                                   │ Window Manager    │ DWM-7           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-7                                   │ Window Manager    │ DWM-7           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-8                                   │ Window Manager    │ DWM-8           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-8                                   │ Window Manager    │ DWM-8           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-9                                   │ Window Manager    │ DWM-9           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                                                                                                                                                                                                                                                                 │            1 │
│ rd01.shieldbase.com │ S-1-5-90-0-9                                   │ Window Manager    │ DWM-9           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                                                                                                                                                                                                                                                                 │            1 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
  38 rows                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   6 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4672' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.PrivilegeList     │        17113 │
│ $.Event.EventData.SubjectDomainName │        17113 │
│ $.Event.EventData.SubjectLogonId    │        17113 │
│ $.Event.EventData.SubjectUserName   │        17113 │
│ $.Event.EventData.SubjectUserSid    │        17113 │
└─────────────────────────────────────┴──────────────┘
```



## 4673 (A privileged service was called)



## 4674 (An operation was attempted on a privileged object)



## 4688 (A new process has been created)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TokenElevationType, Event.EventData.MandatoryLabel, Event.EventData.ParentProcessName, Event.EventData.NewProcessName, Event.EventData.CommandLine, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4688
                GROUP BY ALL
                ORDER BY Event.EventData.ParentProcessName, Event.EventData.NewProcessName;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬───────────────┬──────────────────┬────────────────┬────────────────────┬────────────────┬─────────────────────────────────┬──────────────────────────────────┬─────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │ TargetUserSid │ TargetDomainName │ TargetUserName │ TokenElevationType │ MandatoryLabel │        ParentProcessName        │          NewProcessName          │ CommandLine │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │    varchar    │     varchar      │    varchar     │      varchar       │    varchar     │             varchar             │             varchar              │   varchar   │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼───────────────┼──────────────────┼────────────────┼────────────────────┼────────────────┼─────────────────────────────────┼──────────────────────────────────┼─────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │                                 │ C:\Windows\System32\smss.exe     │             │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │                                 │ Registry                         │             │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\autochk.exe  │             │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\csrss.exe    │             │            8 │
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\smss.exe     │             │            8 │
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\wininit.exe  │             │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\winlogon.exe │             │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\wininit.exe │ C:\Windows\System32\lsass.exe    │             │            4 │
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\wininit.exe │ C:\Windows\System32\services.exe │             │            4 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴───────────────┴──────────────────┴────────────────┴────────────────────┴────────────────┴─────────────────────────────────┴──────────────────────────────────┴─────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4688' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌──────────────────────────────────────┬──────────────┐
│               fullkey                │ count_star() │
│               varchar                │    int64     │
├──────────────────────────────────────┼──────────────┤
│ $.Event.EventData.CommandLine        │          287 │
│ $.Event.EventData.MandatoryLabel     │          287 │
│ $.Event.EventData.NewProcessId       │          287 │
│ $.Event.EventData.NewProcessName     │          287 │
│ $.Event.EventData.ParentProcessName  │          287 │
│ $.Event.EventData.ProcessId          │          287 │
│ $.Event.EventData.SubjectDomainName  │          287 │
│ $.Event.EventData.SubjectLogonId     │          287 │
│ $.Event.EventData.SubjectUserName    │          287 │
│ $.Event.EventData.SubjectUserSid     │          287 │
│ $.Event.EventData.TargetDomainName   │          287 │
│ $.Event.EventData.TargetLogonId      │          287 │
│ $.Event.EventData.TargetUserName     │          287 │
│ $.Event.EventData.TargetUserSid      │          287 │
│ $.Event.EventData.TokenElevationType │          287 │
└──────────────────────────────────────┴──────────────┘
  15 rows                                   2 columns
```
TokenElevationType
```
%%1936  - Type 1 is a full token with no privileges removed or groups disabled.  A full token is only used if User Account Control is disabled or if the user is the built-in Administrator account or a service account.
%%1937 - Type 2 is an elevated token with no privileges removed or groups disabled.  An elevated token is used when User Account Control is enabled and the user chooses to start the program using Run as administrator.  An elevated token is also used when an application is configured to always require administrative privilege or to always require maximum privilege, and the user is a member of the Administrators group.
%%1938 - Type 3 is the normal value when UAC is enabled and a user simply starts a program from the Start Menu.  It's a limited token with administrative privileges removed and administrative groups disabled.  The limited token is used when User Account Control is enabled, the application does not require administrative privilege, and the user does not choose to start the program using Run as administrator.
```
MandatoryLabel
```
S-1-16-0: SECURITY_MANDATORY_UNTRUSTED_RID
S-1-16-4096: SECURITY_MANDATORY_LOW_RID
S-1-16-8192: SECURITY_MANDATORY_MEDIUM_RID
S-1-16-8448: SECURITY_MANDATORY_MEDIUM_PLUS_RID
S-1-16-12288: SECURITY_MANDATORY_HIGH_RID
S-1-16-16384: SECURITY_MANDATORY_SYSTEM_RID
S-1-16-20480: SECURITY_MANDATORY_PROTECTED_PROCESS_RID
```



## 4689 (A process has exited)



## 4696 (A privileged service was called)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.ProcessName, Event.EventData.TargetProcessName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4696
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid , Event.EventData.TargetUserSid, Event.EventData.ProcessName, Event.EventData.TargetProcessName;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬───────────────┬──────────────────┬────────────────┬─────────────┬───────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │ TargetUserSid │ TargetDomainName │ TargetUserName │ ProcessName │ TargetProcessName │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │    varchar    │     varchar      │    varchar     │   varchar   │      varchar      │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼───────────────┼──────────────────┼────────────────┼─────────────┼───────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │             │ Registry          │            4 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴───────────────┴──────────────────┴────────────────┴─────────────┴───────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4696' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.ProcessId         │           26 │
│ $.Event.EventData.ProcessName       │           26 │
│ $.Event.EventData.SubjectDomainName │           26 │
│ $.Event.EventData.SubjectLogonId    │           26 │
│ $.Event.EventData.SubjectUserName   │           26 │
│ $.Event.EventData.SubjectUserSid    │           26 │
│ $.Event.EventData.TargetDomainName  │           26 │
│ $.Event.EventData.TargetLogonId     │           26 │
│ $.Event.EventData.TargetProcessId   │           26 │
│ $.Event.EventData.TargetProcessName │           26 │
│ $.Event.EventData.TargetUserName    │           26 │
│ $.Event.EventData.TargetUserSid     │           26 │
└─────────────────────────────────────┴──────────────┘
  12 rows                                  2 columns
```



## 4697 (A service was installed on the system)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ServiceAccount, Event.EventData.ServiceName, Event.EventData.ServiceFileName, Event.EventData.ServiceType, Event.EventData.ServiceStartType, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4697
                AND Event.EventData.ServiceFileName NOT ILIKE 'C:\Windows\system32\svchost.exe%'
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.ServiceFileName;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬────────────────┬─────────────────────────────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬─────────────┬──────────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │ ServiceAccount │                 ServiceName                 │                                                       ServiceFileName                                                       │ ServiceType │ ServiceStartType │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │    varchar     │                   varchar                   │                                                           varchar                                                           │   varchar   │      int64       │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────┼─────────────────────────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────┼──────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ Ec2Config                                   │ "C:\Program Files\Amazon\Ec2ConfigService\Ec2Config.exe"                                                                    │ 0x10        │                2 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ Velociraptor                                │ "C:\Program Files\Velociraptor\Velociraptor.exe"  --config "C:\Program Files\Velociraptor\/client.config.yaml" service run  │ 0x10        │                2 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ MpKsle2439143                               │ C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{1FEC7FC9-D47D-4480-85E5-AE4DD6CCA988}\MpKslDrv.sys            │ 0x1         │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ MpKsld6867d33                               │ C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{A0638AC4-7F42-4619-900A-6971E7AD116E}\MpKslDrv.sys            │ 0x1         │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_378e21a  │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_e7b07f1  │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_29395f8  │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_6c2a166  │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_26aa58fe │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_1e9ef1f6 │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_1555bf7a │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_1755cd8a │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_1e0f1442 │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_1f85cdd6 │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_140a51a7 │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_7d5e3    │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_2b14505  │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_21d5806c │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_1fd1a92  │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_27671121 │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_e7fae    │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_799cd    │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ CredentialEnrollmentManagerUserSvc_7147ed   │ C:\Windows\system32\CredentialEnrollmentManager.exe                                                                         │ 0xd0        │                3 │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ MpKsl80b1fd2a                               │ C:\Windows\system32\MpEngineStore\MpKslDrv.sys                                                                              │ 0x1         │                3 │            3 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ MpKsl3f390b95                               │ C:\Windows\system32\MpEngineStore\MpKslDrv.sys                                                                              │ 0x1         │                3 │            2 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ LocalSystem    │ mnemosyne                                   │ C:\windows/Mnemosyne.sys                                                                                                    │ 0x1         │                3 │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase        │ cbarton-a       │ LocalSystem    │ F-Response Subject Service                  │ "C:\windows\subject_srv.exe" -s "172.16.5.25:5682" -l 3262 -v "F-Response Subject Service" -k "155522845"                   │ 0x10        │                2 │            1 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴────────────────┴─────────────────────────────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴─────────────┴──────────────────┴──────────────┘
  27 rows                                                                                                                                                                                                                                                                                                                                      10 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4697' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────────┬──────────────┐
│                 fullkey                 │ count_star() │
│                 varchar                 │    int64     │
├─────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.ClientProcessId       │          955 │
│ $.Event.EventData.ClientProcessStartKey │          955 │
│ $.Event.EventData.ParentProcessId       │          955 │
│ $.Event.EventData.ServiceAccount        │          955 │
│ $.Event.EventData.ServiceFileName       │          955 │
│ $.Event.EventData.ServiceName           │          955 │
│ $.Event.EventData.ServiceStartType      │          955 │
│ $.Event.EventData.ServiceType           │          955 │
│ $.Event.EventData.SubjectDomainName     │          955 │
│ $.Event.EventData.SubjectLogonId        │          955 │
│ $.Event.EventData.SubjectUserName       │          955 │
│ $.Event.EventData.SubjectUserSid        │          955 │
└─────────────────────────────────────────┴──────────────┘
  12 rows                                      2 columns
```
ServiceType
```
0x1: Kernel Driver
0x2: File System Driver
0x8: Recognizer Driver
0x10: Win32 Own Process
0x20: Win32 Share Process
0x110: Interactive Own Process
0x120: Interactive Share Process
```
ServiceStartType
```
0: Boot
1: System
2: Automatic
3: Manual
4: Disabled
```



## 4698 (Scheduled Task created)



## 4699 (Scheduled Task deleted)



## 4700 (A scheduled task was enabled)



## 4701 (A scheduled task was disabled)



## 4702 (A scheduled task was updated)



## 4703 (A user right was adjusted)



## 4704 (A user right was assigned)



## 4705 (A user right was removed)



## 4706 (A new trust was created to a domain)



## 4707 (A trust to a domain was removed)



## 4713 (Kerberos policy was changed)



## 4716 (Trusted domain information was modified0)



## 4717 (System security access was granted to an account)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.AccessGranted, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.EventID = 4717
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────┬────────────────┬───────────────────┬─────────────────┬────────────────────────────────────────────────────────────────┬───────────────────────────────┬──────────────┐
│    Computer     │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                           TargetSid                            │         AccessGranted         │ count_star() │
│     varchar     │    varchar     │      varchar      │     varchar     │                            varchar                             │            varchar            │    int64     │
├─────────────────┼────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────┼───────────────────────────────┼──────────────┤
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-21-2908624845-2463485410-257172065-501                   │ SeDenyInteractiveLogonRight   │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-21-2908624845-2463485410-257172065-501                   │ SeDenyNetworkLogonRight       │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-21-2908624845-2463485410-257172065-501                   │ SeInteractiveLogonRight       │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-544                                                   │ SeRemoteInteractiveLogonRight │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-551                                                   │ SeNetworkLogonRight           │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-551                                                   │ SeBatchLogonRight             │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-555                                                   │ SeRemoteInteractiveLogonRight │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-559                                                   │ SeBatchLogonRight             │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-80-0                                                     │ SeServiceLogonRight           │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-80-3169285310-278349998-1452333686-3865143136-4212226833 │ SeServiceLogonRight           │            1 │
└─────────────────┴────────────────┴───────────────────┴─────────────────┴────────────────────────────────────────────────────────────────┴───────────────────────────────┴──────────────┘
  10 rows                                                                                                                                                                      7 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4717' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AccessGranted     │           10 │
│ $.Event.EventData.SubjectDomainName │           10 │
│ $.Event.EventData.SubjectLogonId    │           10 │
│ $.Event.EventData.SubjectUserName   │           10 │
│ $.Event.EventData.SubjectUserSid    │           10 │
│ $.Event.EventData.TargetSid         │           10 │
└─────────────────────────────────────┴──────────────┘
```



## 4718 (System security access was removed from an account)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.AccessRemoved, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.EventID = 4718
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────┬────────────────┬───────────────────┬─────────────────┬────────────────────────────────────────────────────────────────┬───────────────────────────────┬──────────────┐
│    Computer     │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                           TargetSid                            │         AccessRemoved         │ count_star() │
│     varchar     │    varchar     │      varchar      │     varchar     │                            varchar                             │            varchar            │    int64     │
├─────────────────┼────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────┼───────────────────────────────┼──────────────┤
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-1-0                                                        │ SeRemoteInteractiveLogonRight │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-1-0                                                        │ SeInteractiveLogonRight       │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-546                                                   │ SeInteractiveLogonRight       │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-581                                                   │ SeNetworkLogonRight           │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-581                                                   │ SeInteractiveLogonRight       │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-583                                                   │ SeNetworkLogonRight           │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-583                                                   │ SeInteractiveLogonRight       │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-80-3169285310-278349998-1452333686-3865143136-4212226833 │ SeServiceLogonRight           │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-90-0                                                     │ SeInteractiveLogonRight       │            1 │
└─────────────────┴────────────────┴───────────────────┴─────────────────┴────────────────────────────────────────────────────────────────┴───────────────────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4718' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AccessRemoved     │            9 │
│ $.Event.EventData.SubjectDomainName │            9 │
│ $.Event.EventData.SubjectLogonId    │            9 │
│ $.Event.EventData.SubjectUserName   │            9 │
│ $.Event.EventData.SubjectUserSid    │            9 │
│ $.Event.EventData.TargetSid         │            9 │
└─────────────────────────────────────┴──────────────┘
```



## 4719 (System audit policy was changed)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.SubcategoryGuid,Event.EventData.AuditPolicyChanges, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.EventID = 4719
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid ,Event.EventData.SubcategoryGuid;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬──────────────────────────────────────┬────────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │           SubcategoryGuid            │ AuditPolicyChanges │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │                 uuid                 │      varchar       │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼──────────────────────────────────────┼────────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9210-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9210-69ae-11d9-bed3-505054503030 │ %%8448             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9211-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9212-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9212-69ae-11d9-bed3-505054503030 │ %%8448, %%8450     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9213-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9214-69ae-11d9-bed3-505054503030 │ %%8448, %%8450     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9215-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9215-69ae-11d9-bed3-505054503030 │ %%8448, %%8450     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9216-69ae-11d9-bed3-505054503030 │ %%8448             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9216-69ae-11d9-bed3-505054503030 │ %%8449             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9217-69ae-11d9-bed3-505054503030 │ %%8448             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9217-69ae-11d9-bed3-505054503030 │ %%8449             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce921b-69ae-11d9-bed3-505054503030 │ %%8448             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce921b-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce921c-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9224-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce922d-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce922f-69ae-11d9-bed3-505054503030 │ %%8448             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce922f-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9230-69ae-11d9-bed3-505054503030 │ %%8448             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9230-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9232-69ae-11d9-bed3-505054503030 │ %%8449             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9235-69ae-11d9-bed3-505054503030 │ %%8451             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9236-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9237-69ae-11d9-bed3-505054503030 │ %%8451             │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce923a-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce923f-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9240-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9241-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9242-69ae-11d9-bed3-505054503030 │ %%8449, %%8451     │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ 0cce9243-69ae-11d9-bed3-505054503030 │ %%8448, %%8450     │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴──────────────────────────────────────┴────────────────────┴──────────────┘
  32 rows                                                                                                                                     7 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4719' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌──────────────────────────────────────┬──────────────┐
│               fullkey                │ count_star() │
│               varchar                │    int64     │
├──────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AuditPolicyChanges │           32 │
│ $.Event.EventData.CategoryId         │           32 │
│ $.Event.EventData.SubcategoryGuid    │           32 │
│ $.Event.EventData.SubcategoryId      │           32 │
│ $.Event.EventData.SubjectDomainName  │           32 │
│ $.Event.EventData.SubjectLogonId     │           32 │
│ $.Event.EventData.SubjectUserName    │           32 │
│ $.Event.EventData.SubjectUserSid     │           32 │
└──────────────────────────────────────┴──────────────┘
```
subcategory
```powershell
PS C:\Users\SANSDFIR> auditpol /list /subcategory:* /v
Category/Subcategory                    GUID
System                                  {69979848-797A-11D9-BED3-505054503030}
  Security State Change                     {0CCE9210-69AE-11D9-BED3-505054503030}
  Security System Extension                 {0CCE9211-69AE-11D9-BED3-505054503030}
  System Integrity                          {0CCE9212-69AE-11D9-BED3-505054503030}
  IPsec Driver                              {0CCE9213-69AE-11D9-BED3-505054503030}
  Other System Events                       {0CCE9214-69AE-11D9-BED3-505054503030}
Logon/Logoff                            {69979849-797A-11D9-BED3-505054503030}
  Logon                                     {0CCE9215-69AE-11D9-BED3-505054503030}
  Logoff                                    {0CCE9216-69AE-11D9-BED3-505054503030}
  Account Lockout                           {0CCE9217-69AE-11D9-BED3-505054503030}
  IPsec Main Mode                           {0CCE9218-69AE-11D9-BED3-505054503030}
  IPsec Quick Mode                          {0CCE9219-69AE-11D9-BED3-505054503030}
  IPsec Extended Mode                       {0CCE921A-69AE-11D9-BED3-505054503030}
  Special Logon                             {0CCE921B-69AE-11D9-BED3-505054503030}
  Other Logon/Logoff Events                 {0CCE921C-69AE-11D9-BED3-505054503030}
  Network Policy Server                     {0CCE9243-69AE-11D9-BED3-505054503030}
  User / Device Claims                      {0CCE9247-69AE-11D9-BED3-505054503030}
  Group Membership                          {0CCE9249-69AE-11D9-BED3-505054503030}
Object Access                           {6997984A-797A-11D9-BED3-505054503030}
  File System                               {0CCE921D-69AE-11D9-BED3-505054503030}
  Registry                                  {0CCE921E-69AE-11D9-BED3-505054503030}
  Kernel Object                             {0CCE921F-69AE-11D9-BED3-505054503030}
  SAM                                       {0CCE9220-69AE-11D9-BED3-505054503030}
  Certification Services                    {0CCE9221-69AE-11D9-BED3-505054503030}
  Application Generated                     {0CCE9222-69AE-11D9-BED3-505054503030}
  Handle Manipulation                       {0CCE9223-69AE-11D9-BED3-505054503030}
  File Share                                {0CCE9224-69AE-11D9-BED3-505054503030}
  Filtering Platform Packet Drop            {0CCE9225-69AE-11D9-BED3-505054503030}
  Filtering Platform Connection             {0CCE9226-69AE-11D9-BED3-505054503030}
  Other Object Access Events                {0CCE9227-69AE-11D9-BED3-505054503030}
  Detailed File Share                       {0CCE9244-69AE-11D9-BED3-505054503030}
  Removable Storage                         {0CCE9245-69AE-11D9-BED3-505054503030}
  Central Policy Staging                    {0CCE9246-69AE-11D9-BED3-505054503030}
Privilege Use                           {6997984B-797A-11D9-BED3-505054503030}
  Sensitive Privilege Use                   {0CCE9228-69AE-11D9-BED3-505054503030}
  Non Sensitive Privilege Use               {0CCE9229-69AE-11D9-BED3-505054503030}
  Other Privilege Use Events                {0CCE922A-69AE-11D9-BED3-505054503030}
Detailed Tracking                       {6997984C-797A-11D9-BED3-505054503030}
  Process Creation                          {0CCE922B-69AE-11D9-BED3-505054503030}
  Process Termination                       {0CCE922C-69AE-11D9-BED3-505054503030}
  DPAPI Activity                            {0CCE922D-69AE-11D9-BED3-505054503030}
  RPC Events                                {0CCE922E-69AE-11D9-BED3-505054503030}
  Plug and Play Events                      {0CCE9248-69AE-11D9-BED3-505054503030}
  Token Right Adjusted Events               {0CCE924A-69AE-11D9-BED3-505054503030}
Policy Change                           {6997984D-797A-11D9-BED3-505054503030}
  Audit Policy Change                       {0CCE922F-69AE-11D9-BED3-505054503030}
  Authentication Policy Change              {0CCE9230-69AE-11D9-BED3-505054503030}
  Authorization Policy Change               {0CCE9231-69AE-11D9-BED3-505054503030}
  MPSSVC Rule-Level Policy Change           {0CCE9232-69AE-11D9-BED3-505054503030}
  Filtering Platform Policy Change          {0CCE9233-69AE-11D9-BED3-505054503030}
  Other Policy Change Events                {0CCE9234-69AE-11D9-BED3-505054503030}
Account Management                      {6997984E-797A-11D9-BED3-505054503030}
  User Account Management                   {0CCE9235-69AE-11D9-BED3-505054503030}
  Computer Account Management               {0CCE9236-69AE-11D9-BED3-505054503030}
  Security Group Management                 {0CCE9237-69AE-11D9-BED3-505054503030}
  Distribution Group Management             {0CCE9238-69AE-11D9-BED3-505054503030}
  Application Group Management              {0CCE9239-69AE-11D9-BED3-505054503030}
  Other Account Management Events           {0CCE923A-69AE-11D9-BED3-505054503030}
DS Access                               {6997984F-797A-11D9-BED3-505054503030}
  Directory Service Access                  {0CCE923B-69AE-11D9-BED3-505054503030}
  Directory Service Changes                 {0CCE923C-69AE-11D9-BED3-505054503030}
  Directory Service Replication             {0CCE923D-69AE-11D9-BED3-505054503030}
  Detailed Directory Service Replication    {0CCE923E-69AE-11D9-BED3-505054503030}
Account Logon                           {69979850-797A-11D9-BED3-505054503030}
  Credential Validation                     {0CCE923F-69AE-11D9-BED3-505054503030}
  Kerberos Service Ticket Operations        {0CCE9240-69AE-11D9-BED3-505054503030}
  Other Account Logon Events                {0CCE9241-69AE-11D9-BED3-505054503030}
  Kerberos Authentication Service           {0CCE9242-69AE-11D9-BED3-505054503030}
```



## 4720 (A new account was created)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.SamAccountName, Event.EventData.UserPrincipalName, Event.EventData.PrimaryGroupId, Event.EventData.UserParameters, Event.EventData.DisplayName,  Event.EventData.HomeDirectory, Event.EventData.HomePath, Event.EventData.ScriptPath, Event.EventData.ProfilePath, Event.EventData.UserWorkstations, Event.EventData.PasswordLastSet, Event.EventData.AccountExpires, Event.EventData.LogonHours, Event.EventData.OldUacValue, Event.EventData.NewUacValue, Event.EventData.UserAccountControl,  Event.EventData.AllowedToDelegateTo, Event.EventData.SidHistory, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.EventID = 4720
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────┬────────────────┬───────────────────┬─────────────────┬───────────────────────────────────────────────┬──────────────────┬────────────────────┬────────────────────┬───────────────────┬────────────────┬────────────────┬─────────────┬───────────────┬──────────┬────────────┬─────────────┬──────────────────┬─────────────────┬────────────────┬────────────┬─────────────┬─────────────┬────────────────────────────────────────────┬─────────────────────┬────────────┬──────────────┐
│    Computer     │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                   TargetSid                   │ TargetDomainName │   TargetUserName   │   SamAccountName   │ UserPrincipalName │ PrimaryGroupId │ UserParameters │ DisplayName │ HomeDirectory │ HomePath │ ScriptPath │ ProfilePath │ UserWorkstations │ PasswordLastSet │ AccountExpires │ LogonHours │ OldUacValue │ NewUacValue │             UserAccountControl             │ AllowedToDelegateTo │ SidHistory │ count_star() │
│     varchar     │    varchar     │      varchar      │     varchar     │                    varchar                    │     varchar      │      varchar       │      varchar       │      varchar      │    varchar     │    varchar     │   varchar   │    varchar    │ varchar  │  varchar   │   varchar   │     varchar      │     varchar     │    varchar     │  varchar   │   varchar   │   varchar   │                  varchar                   │       varchar       │  varchar   │    int64     │
├─────────────────┼────────────────┼───────────────────┼─────────────────┼───────────────────────────────────────────────┼──────────────────┼────────────────────┼────────────────────┼───────────────────┼────────────────┼────────────────┼─────────────┼───────────────┼──────────┼────────────┼─────────────┼──────────────────┼─────────────────┼────────────────┼────────────┼─────────────┼─────────────┼────────────────────────────────────────────┼─────────────────────┼────────────┼──────────────┤
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-21-2908624845-2463485410-257172065-1000 │ TPL-PACKER       │ defaultuser0       │ defaultuser0       │ -                 │ 513            │ %%1793         │ %%1793      │ %%1793        │ %%1793   │ %%1793     │ %%1793      │ %%1793           │ %%1794          │ %%1794         │ %%1797     │ 0x0         │ 0x15        │ \r\n\t\t%%2080\r\n\t\t%%2082\r\n\t\t%%2084 │ -                   │ -          │            1 │
│ rd01            │ S-1-5-18       │ WORKGROUP         │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ RD01             │ SRLAdmin           │ SRLAdmin           │ -                 │ 513            │ %%1793         │ %%1793      │ %%1793        │ %%1793   │ %%1793     │ %%1793      │ %%1793           │ %%1794          │ %%1794         │ %%1797     │ 0x0         │ 0x15        │ \r\n\t\t%%2080\r\n\t\t%%2082\r\n\t\t%%2084 │ -                   │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-21-2908624845-2463485410-257172065-504  │ MINWINPC         │ WDAGUtilityAccount │ WDAGUtilityAccount │ -                 │ 513            │ %%1793         │ %%1793      │ %%1793        │ %%1793   │ %%1793     │ %%1793      │ %%1793           │ %%1794          │ %%1794         │ %%1797     │ 0x0         │ 0x15        │ \r\n\t\t%%2080\r\n\t\t%%2082\r\n\t\t%%2084 │ -                   │ -          │            1 │
└─────────────────┴────────────────┴───────────────────┴─────────────────┴───────────────────────────────────────────────┴──────────────────┴────────────────────┴────────────────────┴───────────────────┴────────────────┴────────────────┴─────────────┴───────────────┴──────────┴────────────┴─────────────┴──────────────────┴─────────────────┴────────────────┴────────────┴─────────────┴─────────────┴────────────────────────────────────────────┴─────────────────────┴────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4720' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌───────────────────────────────────────┬──────────────┐
│                fullkey                │ count_star() │
│                varchar                │    int64     │
├───────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AccountExpires      │            3 │
│ $.Event.EventData.AllowedToDelegateTo │            3 │
│ $.Event.EventData.DisplayName         │            3 │
│ $.Event.EventData.HomeDirectory       │            3 │
│ $.Event.EventData.HomePath            │            3 │
│ $.Event.EventData.LogonHours          │            3 │
│ $.Event.EventData.NewUacValue         │            3 │
│ $.Event.EventData.OldUacValue         │            3 │
│ $.Event.EventData.PasswordLastSet     │            3 │
│ $.Event.EventData.PrimaryGroupId      │            3 │
│ $.Event.EventData.PrivilegeList       │            3 │
│ $.Event.EventData.ProfilePath         │            3 │
│ $.Event.EventData.SamAccountName      │            3 │
│ $.Event.EventData.ScriptPath          │            3 │
│ $.Event.EventData.SidHistory          │            3 │
│ $.Event.EventData.SubjectDomainName   │            3 │
│ $.Event.EventData.SubjectLogonId      │            3 │
│ $.Event.EventData.SubjectUserName     │            3 │
│ $.Event.EventData.SubjectUserSid      │            3 │
│ $.Event.EventData.TargetDomainName    │            3 │
│ $.Event.EventData.TargetSid           │            3 │
│ $.Event.EventData.TargetUserName      │            3 │
│ $.Event.EventData.UserAccountControl  │            3 │
│ $.Event.EventData.UserParameters      │            3 │
│ $.Event.EventData.UserPrincipalName   │            3 │
│ $.Event.EventData.UserWorkstations    │            3 │
└───────────────────────────────────────┴──────────────┘
  26 rows                                    2 columns
```



## 4722 (A user account was enabled)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.EventID = 4722
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌────────────┬────────────────┬───────────────────┬─────────────────┬───────────────────────────────────────────────┬──────────────────┬────────────────┬──────────────┐
│  Computer  │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                   TargetSid                   │ TargetDomainName │ TargetUserName │ count_star() │
│  varchar   │    varchar     │      varchar      │     varchar     │                    varchar                    │     varchar      │    varchar     │    int64     │
├────────────┼────────────────┼───────────────────┼─────────────────┼───────────────────────────────────────────────┼──────────────────┼────────────────┼──────────────┤
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-21-2908624845-2463485410-257172065-1000 │ TPL-PACKER       │ defaultuser0   │            1 │
│ rd01       │ S-1-5-18       │ WORKGROUP         │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ RD01             │ SRLAdmin       │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-21-2908624845-2463485410-257172065-500  │ TPL-PACKER       │ Administrator  │            1 │
└────────────┴────────────────┴───────────────────┴─────────────────┴───────────────────────────────────────────────┴──────────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4722' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.SubjectDomainName │            3 │
│ $.Event.EventData.SubjectLogonId    │            3 │
│ $.Event.EventData.SubjectUserName   │            3 │
│ $.Event.EventData.SubjectUserSid    │            3 │
│ $.Event.EventData.TargetDomainName  │            3 │
│ $.Event.EventData.TargetSid         │            3 │
│ $.Event.EventData.TargetUserName    │            3 │
└─────────────────────────────────────┴──────────────┘
```



## 4723 (An attempt was made to change an account's password)



## 4724 (An attempt was made to reset an account's password)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4724
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬───────────────────────────────────────────────┬──────────────────┬────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                   TargetSid                   │ TargetDomainName │ TargetUserName │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │                    varchar                    │     varchar      │    varchar     │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼───────────────────────────────────────────────┼──────────────────┼────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ RD01             │ SRLAdmin       │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴───────────────────────────────────────────────┴──────────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4724' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.SubjectDomainName │           18 │
│ $.Event.EventData.SubjectLogonId    │           18 │
│ $.Event.EventData.SubjectUserName   │           18 │
│ $.Event.EventData.SubjectUserSid    │           18 │
│ $.Event.EventData.TargetDomainName  │           18 │
│ $.Event.EventData.TargetSid         │           18 │
│ $.Event.EventData.TargetUserName    │           18 │
└─────────────────────────────────────┴──────────────┘
```



## 4725 (A user account was disabled)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4725
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬──────────────────────────────────────────────┬──────────────────┬────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                  TargetSid                   │ TargetDomainName │ TargetUserName │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │                   varchar                    │     varchar      │    varchar     │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼──────────────────────────────────────────────┼──────────────────┼────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-500 │ RD01             │ Administrator  │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴──────────────────────────────────────────────┴──────────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4725' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.SubjectDomainName │            4 │
│ $.Event.EventData.SubjectLogonId    │            4 │
│ $.Event.EventData.SubjectUserName   │            4 │
│ $.Event.EventData.SubjectUserSid    │            4 │
│ $.Event.EventData.TargetDomainName  │            4 │
│ $.Event.EventData.TargetSid         │            4 │
│ $.Event.EventData.TargetUserName    │            4 │
└─────────────────────────────────────┴──────────────┘
```



## 4726 (A user account was deleted)
```sql
security-rd08 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.PrivilegeList, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd08.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4726
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬────────────────────────────────────────────────┬──────────────────┬────────────────┬───────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                   TargetSid                    │ TargetDomainName │ TargetUserName │ PrivilegeList │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │                    varchar                     │     varchar      │    varchar     │    varchar    │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────┼──────────────────┼────────────────┼───────────────┼──────────────┤
│ rd08.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD08$           │ S-1-5-21-2251190123-3620890617-4175171818-1000 │ RD08             │ dfargo         │ -             │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴────────────────────────────────────────────────┴──────────────────┴────────────────┴───────────────┴──────────────┘
```
fullkey
```sql
security-rd08 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd08.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4726' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.PrivilegeList     │            1 │
│ $.Event.EventData.SubjectDomainName │            1 │
│ $.Event.EventData.SubjectLogonId    │            1 │
│ $.Event.EventData.SubjectUserName   │            1 │
│ $.Event.EventData.SubjectUserSid    │            1 │
│ $.Event.EventData.TargetDomainName  │            1 │
│ $.Event.EventData.TargetSid         │            1 │
│ $.Event.EventData.TargetUserName    │            1 │
└─────────────────────────────────────┴──────────────┘
```



## 4728 (A member was added to a security-enabled global group)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.MemberSid, Event.EventData.MemberName, Event.EventData.PrivilegeList, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.EventID = 4728
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid, Event.EventData.MemberSid;
┌─────────────────┬────────────────┬───────────────────┬─────────────────┬──────────────────────────────────────────────┬──────────────────┬────────────────┬───────────────────────────────────────────────┬────────────┬───────────────┬──────────────┐
│    Computer     │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                  TargetSid                   │ TargetDomainName │ TargetUserName │                   MemberSid                   │ MemberName │ PrivilegeList │ count_star() │
│     varchar     │    varchar     │      varchar      │     varchar     │                   varchar                    │     varchar      │    varchar     │                    varchar                    │  varchar   │    varchar    │    int64     │
├─────────────────┼────────────────┼───────────────────┼─────────────────┼──────────────────────────────────────────────┼──────────────────┼────────────────┼───────────────────────────────────────────────┼────────────┼───────────────┼──────────────┤
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-21-2908624845-2463485410-257172065-513 │ TPL-PACKER       │ None           │ S-1-5-21-2908624845-2463485410-257172065-1000 │ -          │ -             │            1 │
│ rd01            │ S-1-5-18       │ WORKGROUP         │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-513 │ RD01             │ None           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ -          │ -             │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-21-2908624845-2463485410-257172065-513 │ MINWINPC         │ None           │ S-1-5-21-2908624845-2463485410-257172065-504  │ -          │ -             │            1 │
└─────────────────┴────────────────┴───────────────────┴─────────────────┴──────────────────────────────────────────────┴──────────────────┴────────────────┴───────────────────────────────────────────────┴────────────┴───────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4728' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.MemberName        │            3 │
│ $.Event.EventData.MemberSid         │            3 │
│ $.Event.EventData.PrivilegeList     │            3 │
│ $.Event.EventData.SubjectDomainName │            3 │
│ $.Event.EventData.SubjectLogonId    │            3 │
│ $.Event.EventData.SubjectUserName   │            3 │
│ $.Event.EventData.SubjectUserSid    │            3 │
│ $.Event.EventData.TargetDomainName  │            3 │
│ $.Event.EventData.TargetSid         │            3 │
│ $.Event.EventData.TargetUserName    │            3 │
└─────────────────────────────────────┴──────────────┘
  10 rows                                  2 columns
```



## 4731 (A security-enabled local group was created)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.SamAccountName, Event.EventData.PrivilegeList, Event.EventData.SidHistory, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.EventID = 4731
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────┬────────────────┬───────────────────┬─────────────────┬──────────────┬──────────────────┬─────────────────────────────────────┬─────────────────────────────────────┬───────────────┬────────────┬──────────────┐
│    Computer     │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │  TargetSid   │ TargetDomainName │           TargetUserName            │           SamAccountName            │ PrivilegeList │ SidHistory │ count_star() │
│     varchar     │    varchar     │      varchar      │     varchar     │   varchar    │     varchar      │               varchar               │               varchar               │    varchar    │  varchar   │    int64     │
├─────────────────┼────────────────┼───────────────────┼─────────────────┼──────────────┼──────────────────┼─────────────────────────────────────┼─────────────────────────────────────┼───────────────┼────────────┼──────────────┤
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-555 │ Builtin          │ Remote Desktop Users                │ Remote Desktop Users                │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-556 │ Builtin          │ Network Configuration Operators     │ Network Configuration Operators     │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-558 │ Builtin          │ Performance Monitor Users           │ Performance Monitor Users           │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-559 │ Builtin          │ Performance Log Users               │ Performance Log Users               │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-562 │ Builtin          │ Distributed COM Users               │ Distributed COM Users               │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-568 │ Builtin          │ IIS_IUSRS                           │ IIS_IUSRS                           │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-569 │ Builtin          │ Cryptographic Operators             │ Cryptographic Operators             │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-573 │ Builtin          │ Event Log Readers                   │ Event Log Readers                   │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-578 │ Builtin          │ Hyper-V Administrators              │ Hyper-V Administrators              │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-579 │ Builtin          │ Access Control Assistance Operators │ Access Control Assistance Operators │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-580 │ Builtin          │ Remote Management Users             │ Remote Management Users             │ -             │ -          │            1 │
└─────────────────┴────────────────┴───────────────────┴─────────────────┴──────────────┴──────────────────┴─────────────────────────────────────┴─────────────────────────────────────┴───────────────┴────────────┴──────────────┘
  11 rows                                                                                                                                                                                                               11 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4731' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.PrivilegeList     │           11 │
│ $.Event.EventData.SamAccountName    │           11 │
│ $.Event.EventData.SidHistory        │           11 │
│ $.Event.EventData.SubjectDomainName │           11 │
│ $.Event.EventData.SubjectLogonId    │           11 │
│ $.Event.EventData.SubjectUserName   │           11 │
│ $.Event.EventData.SubjectUserSid    │           11 │
│ $.Event.EventData.TargetDomainName  │           11 │
│ $.Event.EventData.TargetSid         │           11 │
│ $.Event.EventData.TargetUserName    │           11 │
└─────────────────────────────────────┴──────────────┘
  10 rows                                  2 columns
```



## 4732 (A member was added to a security-enabled local group)
```sql
security-rd09 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.MemberSid, Event.EventData.MemberName, Event.EventData.PrivilegeList, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd09.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4732
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid, Event.EventData.MemberSid;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬──────────────┬──────────────────┬────────────────┬────────────────────────────────────────────────┬────────────┬───────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │  TargetSid   │ TargetDomainName │ TargetUserName │                   MemberSid                    │ MemberName │ PrivilegeList │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │   varchar    │     varchar      │    varchar     │                    varchar                     │  varchar   │    varchar    │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼──────────────┼──────────────────┼────────────────┼────────────────────────────────────────────────┼────────────┼───────────────┼──────────────┤
│ rd09.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD09$           │ S-1-5-32-544 │ Builtin          │ Administrators │ S-1-5-21-2838623409-1327563992-2591358621-1134 │ -          │ -             │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴──────────────┴──────────────────┴────────────────┴────────────────────────────────────────────────┴────────────┴───────────────┴──────────────┘
```
fullkey
```sql
security-rd09 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd09.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4732' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.MemberName        │           12 │
│ $.Event.EventData.MemberSid         │           12 │
│ $.Event.EventData.PrivilegeList     │           12 │
│ $.Event.EventData.SubjectDomainName │           12 │
│ $.Event.EventData.SubjectLogonId    │           12 │
│ $.Event.EventData.SubjectUserName   │           12 │
│ $.Event.EventData.SubjectUserSid    │           12 │
│ $.Event.EventData.TargetDomainName  │           12 │
│ $.Event.EventData.TargetSid         │           12 │
│ $.Event.EventData.TargetUserName    │           12 │
└─────────────────────────────────────┴──────────────┘
  10 rows                                  2 columns
```



## 4733 (A member was removed from a security-enabled local group)
```sql
security-rd08 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.MemberSid, Event.EventData.MemberName, Event.EventData.PrivilegeList, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd08.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4733
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid, Event.EventData.MemberSid;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬──────────────┬──────────────────┬────────────────┬────────────────────────────────────────────────┬────────────┬───────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │  TargetSid   │ TargetDomainName │ TargetUserName │                   MemberSid                    │ MemberName │ PrivilegeList │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │   varchar    │     varchar      │    varchar     │                    varchar                     │  varchar   │    varchar    │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼──────────────┼──────────────────┼────────────────┼────────────────────────────────────────────────┼────────────┼───────────────┼──────────────┤
│ rd08.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD08$           │ S-1-5-32-544 │ Builtin          │ Administrators │ S-1-5-21-2251190123-3620890617-4175171818-1000 │ -          │ -             │            1 │
│ rd08.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD08$           │ S-1-5-32-545 │ Builtin          │ Users          │ S-1-5-21-2251190123-3620890617-4175171818-1000 │ -          │ -             │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴──────────────┴──────────────────┴────────────────┴────────────────────────────────────────────────┴────────────┴───────────────┴──────────────┘
```
fullkey
```sql
security-rd08 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd08.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4733' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.MemberName        │            5 │
│ $.Event.EventData.MemberSid         │            5 │
│ $.Event.EventData.PrivilegeList     │            5 │
│ $.Event.EventData.SubjectDomainName │            5 │
│ $.Event.EventData.SubjectLogonId    │            5 │
│ $.Event.EventData.SubjectUserName   │            5 │
│ $.Event.EventData.SubjectUserSid    │            5 │
│ $.Event.EventData.TargetDomainName  │            5 │
│ $.Event.EventData.TargetSid         │            5 │
│ $.Event.EventData.TargetUserName    │            5 │
└─────────────────────────────────────┴──────────────┘
  10 rows                                  2 columns
```



## 4734 (A security-enabled local group was deleted)



## 4735 (A security-enabled local group was changed)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.SamAccountName, Event.EventData.PrivilegeList, Event.EventData.SidHistory, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.EventID = 4735
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────┬────────────────┬───────────────────┬─────────────────┬──────────────┬──────────────────┬─────────────────────────────────────┬─────────────────────────────────────┬───────────────┬────────────┬──────────────┐
│    Computer     │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │  TargetSid   │ TargetDomainName │           TargetUserName            │           SamAccountName            │ PrivilegeList │ SidHistory │ count_star() │
│     varchar     │    varchar     │      varchar      │     varchar     │   varchar    │     varchar      │               varchar               │               varchar               │    varchar    │  varchar   │    int64     │
├─────────────────┼────────────────┼───────────────────┼─────────────────┼──────────────┼──────────────────┼─────────────────────────────────────┼─────────────────────────────────────┼───────────────┼────────────┼──────────────┤
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-544 │ Builtin          │ Administrators                      │ Administrators                      │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-544 │ Builtin          │ Administrators                      │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-545 │ Builtin          │ Users                               │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-545 │ Builtin          │ Users                               │ Users                               │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-546 │ Builtin          │ Guests                              │ Guests                              │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-546 │ Builtin          │ Guests                              │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-547 │ Builtin          │ Power Users                         │ Power Users                         │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-547 │ Builtin          │ Power Users                         │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-551 │ Builtin          │ Backup Operators                    │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-551 │ Builtin          │ Backup Operators                    │ Backup Operators                    │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-552 │ Builtin          │ Replicator                          │ Replicator                          │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-552 │ Builtin          │ Replicator                          │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-555 │ Builtin          │ Remote Desktop Users                │ -                                   │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-555 │ Builtin          │ Remote Desktop Users                │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-555 │ Builtin          │ Remote Desktop Users                │ Remote Desktop Users                │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-556 │ Builtin          │ Network Configuration Operators     │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-556 │ Builtin          │ Network Configuration Operators     │ Network Configuration Operators     │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-556 │ Builtin          │ Network Configuration Operators     │ -                                   │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-558 │ Builtin          │ Performance Monitor Users           │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-558 │ Builtin          │ Performance Monitor Users           │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-558 │ Builtin          │ Performance Monitor Users           │ Performance Monitor Users           │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-559 │ Builtin          │ Performance Log Users               │ -                                   │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-559 │ Builtin          │ Performance Log Users               │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-559 │ Builtin          │ Performance Log Users               │ Performance Log Users               │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-562 │ Builtin          │ Distributed COM Users               │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-562 │ Builtin          │ Distributed COM Users               │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-562 │ Builtin          │ Distributed COM Users               │ Distributed COM Users               │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-568 │ Builtin          │ IIS_IUSRS                           │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-568 │ Builtin          │ IIS_IUSRS                           │ IIS_IUSRS                           │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-568 │ Builtin          │ IIS_IUSRS                           │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-569 │ Builtin          │ Cryptographic Operators             │ Cryptographic Operators             │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-569 │ Builtin          │ Cryptographic Operators             │ -                                   │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-569 │ Builtin          │ Cryptographic Operators             │ -                                   │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-573 │ Builtin          │ Event Log Readers                   │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-573 │ Builtin          │ Event Log Readers                   │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-573 │ Builtin          │ Event Log Readers                   │ Event Log Readers                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-578 │ Builtin          │ Hyper-V Administrators              │ -                                   │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-578 │ Builtin          │ Hyper-V Administrators              │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-578 │ Builtin          │ Hyper-V Administrators              │ Hyper-V Administrators              │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-579 │ Builtin          │ Access Control Assistance Operators │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-579 │ Builtin          │ Access Control Assistance Operators │ Access Control Assistance Operators │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-579 │ Builtin          │ Access Control Assistance Operators │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-580 │ Builtin          │ Remote Management Users             │ Remote Management Users             │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-580 │ Builtin          │ Remote Management Users             │ -                                   │ -             │ -          │            1 │
│ OFFDEVS-TUHMGJE │ S-1-5-18       │                   │ MINWINPC$       │ S-1-5-32-580 │ Builtin          │ Remote Management Users             │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-581 │ Builtin          │ System Managed Accounts Group       │ -                                   │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-581 │ Builtin          │ System Managed Accounts Group       │ System Managed Accounts Group       │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-583 │ Builtin          │ Device Owners                       │ Device Owners                       │ -             │ -          │            1 │
│ tpl-packer      │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-583 │ Builtin          │ Device Owners                       │ -                                   │ -             │ -          │            1 │
└─────────────────┴────────────────┴───────────────────┴─────────────────┴──────────────┴──────────────────┴─────────────────────────────────────┴─────────────────────────────────────┴───────────────┴────────────┴──────────────┘
  49 rows                                                                                                                                                                                                               11 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4735' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.PrivilegeList     │           49 │
│ $.Event.EventData.SamAccountName    │           49 │
│ $.Event.EventData.SidHistory        │           49 │
│ $.Event.EventData.SubjectDomainName │           49 │
│ $.Event.EventData.SubjectLogonId    │           49 │
│ $.Event.EventData.SubjectUserName   │           49 │
│ $.Event.EventData.SubjectUserSid    │           49 │
│ $.Event.EventData.TargetDomainName  │           49 │
│ $.Event.EventData.TargetSid         │           49 │
│ $.Event.EventData.TargetUserName    │           49 │
└─────────────────────────────────────┴──────────────┘
  10 rows                                  2 columns
```



## 4738 (A user account was changed)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.SamAccountName, Event.EventData.UserPrincipalName, Event.EventData.PrimaryGroupId, Event.EventData.PrivilegeList, Event.EventData.UserParameters, Event.EventData.DisplayName,  Event.EventData.HomeDirectory, Event.EventData.HomePath, Event.EventData.ScriptPath, Event.EventData.ProfilePath, Event.EventData.UserWorkstations, Event.EventData.PasswordLastSet, Event.EventData.AccountExpires, Event.EventData.LogonHours, Event.EventData.OldUacValue, Event.EventData.NewUacValue, Event.EventData.UserAccountControl,  Event.EventData.AllowedToDelegateTo, Event.EventData.SidHistory, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4738
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid , Event.EventData.TargetSid;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬───────────────────────────────────────────────┬──────────────────┬────────────────┬────────────────┬───────────────────┬────────────────┬───────────────┬────────────────┬─────────────┬───────────────┬──────────┬────────────┬─────────────┬──────────────────┬─────────────────────┬────────────────┬────────────┬─────────────┬─────────────┬────────────────────┬─────────────────────┬────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                   TargetSid                   │ TargetDomainName │ TargetUserName │ SamAccountName │ UserPrincipalName │ PrimaryGroupId │ PrivilegeList │ UserParameters │ DisplayName │ HomeDirectory │ HomePath │ ScriptPath │ ProfilePath │ UserWorkstations │   PasswordLastSet   │ AccountExpires │ LogonHours │ OldUacValue │ NewUacValue │ UserAccountControl │ AllowedToDelegateTo │ SidHistory │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │                    varchar                    │     varchar      │    varchar     │    varchar     │      varchar      │    varchar     │    varchar    │    varchar     │   varchar   │    varchar    │ varchar  │  varchar   │   varchar   │     varchar      │       varchar       │    varchar     │  varchar   │   varchar   │   varchar   │      varchar       │       varchar       │  varchar   │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼───────────────────────────────────────────────┼──────────────────┼────────────────┼────────────────┼───────────────────┼────────────────┼───────────────┼────────────────┼─────────────┼───────────────┼──────────┼────────────┼─────────────┼──────────────────┼─────────────────────┼────────────────┼────────────┼─────────────┼─────────────┼────────────────────┼─────────────────────┼────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ RD01             │ SRLAdmin       │ SRLAdmin       │ -                 │ 513            │ -             │ -              │ %%1793      │ %%1793        │ %%1793   │ %%1793     │ %%1793      │ %%1793           │ 1/2/2023 2:59:48 PM │ %%1794         │ %%1797     │ 0x10        │ 0x10        │ -                  │ -                   │ -          │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-500  │ RD01             │ Administrator  │ -              │ -                 │ -              │ -             │ -              │ -           │ -             │ -        │ -          │ -           │ -                │ -                   │ -              │ -          │ 0x210       │ 0x211       │ \r\n\t\t%%2080     │ -                   │ -          │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴───────────────────────────────────────────────┴──────────────────┴────────────────┴────────────────┴───────────────────┴────────────────┴───────────────┴────────────────┴─────────────┴───────────────┴──────────┴────────────┴─────────────┴──────────────────┴─────────────────────┴────────────────┴────────────┴─────────────┴─────────────┴────────────────────┴─────────────────────┴────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4738' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌───────────────────────────────────────┬──────────────┐
│                fullkey                │ count_star() │
│                varchar                │    int64     │
├───────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AccountExpires      │           35 │
│ $.Event.EventData.AllowedToDelegateTo │           35 │
│ $.Event.EventData.DisplayName         │           35 │
│ $.Event.EventData.Dummy               │           35 │
│ $.Event.EventData.HomeDirectory       │           35 │
│ $.Event.EventData.HomePath            │           35 │
│ $.Event.EventData.LogonHours          │           35 │
│ $.Event.EventData.NewUacValue         │           35 │
│ $.Event.EventData.OldUacValue         │           35 │
│ $.Event.EventData.PasswordLastSet     │           35 │
│ $.Event.EventData.PrimaryGroupId      │           35 │
│ $.Event.EventData.PrivilegeList       │           35 │
│ $.Event.EventData.ProfilePath         │           35 │
│ $.Event.EventData.SamAccountName      │           35 │
│ $.Event.EventData.ScriptPath          │           35 │
│ $.Event.EventData.SidHistory          │           35 │
│ $.Event.EventData.SubjectDomainName   │           35 │
│ $.Event.EventData.SubjectLogonId      │           35 │
│ $.Event.EventData.SubjectUserName     │           35 │
│ $.Event.EventData.SubjectUserSid      │           35 │
│ $.Event.EventData.TargetDomainName    │           35 │
│ $.Event.EventData.TargetSid           │           35 │
│ $.Event.EventData.TargetUserName      │           35 │
│ $.Event.EventData.UserAccountControl  │           35 │
│ $.Event.EventData.UserParameters      │           35 │
│ $.Event.EventData.UserPrincipalName   │           35 │
│ $.Event.EventData.UserWorkstations    │           35 │
└───────────────────────────────────────┴──────────────┘
  27 rows                                    2 columns
```
UAC  
https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/useraccountcontrol-manipulate-account-properties#list-of-property-flags



## 4740 (A user account was locked out)



## 4741 (A computer account was created)



## 4742 (A computer account was changed)



## 4743 (A computer account was deleted)



## 4764 (A group's type was changed)



## 4768 (A Kerberos authentication ticket (TGT) was requested)
```sql
security-dc01 D SELECT Event.System.Computer, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.ServiceSid, Event.EventData.ServiceName, Event.EventData.PreAuthType, Event.EventData.TicketEncryptionType, Event.EventData.TicketOptions, Event.EventData.Status, Event.EventData.IpAddress, Event.EventData.CertIssuerName, Event.EventData.CertSerialNumber, Event.EventData.CertThumbprint, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND json_extract_string(Event.System.EventID, '$') = '4768'
                GROUP BY ALL
                ORDER BY Event.EventData.TargetSid, Event.EventData.ServiceSid, Event.EventData.IpAddress;
┌─────────────────────┬────────────────────────────────────────────────┬──────────────────┬──────────────────────┬───────────────────────────────────────────────┬─────────────┬─────────────┬──────────────────────┬───────────────┬─────────┬────────────────────┬────────────────┬──────────────────┬────────────────┬──────────────┐
│      Computer       │                   TargetSid                    │ TargetDomainName │    TargetUserName    │                  ServiceSid                   │ ServiceName │ PreAuthType │ TicketEncryptionType │ TicketOptions │ Status  │     IpAddress      │ CertIssuerName │ CertSerialNumber │ CertThumbprint │ count_star() │
│       varchar       │                    varchar                     │     varchar      │       varchar        │                    varchar                    │   varchar   │   varchar   │       varchar        │    varchar    │ varchar │      varchar       │    varchar     │     varchar      │    varchar     │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼──────────────────┼──────────────────────┼───────────────────────────────────────────────┼─────────────┼─────────────┼──────────────────────┼───────────────┼─────────┼────────────────────┼────────────────┼──────────────────┼────────────────┼──────────────┤
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1001 │ SHIELDBASE.COM   │ DC01$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::1                │                │                  │                │           74 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1001 │ shieldbase.com   │ DC01$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::1                │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1105 │ SHIELDBASE.COM   │ Jblake               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.13 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1106 │ SHIELDBASE.COM   │ bnicholson           │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.12 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1106 │ shieldbase.com   │ bnicholson           │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.12 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1107 │ shieldbase.com   │ cbarton              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1107 │ SHIELDBASE.COM   │ cbarton              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase       │ cbarton-a            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ SHIELDBASE.COM   │ cbarton-a            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ SHIELDBASE       │ cbarton-a            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │          310 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase.com   │ cbarton-a            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1109 │ SHIELDBASE.COM   │ czelman              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.13 │                │                  │                │            9 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1109 │ shieldbase.com   │ czelman              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.13 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1110 │ SHIELDBASE.COM   │ dwatson              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.19 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1110 │ shieldbase.com   │ dwatson              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.19 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1113 │ SHIELDBASE.COM   │ jpallen              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.12 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1115 │ shieldbase.com   │ jzoch                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.17 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1115 │ SHIELDBASE.COM   │ jzoch                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.17 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1116 │ SHIELDBASE.COM   │ kespinoza            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.16 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1116 │ shieldbase.com   │ kespinoza            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.16 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1118 │ shieldbase.com   │ mhill                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1119 │ shieldbase.com   │ nfury                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1119 │ SHIELDBASE.COM   │ nfury                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1120 │ shieldbase.com   │ nromanoff            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.14 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1120 │ SHIELDBASE.COM   │ nromanoff            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.14 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1122 │ shieldbase.com   │ rdodson              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.15 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1122 │ SHIELDBASE.COM   │ rdodson              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.15 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1124 │ shieldbase       │ rsydow               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1124 │ SHIELDBASE.COM   │ rsydow               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1124 │ shieldbase.com   │ rsydow               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::1                │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::1                │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.7  │                │                  │                │           11 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1126 │ SHIELDBASE.COM   │ smckee               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.20 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1132 │ SHIELDBASE.COM   │ sreese               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.18 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1132 │ shieldbase.com   │ sreese               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.18 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1134 │ shieldbase.com   │ srogers              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.19 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1134 │ SHIELDBASE.COM   │ srogers              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.19 │                │                  │                │           10 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1135 │ shieldbase.com   │ tanderson            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.20 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1135 │ SHIELDBASE.COM   │ tanderson            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.20 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ SHIELDBASE.COM   │ tdungan              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase.com   │ tdungan              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           11 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1137 │ SHIELDBASE.COM   │ toslund              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.16 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1138 │ SHIELDBASE.COM   │ zdorsey              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.14 │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1138 │ shieldbase.com   │ zdorsey              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.14 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1139 │ shieldbase.com   │ RD03$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.13 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1139 │ SHIELDBASE.COM   │ RD03$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.13 │                │                  │                │           41 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1140 │ SHIELDBASE.COM   │ RD05$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.15 │                │                  │                │           58 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1140 │ shieldbase.com   │ RD05$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.15 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1141 │ shieldbase.com   │ ADMIN01$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1141 │ SHIELDBASE.COM   │ ADMIN01$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │           40 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1142 │ SHIELDBASE.COM   │ RD08$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1142 │ shieldbase.com   │ RD08$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1143 │ shieldbase.com   │ RD09$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.19 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1143 │ SHIELDBASE.COM   │ RD09$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.19 │                │                  │                │           43 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1144 │ shieldbase.com   │ RD10$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.20 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1144 │ SHIELDBASE.COM   │ RD10$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.20 │                │                  │                │           41 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1145 │ shieldbase.com   │ RD06$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.16 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1145 │ SHIELDBASE.COM   │ RD06$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.16 │                │                  │                │           56 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1146 │ SHIELDBASE.COM   │ RD02$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.12 │                │                  │                │           40 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1146 │ shieldbase.com   │ RD02$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.12 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1147 │ shieldbase.com   │ RD01$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           11 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1147 │ SHIELDBASE.COM   │ RD01$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           41 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1148 │ SHIELDBASE.COM   │ EXCHANGE01$          │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │           79 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1148 │ shieldbase.com   │ EXCHANGE01$          │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1149 │ shieldbase.com   │ FILE01$              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.5  │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1149 │ SHIELDBASE.COM   │ FILE01$              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.5  │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1150 │ shieldbase.com   │ WKSTN06$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.16 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1150 │ SHIELDBASE.COM   │ WKSTN06$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.16 │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1151 │ SHIELDBASE.COM   │ WKSTN05$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1151 │ shieldbase.com   │ WKSTN05$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1152 │ shieldbase.com   │ WKSTN04$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.14 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1152 │ SHIELDBASE.COM   │ WKSTN04$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.14 │                │                  │                │           42 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1153 │ shieldbase.com   │ RD04$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.14 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1153 │ SHIELDBASE.COM   │ RD04$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.14 │                │                  │                │           37 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1154 │ SHIELDBASE.COM   │ WKSTN08$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.18 │                │                  │                │           43 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1154 │ shieldbase.com   │ WKSTN08$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.18 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1155 │ shieldbase.com   │ WKSTN03$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.13 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1155 │ SHIELDBASE.COM   │ WKSTN03$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.13 │                │                  │                │           60 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1156 │ shieldbase.com   │ WKSTN09$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.19 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1156 │ SHIELDBASE.COM   │ WKSTN09$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.19 │                │                  │                │           40 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1157 │ shieldbase.com   │ WKSTN10$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.20 │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1157 │ SHIELDBASE.COM   │ WKSTN10$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.20 │                │                  │                │           47 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1158 │ shieldbase.com   │ WKSTN02$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.12 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1158 │ SHIELDBASE.COM   │ WKSTN02$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.12 │                │                  │                │           50 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1159 │ shieldbase.com   │ HUNT01$              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1159 │ SHIELDBASE.COM   │ HUNT01$              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │           41 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1160 │ SHIELDBASE.COM   │ RD07$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.17 │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1160 │ shieldbase.com   │ RD07$                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.17 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1161 │ SHIELDBASE.COM   │ WKSTN01$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1161 │ shieldbase.com   │ WKSTN01$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1162 │ shieldbase.com   │ WKSTN07$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.17 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1162 │ SHIELDBASE.COM   │ WKSTN07$             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.17 │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1194 │ SHIELDBASE.COM   │ HealthMailboxc8fd8db │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │         9986 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1195 │ SHIELDBASE.COM   │ HealthMailbox8d23eed │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1206 │ SHIELDBASE.COM   │ ELF01$               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.21 │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1206 │ shieldbase.com   │ ELF01$               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.5.21 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1212 │ SHIELDBASE.COM   │ scarter              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.17 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1212 │ shieldbase.com   │ scarter              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.17 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ SHIELDBASE.COM   │ slevine              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ shieldbase.com   │ slevine              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1214 │ SHIELDBASE.COM   │ WAC01$               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.7  │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1214 │ shieldbase.com   │ WAC01$               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.7  │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1215 │ shieldbase.com   │ SQL01$               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.8  │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1215 │ SHIELDBASE.COM   │ SQL01$               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.8  │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ SHIELDBASE.COM   │ DEV01$               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ shieldbase.com   │ DEV01$               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           11 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           14 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │            7 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 2           │ 0x12                 │ 0x40810010    │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            4 │
└─────────────────────┴────────────────────────────────────────────────┴──────────────────┴──────────────────────┴───────────────────────────────────────────────┴─────────────┴─────────────┴──────────────────────┴───────────────┴─────────┴────────────────────┴────────────────┴──────────────────┴────────────────┴──────────────┘
  124 rows                                                                                                                                                                                                                                                                                                                  15 columns
```
fullkey
```sql
security-dc01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-dc01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4768' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌────────────────────────────────────────┬──────────────┐
│                fullkey                 │ count_star() │
│                varchar                 │    int64     │
├────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.CertIssuerName       │        11962 │
│ $.Event.EventData.CertSerialNumber     │        11962 │
│ $.Event.EventData.CertThumbprint       │        11962 │
│ $.Event.EventData.IpAddress            │        11962 │
│ $.Event.EventData.IpPort               │        11962 │
│ $.Event.EventData.PreAuthType          │        11962 │
│ $.Event.EventData.ServiceName          │        11962 │
│ $.Event.EventData.ServiceSid           │        11962 │
│ $.Event.EventData.Status               │        11962 │
│ $.Event.EventData.TargetDomainName     │        11962 │
│ $.Event.EventData.TargetSid            │        11962 │
│ $.Event.EventData.TargetUserName       │        11962 │
│ $.Event.EventData.TicketEncryptionType │        11962 │
│ $.Event.EventData.TicketOptions        │        11962 │
└────────────────────────────────────────┴──────────────┘
  14 rows                                     2 columns
```
PreAuthType
```
0: Logon without Pre-Authentication.
2: PA-ENC-TIMESTAMP - This type is normal for standard password authentication.
11: PA-ETYPE-INFO - The ETYPE-INFO pre-authentication type is sent by the KDC in a KRB-ERROR indicating a requirement for additional pre-authentication. It is usually used to notify a client of which key to use for the encryption of an encrypted timestamp for the purposes of sending a PA-ENC-TIMESTAMP pre-authentication value. Never saw this Pre-Authentication Type in Microsoft Active Directory environment.
15: PA-PK-AS-REP_OLD - Used for Smart Card logon authentication.
16: PA-PK-AS-REQ - Request sent to KDC in Smart Card authentication scenarios.
17: PA-PK-AS-REP - This type should also be used for Smart Card authentication, but in certain Active Directory environments, it is never seen.
19: PA-ETYPE-INFO2 - The ETYPE-INFO2 pre-authentication type is sent by the KDC in a KRB-ERROR indicating a requirement for additional pre-authentication. It is usually used to notify a client of which key to use for the encryption of an encrypted timestamp for the purposes of sending a PA-ENC-TIMESTAMP pre-authentication value. Never saw this Pre-Authentication Type in Microsoft Active Directory environment.
20: PA-SVR-REFERRAL-INFO - Used in KDC Referrals tickets.
138: PA-ENCRYPTED-CHALLENGE - Logon using Kerberos Armoring (FAST). Supported starting from Windows Server 2012 domain controllers and Windows 8 clients.
-: This type shows in Audit Failure events.
```
TicketEncryptionType
```
0x1: DES-CBC-CRC
0x3: DES-CBC-MD5
0x11: AES128-CTS-HMAC-SHA1-96
0x12: AES256-CTS-HMAC-SHA1-96
0x17: RC4-HMAC
0x18: RC4-HMAC-EXP
0xFFFFFFFF: Audit Failure
0xffffffff: Audit Failure
```
Status
```
0x0: KDC_ERR_NONE - No error - No errors were found.
0x1: KDC_ERR_NAME_EXP - Client's entry in KDC database has expired - No information.
0x2: KDC_ERR_SERVICE_EXP - Server's entry in KDC database has expired - No information.
0x3: KDC_ERR_BAD_PVNO - Requested Kerberos version number not supported - No information.
0x4: KDC_ERR_C_OLD_MAST_KVNO - Client's key encrypted in old master key - No information.
0x5: KDC_ERR_S_OLD_MAST_KVNO - Server's key encrypted in old master key - No information.
0x6: KDC_ERR_C_PRINCIPAL_UNKNOWN - Client not found in Kerberos database - The username doesn't exist.
0x7: KDC_ERR_S_PRINCIPAL_UNKNOWN - Server not found in Kerberos database - This error can occur if the domain controller cannot find the server's name in Active Directory. This error is similar to KDC_ERR_C_PRINCIPAL_UNKNOWN except that it occurs when the server name cannot be found.
0x8: KDC_ERR_PRINCIPAL_NOT_UNIQUE - Multiple principal entries in KDC database - This error occurs if duplicate principal names exist. Unique principal names are crucial for ensuring mutual authentication. Thus, duplicate principal names are strictly forbidden, even across multiple realms. Without unique principal names, the client has no way of ensuring that the server it is communicating with is the correct one.
0x9: KDC_ERR_NULL_KEY - The client or server has a null key (master key) - No master key was found for client or server. Usually it means that administrator should reset the password on the account.
0xA: KDC_ERR_CANNOT_POSTDATE - Ticket (TGT) not eligible for postdating - This error can occur if a client requests postdating of a Kerberos ticket. Postdating is the act of requesting that a ticket's start time be set into the future. It also can occur if there is a time difference between the client and the KDC.
0xB: KDC_ERR_NEVER_VALID - Requested start time is later than end time - There is a time difference between the KDC and the client.
0xC: KDC_ERR_POLICY - Requested start time is later than end time - This error is usually the result of logon restrictions in place on a user's account. For example workstation restriction, smart card authentication requirement or logon time restriction.
0xD: KDC_ERR_BADOPTION - KDC cannot accommodate requested option - Impending expiration of a TGT. The SPN to which the client is attempting to delegate credentials is not in its Allowed-to-delegate-to list
0xE: KDC_ERR_ETYPE_NOTSUPP - KDC has no support for encryption type - In general, this error occurs when the KDC or a client receives a packet that it cannot decrypt.
0xF: KDC_ERR_SUMTYPE_NOSUPP - KDC has no support for checksum type - The KDC, server, or client receives a packet for which it does not have a key of the appropriate encryption type. The result is that the computer is unable to decrypt the ticket.
0x10: KDC_ERR_PADATA_TYPE_NOSUPP - KDC has no support for PADATA type (pre-authentication data) - Smart card logon is being attempted and the proper certificate cannot be located. This can happen because the wrong certification authority (CA) is being queried or the proper CA cannot be contacted. It can also happen when a domain controller doesn't have a certificate installed for smart cards (Domain Controller or Domain Controller Authentication templates). This error code cannot occur in event "4768. A Kerberos authentication ticket (TGT) was requested". It occurs in "4771. Kerberos pre-authentication failed" event.
0x11: KDC_ERR_TRTYPE_NO_SUPP - KDC has no support for transited type - No information.
0x12: KDC_ERR_CLIENT_REVOKED - Client's credentials have been revoked - This might be because of an explicit disabling or because of other restrictions in place on the account. For example, account disabled, expired, or locked out.
0x13: KDC_ERR_SERVICE_REVOKED - Credentials for server have been revoked - No information.
0x14: KDC_ERR_TGT_REVOKED - TGT has been revoked - Since the remote KDC may change its PKCROSS key while there are PKCROSS tickets still active, it SHOULD cache the old PKCROSS keys until the last issued PKCROSS ticket expires. Otherwise, the remote KDC will respond to a client with a KRB-ERROR message of type KDC_ERR_TGT_REVOKED. See RFC1510 for more details.
0x15: KDC_ERR_CLIENT_NOTYET - Client not yet valid-try again later - No information.
0x16: KDC_ERR_SERVICE_NOTYET - Server not yet valid-try again later - No information.
0x17: KDC_ERR_KEY_EXPIRED - Password has expired-change password to reset - The user's password has expired. This error code cannot occur in event "4768. A Kerberos authentication ticket (TGT) was requested". It occurs in "4771. Kerberos pre-authentication failed" event.
0x18: KDC_ERR_PREAUTH_FAILED - Pre-authentication information was invalid - The wrong password was provided. This error code cannot occur in event "4768. A Kerberos authentication ticket (TGT) was requested". It occurs in "4771. Kerberos pre-authentication failed" event.
0x19: KDC_ERR_PREAUTH_REQUIRED - Additional pre-authentication required - This error often occurs in UNIX interoperability scenarios. MIT-Kerberos clients do not request pre-authentication when they send a KRB_AS_REQ message. If pre-authentication is required (the default), Windows systems will send this error. Most MIT-Kerberos clients will respond to this error by giving the pre-authentication, in which case the error can be ignored, but some clients might not respond in this way.
0x1A: KDC_ERR_SERVER_NOMATCH - KDC does not know about the requested server - No information.
0x1B: KDC_ERR_MUST_USE_USER2USER - Server principal valid for user2user only - This error occurs because the service is missing an SPN.
0x1F: KRB_AP_ERR_BAD_INTEGRITY - Integrity check on decrypted field failed - The authenticator was encrypted with something other than the session key. The result is that the client cannot decrypt the resulting message. The modification of the message could be the result of an attack or it could be because of network noise.
0x20: KRB_AP_ERR_TKT_EXPIRED - The ticket has expired - The smaller the value for the "Maximum lifetime for user ticket" Kerberos policy setting, the more likely it is that this error will occur. Because ticket renewal is automatic, you should not have to do anything if you get this message.
0x21: KRB_AP_ERR_TKT_NYV - The ticket is not yet valid - The ticket presented to the server is not yet valid (in relationship to the server time). The most probable cause is that the clocks on the KDC and the client are not synchronized. If cross-realm Kerberos authentication is being attempted, then you should verify time synchronization between the KDC in the target realm and the KDC in the client realm, as well.
0x22: KRB_AP_ERR_REPEAT - The request is a replay - This error indicates that a specific authenticator showed up twice - the KDC has detected that this session ticket duplicates one that it has already received.
0x23: KRB_AP_ERR_NOT_US - The ticket is not for us - The server has received a ticket that was meant for a different realm.
0x24: KRB_AP_ERR_BADMATCH - The ticket and authenticator do not match - The KRB_TGS_REQ is being sent to the wrong KDC. There is an account mismatch during protocol transition.
0x25: KRB_AP_ERR_SKEW - The clock skew is too great - This error is logged if a client computer sends a timestamp whose value differs from that of the server's timestamp by more than the number of minutes found in the "Maximum tolerance for computer clock synchronization" setting in Kerberos policy.
0x26: KRB_AP_ERR_BADADDR - Network address in network layer header doesn't match address inside ticket - Session tickets MAY include the addresses from which they are valid. This error can occur if the address of the computer sending the ticket is different from the valid address in the ticket. A possible cause of this could be an Internet Protocol (IP) address change. Another possible cause is when a ticket is passed through a proxy server or NAT. The client is unaware of the address scheme used by the proxy server, so unless the program caused the client to request a proxy server ticket with the proxy server's source address, the ticket could be invalid.
0x27: KRB_AP_ERR_BADVERSION - Protocol version numbers don't match (PVNO) - When an application receives a KRB_SAFE message, it verifies it. If any error occurs, an error code is reported for use by the application. The message is first checked by verifying that the protocol version and type fields match the current version and KRB_SAFE, respectively. A mismatch generates a KRB_AP_ERR_BADVERSION. See RFC4120 for more details.
0x28: KRB_AP_ERR_MSG_TYPE - Message type is unsupported - This message is generated when target server finds that message format is wrong. This applies to KRB_AP_REQ, KRB_SAFE, KRB_PRIV and KRB_CRED messages. This error also generated if use of UDP protocol is being attempted with User-to-User authentication.
0x29: KRB_AP_ERR_MODIFIED - Message stream modified and checksum didn't match - The authentication data was encrypted with the wrong key for the intended server. The authentication data was modified in transit by a hardware or software error, or by an attacker. The client sent the authentication data to the wrong server because incorrect DNS data caused the client to send the request to the wrong server. The client sent the authentication data to the wrong server because DNS data was out-of-date on the client.
0x2A: KRB_AP_ERR_BADORDER - Message out of order (possible tampering) - This event generates for KRB_SAFE and KRB_PRIV messages if an incorrect sequence number is included, or if a sequence number is expected but not present. See RFC4120 for more details.
0x2C: KRB_AP_ERR_BADKEYVER - Specified version of key is not available - This error might be generated on server side during receipt of invalid KRB_AP_REQ message. If the key version indicated by the Ticket in the KRB_AP_REQ is not one the server can use (e.g., it indicates an old key, and the server no longer possesses a copy of the old key), the KRB_AP_ERR_BADKEYVER error is returned.
0x2D: KRB_AP_ERR_NOKEY - Service key not available - This error might be generated on server side during receipt of invalid KRB_AP_REQ message. Because it is possible for the server to be registered in multiple realms, with different keys in each, the realm field in the unencrypted portion of the ticket in the KRB_AP_REQ is used to specify which secret key the server should use to decrypt that ticket. The KRB_AP_ERR_NOKEY error code is returned if the server doesn't have the proper key to decipher the ticket.
0x2E: KRB_AP_ERR_MUT_FAIL - Mutual authentication failed - No information.
0x2F: KRB_AP_ERR_BADDIRECTION - Incorrect message direction - No information.
0x30: KRB_AP_ERR_METHOD - Alternative authentication method required - According RFC4120 this error message is obsolete.
0x31: KRB_AP_ERR_BADSEQ - Incorrect sequence number in message - No information.
0x32: KRB_AP_ERR_INAPP_CKSUM - Inappropriate type of checksum in message (checksum may be unsupported) - When KDC receives KRB_TGS_REQ message it decrypts it, and after the user-supplied checksum in the Authenticator MUST be verified against the contents of the request, and the message MUST be rejected if the checksums do not match (with an error code of KRB_AP_ERR_MODIFIED) or if the checksum is not collision-proof (with an error code of KRB_AP_ERR_INAPP_CKSUM).
0x33: KRB_AP_PATH_NOT_ACCEPTED - Desired path is unreachable - No information.
0x34: KRB_ERR_RESPONSE_TOO_BIG - Too much data. The size of a ticket is too large to be transmitted reliably via UDP. In a Windows environment, this message is purely informational. A computer running a Windows operating system will automatically try TCP if UDP fails.
0x3C: KRB_ERR_GENERIC - Generic error - Group membership has overloaded the PAC. Multiple recent password changes have not propagated. Crypto subsystem error caused by running out of memory. SPN too long. SPN has too many parts.
0x3D: KRB_ERR_FIELD_TOOLONG - Field is too long for this implementation - Each request (KRB_KDC_REQ) and response (KRB_KDC_REP or KRB_ERROR) sent over the TCP stream is preceded by the length of the request as 4 octets in network byte order. The high bit of the length is reserved for future expansion and MUST currently be set to zero. If a KDC that does not understand how to interpret a set high bit of the length encoding receives a request with the high order bit of the length set, it MUST return a KRB-ERROR message with the error KRB_ERR_FIELD_TOOLONG and MUST close the TCP stream.
0x3E: KDC_ERR_CLIENT_NOT_TRUSTED - The client trust failed or is not implemented - This typically happens when user's smart-card certificate is revoked or the root Certification Authority that issued the smart card certificate (in a chain) is not trusted by the domain controller.
0x3F: KDC_ERR_KDC_NOT_TRUSTED - The KDC server trust failed or could not be verified - The trustedCertifiers field contains a list of certification authorities trusted by the client, in the case that the client does not possess the KDC's public key certificate. If the KDC has no certificate signed by any of the trustedCertifiers, then it returns an error of type KDC_ERR_KDC_NOT_TRUSTED. See RFC1510 for more details.
0x40: KDC_ERR_INVALID_SIG - The signature is invalid - This error is related to PKINIT. If a PKI trust relationship exists, the KDC then verifies the client's signature on AuthPack (TGT request signature). If that fails, the KDC returns an error message of type KDC_ERR_INVALID_SIG.
0x41: KDC_ERR_KEY_TOO_WEAK - A higher encryption level is needed - If the clientPublicValue field is filled in, indicating that the client wishes to use Diffie-Hellman key agreement, then the KDC checks to see that the parameters satisfy its policy. If they do not (e.g., the prime size is insufficient for the expected encryption type), then the KDC sends back an error message of type KDC_ERR_KEY_TOO_WEAK.
0x42: KRB_AP_ERR_USER_TO_USER_REQUIRED - User-to-user authorization is required - In the case that the client application doesn't know that a service requires user-to-user authentication, and requests and receives a conventional KRB_AP_REP, the client will send the KRB_AP_REP request, and the server will respond with a KRB_ERROR token as described in RFC1964, with a msg-type of KRB_AP_ERR_USER_TO_USER_REQUIRED.
0x43: KRB_AP_ERR_NO_TGT - No TGT was presented or available - In user-to-user authentication if the service does not possess a ticket granting ticket, it should return the error KRB_AP_ERR_NO_TGT.
0x44: KDC_ERR_WRONG_REALM - Incorrect domain or principal - Although this error rarely occurs, it occurs when a client presents a cross-realm TGT to a realm other than the one specified in the TGT. Typically, this results from incorrectly configured DNS.
```
TicketOptions  
https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4768



## 4769 (A Kerberos service ticket was requested)
```sql
security-dc01 D SELECT Event.System.Computer, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.ServiceSid, Event.EventData.ServiceName, Event.EventData.TicketOptions, Event.EventData.TicketEncryptionType, Event.EventData.Status, Event.EventData.IpAddress, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND json_extract_string(Event.System.EventID, '$') = '4769'
                AND Event.EventData.TargetUserName = 'tdungan@SHIELDBASE.COM'
                GROUP BY ALL
                ORDER BY Event.EventData.TargetUserName, Event.EventData.ServiceSid, Event.EventData.IpAddress;
┌─────────────────────┬──────────────────┬────────────────────────┬────────────────────────────────────────────────┬─────────────┬───────────────┬──────────────────────┬─────────┬────────────────────┬──────────────┐
│      Computer       │ TargetDomainName │     TargetUserName     │                   ServiceSid                   │ ServiceName │ TicketOptions │ TicketEncryptionType │ Status  │     IpAddress      │ count_star() │
│       varchar       │     varchar      │        varchar         │                    varchar                     │   varchar   │    varchar    │       varchar        │ varchar │      varchar       │    int64     │
├─────────────────────┼──────────────────┼────────────────────────┼────────────────────────────────────────────────┼─────────────┼───────────────┼──────────────────────┼─────────┼────────────────────┼──────────────┤
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ tdungan@SHIELDBASE.COM │ S-1-5-21-2838623409-1327563992-2591358621-1001 │ DC01$       │ 0x40800000    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │           55 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ tdungan@SHIELDBASE.COM │ S-1-5-21-2838623409-1327563992-2591358621-1001 │ DC01$       │ 0x40810000    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │           33 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ tdungan@SHIELDBASE.COM │ S-1-5-21-2838623409-1327563992-2591358621-1147 │ RD01$       │ 0x40810000    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │           11 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ tdungan@SHIELDBASE.COM │ S-1-5-21-2838623409-1327563992-2591358621-1148 │ EXCHANGE01$ │ 0x40810000    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │           27 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ tdungan@SHIELDBASE.COM │ S-1-5-21-2838623409-1327563992-2591358621-1149 │ FILE01$     │ 0x40810000    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │           20 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ tdungan@SHIELDBASE.COM │ S-1-5-21-2838623409-1327563992-2591358621-1161 │ WKSTN01$    │ 0x40810000    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │            1 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ tdungan@SHIELDBASE.COM │ S-1-5-21-2838623409-1327563992-2591358621-502  │ krbtgt      │ 0x60810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │           28 │
└─────────────────────┴──────────────────┴────────────────────────┴────────────────────────────────────────────────┴─────────────┴───────────────┴──────────────────────┴─────────┴────────────────────┴──────────────┘
```
fullkey
```sql
security-dc01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-dc01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4769' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌────────────────────────────────────────┬──────────────┐
│                fullkey                 │ count_star() │
│                varchar                 │    int64     │
├────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.IpAddress            │        27488 │
│ $.Event.EventData.IpPort               │        27488 │
│ $.Event.EventData.LogonGuid            │        27488 │
│ $.Event.EventData.ServiceName          │        27488 │
│ $.Event.EventData.ServiceSid           │        27488 │
│ $.Event.EventData.Status               │        27488 │
│ $.Event.EventData.TargetDomainName     │        27488 │
│ $.Event.EventData.TargetUserName       │        27488 │
│ $.Event.EventData.TicketEncryptionType │        27488 │
│ $.Event.EventData.TicketOptions        │        27488 │
│ $.Event.EventData.TransmittedServices  │        27488 │
└────────────────────────────────────────┴──────────────┘
  11 rows                                     2 columns
```



## 4770 (A Kerberos service ticket was renewed)
```sql
security-dc01 D SELECT Event.System.Computer, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.ServiceSid, Event.EventData.ServiceName, Event.EventData.TicketOptions, Event.EventData.TicketEncryptionType, Event.EventData.IpAddress, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND json_extract_string(Event.System.EventID, '$') = '4770'
                GROUP BY ALL
                ORDER BY Event.EventData.TargetUserName, Event.EventData.ServiceSid, Event.EventData.IpAddress;
┌─────────────────────┬──────────────────┬─────────────────────────────────────┬───────────────────────────────────────────────┬─────────────┬───────────────┬──────────────────────┬────────────────────┬──────────────┐
│      Computer       │ TargetDomainName │           TargetUserName            │                  ServiceSid                   │ ServiceName │ TicketOptions │ TicketEncryptionType │     IpAddress      │ count_star() │
│       varchar       │     varchar      │               varchar               │                    varchar                    │   varchar   │    varchar    │       varchar        │      varchar       │    int64     │
├─────────────────────┼──────────────────┼─────────────────────────────────────┼───────────────────────────────────────────────┼─────────────┼───────────────┼──────────────────────┼────────────────────┼──────────────┤
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ ADMIN01$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::1                │            1 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ ADMIN01$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.5.26 │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ DC01$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::1                │          101 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ DEV01$@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.9  │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ ELF01$@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.5.21 │           53 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ EXCHANGE01$@SHIELDBASE.COM          │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.6  │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ FILE01$@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.5  │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ HUNT01$@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.5.25 │           51 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ HealthMailbox8d23eed@SHIELDBASE.COM │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.6  │           42 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ Jblake@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.13 │           26 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD01$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.11 │           51 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD02$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.12 │           51 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD03$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.13 │           51 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD04$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.14 │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD05$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.15 │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD06$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.16 │           50 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD07$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.17 │           55 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD08$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.18 │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD09$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.19 │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ RD10$@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.20 │           51 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ SQL01$@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.8  │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WAC01$@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.7  │           51 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN01$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.11 │           53 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN02$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.12 │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN03$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.13 │           51 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN04$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.14 │           51 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN05$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.15 │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN06$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.16 │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN07$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::1                │            1 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN07$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.17 │           52 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN08$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.18 │           51 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN09$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.19 │           50 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ WKSTN10$@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.20 │           50 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ bnicholson@SHIELDBASE.COM           │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.12 │           26 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ cbarton-a@SHIELDBASE.COM            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::1                │            1 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ cbarton-a@SHIELDBASE.COM            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.5.25 │            4 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ cbarton@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.5.25 │           48 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ czelman@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.13 │           23 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ dwatson@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.19 │           25 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ jpallen@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.12 │           26 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ jzoch@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.17 │           26 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ kespinoza@SHIELDBASE.COM            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.16 │           25 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ mhill@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.11 │            8 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ nfury@SHIELDBASE.COM                │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.15 │           13 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ nromanoff@SHIELDBASE.COM            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.14 │           23 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ rdodson@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.15 │           26 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ rsydow-a@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::1                │           48 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ rsydow-a@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.6  │           10 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ rsydow-a@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.7  │           50 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ rsydow-a@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.9  │           24 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ rsydow-a@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.5.26 │           72 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ rsydow-a@SHIELDBASE.COM             │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.18 │           11 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ rsydow@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.5.26 │           46 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ scarter@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.17 │           28 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ slevine@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.18 │           16 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ smckee@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.20 │           26 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ sreese@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.18 │           26 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ srogers@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.19 │           46 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ tanderson@SHIELDBASE.COM            │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.20 │           25 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ tdungan@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.11 │           18 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ toslund@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.16 │           26 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ wacsvc@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::1                │           17 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ wacsvc@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.6  │           60 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ wacsvc@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.4.9  │           27 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ wacsvc@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.11 │           38 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ wacsvc@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.6.18 │           60 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ wacsvc@SHIELDBASE.COM               │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.15 │           34 │
│ dc01.shieldbase.com │ SHIELDBASE.COM   │ zdorsey@SHIELDBASE.COM              │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ 0x10002       │ 0x12                 │ ::ffff:172.16.7.14 │           25 │
└─────────────────────┴──────────────────┴─────────────────────────────────────┴───────────────────────────────────────────────┴─────────────┴───────────────┴──────────────────────┴────────────────────┴──────────────┘
  68 rows                                                                                                                                                                                                     9 columns
```
fullkey
```sql
security-dc01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-dc01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4770' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌────────────────────────────────────────┬──────────────┐
│                fullkey                 │ count_star() │
│                varchar                 │    int64     │
├────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.IpAddress            │         2624 │
│ $.Event.EventData.IpPort               │         2624 │
│ $.Event.EventData.ServiceName          │         2624 │
│ $.Event.EventData.ServiceSid           │         2624 │
│ $.Event.EventData.TargetDomainName     │         2624 │
│ $.Event.EventData.TargetUserName       │         2624 │
│ $.Event.EventData.TicketEncryptionType │         2624 │
│ $.Event.EventData.TicketOptions        │         2624 │
└────────────────────────────────────────┴──────────────┘
```



## 4771 (Kerberos pre-authentication failed)
```sql
security-dc01 D SELECT Event.System.Computer, Event.EventData.TargetSid, Event.EventData.TargetUserName, Event.EventData.ServiceName, Event.EventData.PreAuthType, Event.EventData.TicketOptions, Event.EventData.Status, Event.EventData.IpAddress, Event.EventData.CertIssuerName, Event.EventData.CertSerialNumber, Event.EventData.CertThumbprint, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND json_extract_string(Event.System.EventID, '$') = '4771'
                GROUP BY ALL
                ORDER BY Event.EventData.TargetUserName, Event.EventData.ServiceName, Event.EventData.IpAddress;
┌─────────────────────┬────────────────────────────────────────────────┬────────────────┬───────────────────┬─────────────┬───────────────┬─────────┬────────────────────┬────────────────┬──────────────────┬────────────────┬──────────────┐
│      Computer       │                   TargetSid                    │ TargetUserName │    ServiceName    │ PreAuthType │ TicketOptions │ Status  │     IpAddress      │ CertIssuerName │ CertSerialNumber │ CertThumbprint │ count_star() │
│       varchar       │                    varchar                     │    varchar     │      varchar      │   varchar   │    varchar    │ varchar │      varchar       │    varchar     │     varchar      │    varchar     │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼────────────────┼───────────────────┼─────────────┼───────────────┼─────────┼────────────────────┼────────────────┼──────────────────┼────────────────┼──────────────┤
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ cbarton-a      │ krbtgt/shieldbase │ 2           │ 0x40810010    │ 0x18    │ ::ffff:172.16.5.25 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1124 │ rsydow         │ krbtgt/shieldbase │ 2           │ 0x40810010    │ 0x18    │ ::ffff:172.16.4.6  │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ rsydow-a       │ krbtgt/shieldbase │ 2           │ 0x40810010    │ 0x18    │ ::ffff:172.16.5.26 │                │                  │                │            1 │
└─────────────────────┴────────────────────────────────────────────────┴────────────────┴───────────────────┴─────────────┴───────────────┴─────────┴────────────────────┴────────────────┴──────────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security-dc01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-dc01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4771' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.CertIssuerName   │            4 │
│ $.Event.EventData.CertSerialNumber │            4 │
│ $.Event.EventData.CertThumbprint   │            4 │
│ $.Event.EventData.IpAddress        │            4 │
│ $.Event.EventData.IpPort           │            4 │
│ $.Event.EventData.PreAuthType      │            4 │
│ $.Event.EventData.ServiceName      │            4 │
│ $.Event.EventData.Status           │            4 │
│ $.Event.EventData.TargetSid        │            4 │
│ $.Event.EventData.TargetUserName   │            4 │
│ $.Event.EventData.TicketOptions    │            4 │
└────────────────────────────────────┴──────────────┘
  11 rows                                 2 columns
```



## 4772 (A Kerberos authentication ticket request failed)



## 4773 (A Kerberos service ticket request failed)



## 4774 (An account was mapped for logon)



## 4775 (An account could not be mapped for logon)



## 4776 (NTLM authentication request)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.Workstation, Event.EventData.TargetUserName, Event.EventData.Status, Event.EventData.PackageName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4776
                GROUP BY ALL
                ORDER BY Event.EventData.TargetUserName;
┌─────────────────────┬─────────────┬────────────────┬────────────┬───────────────────────────────────────┬──────────────┐
│      Computer       │ Workstation │ TargetUserName │   Status   │              PackageName              │ count_star() │
│       varchar       │   varchar   │    varchar     │  varchar   │                varchar                │    int64     │
├─────────────────────┼─────────────┼────────────────┼────────────┼───────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ RD01        │ sprx           │ 0xc0000064 │ MICROSOFT_AUTHENTICATION_PACKAGE_V1_0 │           14 │
└─────────────────────┴─────────────┴────────────────┴────────────┴───────────────────────────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4776' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌──────────────────────────────────┬──────────────┐
│             fullkey              │ count_star() │
│             varchar              │    int64     │
├──────────────────────────────────┼──────────────┤
│ $.Event.EventData.PackageName    │           16 │
│ $.Event.EventData.Status         │           16 │
│ $.Event.EventData.TargetUserName │           16 │
│ $.Event.EventData.Workstation    │           16 │
└──────────────────────────────────┴──────────────┘
```
Status
```
0xC0000064: user name does not exist
0xC000006A: user name is correct but the password is wrong
0XC000006D: the cause is either a bad username or authentication information
0xC000006F: user logon outside authorized hours
0xC0000070: user logon from unauthorized workstation
0xC0000071: user logon with expired password
0xC0000072: user logon to account disabled by administrator
0xC0000193: user logon with expired account
0XC0000224: user is required to change password at next logon
0xC0000234: user is currently locked out
0xC0000371: local account store does not contain secret material for the specified accounts
0x0: Status OK
```



## 4777 (The domain controller failed to validate the credentials for an account)



## 4778 (RDP reconnecting)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.AccountDomain, Event.EventData.AccountName, Event.EventData.ClientName, Event.EventData.ClientAddress, Event.EventData.SessionName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4778
                GROUP BY ALL
                ORDER BY Event.EventData.AccountName, Event.EventData.ClientAddress;
┌─────────────────────┬───────────────┬─────────────┬────────────┬───────────────┬─────────────┬──────────────┐
│      Computer       │ AccountDomain │ AccountName │ ClientName │ ClientAddress │ SessionName │ count_star() │
│       varchar       │    varchar    │   varchar   │  varchar   │    varchar    │   varchar   │    int64     │
├─────────────────────┼───────────────┼─────────────┼────────────┼───────────────┼─────────────┼──────────────┤
│ rd01.shieldbase.com │ shieldbase    │ tdungan     │ DUNGANATOR │ 172.16.30.14  │ RDP-Tcp#0   │            2 │
│ rd01.shieldbase.com │ shieldbase    │ tdungan     │ DUNGANATOR │ 172.16.30.23  │ RDP-Tcp#0   │            1 │
│ rd01.shieldbase.com │ shieldbase    │ tdungan     │ DUNGANATOR │ 172.16.30.3   │ RDP-Tcp#0   │            1 │
│ rd01.shieldbase.com │ shieldbase    │ tdungan     │ DUNGANATOR │ 172.16.30.8   │ RDP-Tcp#0   │            1 │
│ rd01.shieldbase.com │ shieldbase    │ wacsvc      │ phoenix    │ 172.16.6.18   │ RDP-Tcp#1   │            7 │
└─────────────────────┴───────────────┴─────────────┴────────────┴───────────────┴─────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4778' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────┬──────────────┐
│             fullkey             │ count_star() │
│             varchar             │    int64     │
├─────────────────────────────────┼──────────────┤
│ $.Event.EventData.AccountDomain │           35 │
│ $.Event.EventData.AccountName   │           35 │
│ $.Event.EventData.ClientAddress │           35 │
│ $.Event.EventData.ClientName    │           35 │
│ $.Event.EventData.LogonID       │           35 │
│ $.Event.EventData.SessionName   │           35 │
└─────────────────────────────────┴──────────────┘
```



## 4779 (RDP disconnecting)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.AccountDomain, Event.EventData.AccountName, Event.EventData.ClientName, Event.EventData.ClientAddress, Event.EventData.SessionName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4779
                GROUP BY ALL
                ORDER BY Event.EventData.AccountName, Event.EventData.ClientAddress;
┌─────────────────────┬───────────────┬─────────────┬────────────┬───────────────┬─────────────┬──────────────┐
│      Computer       │ AccountDomain │ AccountName │ ClientName │ ClientAddress │ SessionName │ count_star() │
│       varchar       │    varchar    │   varchar   │  varchar   │    varchar    │   varchar   │    int64     │
├─────────────────────┼───────────────┼─────────────┼────────────┼───────────────┼─────────────┼──────────────┤
│ rd01.shieldbase.com │ shieldbase    │ tdungan     │ DUNGANATOR │ 172.16.30.14  │ RDP-Tcp#0   │            1 │
│ rd01.shieldbase.com │ shieldbase    │ tdungan     │ DUNGANATOR │ 172.16.30.3   │ RDP-Tcp#0   │            2 │
│ rd01.shieldbase.com │ shieldbase    │ tdungan     │ DUNGANATOR │ 172.16.30.8   │ RDP-Tcp#0   │            2 │
│ rd01.shieldbase.com │ shieldbase    │ wacsvc      │ phoenix    │ 172.16.6.18   │ RDP-Tcp#1   │            8 │
└─────────────────────┴───────────────┴─────────────┴────────────┴───────────────┴─────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4779' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────┬──────────────┐
│             fullkey             │ count_star() │
│             varchar             │    int64     │
├─────────────────────────────────┼──────────────┤
│ $.Event.EventData.AccountDomain │           37 │
│ $.Event.EventData.AccountName   │           37 │
│ $.Event.EventData.ClientAddress │           37 │
│ $.Event.EventData.ClientName    │           37 │
│ $.Event.EventData.LogonID       │           37 │
│ $.Event.EventData.SessionName   │           37 │
└─────────────────────────────────┴──────────────┘
```



## 4781 (The name of an account was changed)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.OldTargetUserName, Event.EventData.NewTargetUserName, Event.EventData.PrivilegeList, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.EventID = 4781
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌────────────┬────────────────┬───────────────────┬─────────────────┬──────────────────────────────────────────────┬──────────────────┬─────────────────────────────────────┬─────────────────────────────────────┬───────────────┬──────────────┐
│  Computer  │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                  TargetSid                   │ TargetDomainName │          OldTargetUserName          │          NewTargetUserName          │ PrivilegeList │ count_star() │
│  varchar   │    varchar     │      varchar      │     varchar     │                   varchar                    │     varchar      │               varchar               │               varchar               │    varchar    │    int64     │
├────────────┼────────────────┼───────────────────┼─────────────────┼──────────────────────────────────────────────┼──────────────────┼─────────────────────────────────────┼─────────────────────────────────────┼───────────────┼──────────────┤
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-21-2908624845-2463485410-257172065-513 │ TPL-PACKER       │ None                                │ None                                │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-544                                 │ Builtin          │ Administrators                      │ Administrators                      │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-545                                 │ Builtin          │ Users                               │ Users                               │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-546                                 │ Builtin          │ Guests                              │ Guests                              │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-547                                 │ Builtin          │ Power Users                         │ Power Users                         │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-551                                 │ Builtin          │ Backup Operators                    │ Backup Operators                    │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-552                                 │ Builtin          │ Replicator                          │ Replicator                          │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-555                                 │ Builtin          │ Remote Desktop Users                │ Remote Desktop Users                │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-556                                 │ Builtin          │ Network Configuration Operators     │ Network Configuration Operators     │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-558                                 │ Builtin          │ Performance Monitor Users           │ Performance Monitor Users           │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-559                                 │ Builtin          │ Performance Log Users               │ Performance Log Users               │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-562                                 │ Builtin          │ Distributed COM Users               │ Distributed COM Users               │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-568                                 │ Builtin          │ IIS_IUSRS                           │ IIS_IUSRS                           │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-569                                 │ Builtin          │ Cryptographic Operators             │ Cryptographic Operators             │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-573                                 │ Builtin          │ Event Log Readers                   │ Event Log Readers                   │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-578                                 │ Builtin          │ Hyper-V Administrators              │ Hyper-V Administrators              │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-579                                 │ Builtin          │ Access Control Assistance Operators │ Access Control Assistance Operators │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-580                                 │ Builtin          │ Remote Management Users             │ Remote Management Users             │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-581                                 │ Builtin          │ System Managed Accounts Group       │ System Managed Accounts Group       │ -             │            1 │
│ tpl-packer │ S-1-5-18       │ WORKGROUP         │ TPL-PACKER$     │ S-1-5-32-583                                 │ Builtin          │ Device Owners                       │ Device Owners                       │ -             │            1 │
└────────────┴────────────────┴───────────────────┴─────────────────┴──────────────────────────────────────────────┴──────────────────┴─────────────────────────────────────┴─────────────────────────────────────┴───────────────┴──────────────┘
  20 rows                                                                                                                                                                                                                             10 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4781' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.NewTargetUserName │           20 │
│ $.Event.EventData.OldTargetUserName │           20 │
│ $.Event.EventData.PrivilegeList     │           20 │
│ $.Event.EventData.SubjectDomainName │           20 │
│ $.Event.EventData.SubjectLogonId    │           20 │
│ $.Event.EventData.SubjectUserName   │           20 │
│ $.Event.EventData.SubjectUserSid    │           20 │
│ $.Event.EventData.TargetDomainName  │           20 │
│ $.Event.EventData.TargetSid         │           20 │
└─────────────────────────────────────┴──────────────┘
```



## 4782 (The password hash of an account was accessed)



## 4793 (The Password Policy Checking API was called)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserName, Event.EventData.Workstation, Event.EventData.Status, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4793
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetUserName;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬────────────────┬─────────────┬─────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │ TargetUserName │ Workstation │ Status  │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │    varchar     │   varchar   │ varchar │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼────────────────┼─────────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ SRLAdmin       │ RD01        │ 0x0     │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴────────────────┴─────────────┴─────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4793' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.Status            │            1 │
│ $.Event.EventData.SubjectDomainName │            1 │
│ $.Event.EventData.SubjectLogonId    │            1 │
│ $.Event.EventData.SubjectUserName   │            1 │
│ $.Event.EventData.SubjectUserSid    │            1 │
│ $.Event.EventData.TargetUserName    │            1 │
│ $.Event.EventData.Workstation       │            1 │
└─────────────────────────────────────┴──────────────┘
```



## 4797 (An attempt was made to query the existence of a blank password for an account)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.Workstation, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4797
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetUserName;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬──────────────────┬────────────────────┬─────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │ TargetDomainName │   TargetUserName   │ Workstation │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │     varchar      │      varchar       │   varchar   │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────────┼─────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ RD01             │ Administrator      │ RD01        │           12 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ RD01             │ DefaultAccount     │ RD01        │           12 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ RD01             │ Guest              │ RD01        │           12 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ RD01             │ SRLAdmin           │ RD01        │           12 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ RD01             │ WDAGUtilityAccount │ RD01        │           12 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ RD01             │ Administrator      │ RD01        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ RD01             │ DefaultAccount     │ RD01        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ RD01             │ Guest              │ RD01        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ RD01             │ SRLAdmin           │ RD01        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ RD01             │ WDAGUtilityAccount │ RD01        │            2 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴──────────────────┴────────────────────┴─────────────┴──────────────┘
  10 rows                                                                                                                                                               8 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4797' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.SubjectDomainName │          105 │
│ $.Event.EventData.SubjectLogonId    │          105 │
│ $.Event.EventData.SubjectUserName   │          105 │
│ $.Event.EventData.SubjectUserSid    │          105 │
│ $.Event.EventData.TargetDomainName  │          105 │
│ $.Event.EventData.TargetUserName    │          105 │
│ $.Event.EventData.Workstation       │          105 │
└─────────────────────────────────────┴──────────────┘
```



## 4798 (A user's local group membership was enumerated)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.CallerProcessName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4798
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬───────────────────────────────────────────────┬──────────────────┬────────────────────┬─────────────────────────────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                   TargetSid                   │ TargetDomainName │   TargetUserName   │            CallerProcessName            │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │                    varchar                    │     varchar      │      varchar       │                 varchar                 │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼───────────────────────────────────────────────┼──────────────────┼────────────────────┼─────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ RD01             │ SRLAdmin           │ C:\Windows\System32\svchost.exe         │           18 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ RD01             │ SRLAdmin           │ C:\Windows\System32\wbem\WmiPrvSE.exe   │            6 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ RD01             │ SRLAdmin           │ C:\Windows\System32\CompatTelRunner.exe │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-500  │ RD01             │ Administrator      │ C:\Windows\System32\CompatTelRunner.exe │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-500  │ RD01             │ Administrator      │ C:\Windows\System32\svchost.exe         │           18 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-500  │ RD01             │ Administrator      │ C:\Windows\System32\wbem\WmiPrvSE.exe   │            6 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-501  │ RD01             │ Guest              │ C:\Windows\System32\CompatTelRunner.exe │           17 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-501  │ RD01             │ Guest              │ C:\Windows\System32\svchost.exe         │           18 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-501  │ RD01             │ Guest              │ C:\Windows\System32\wbem\WmiPrvSE.exe   │            6 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-503  │ RD01             │ DefaultAccount     │ C:\Windows\System32\CompatTelRunner.exe │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-503  │ RD01             │ DefaultAccount     │ C:\Windows\System32\svchost.exe         │           18 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-503  │ RD01             │ DefaultAccount     │ C:\Windows\System32\wbem\WmiPrvSE.exe   │            6 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-504  │ RD01             │ WDAGUtilityAccount │ C:\Windows\System32\svchost.exe         │           18 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-504  │ RD01             │ WDAGUtilityAccount │ C:\Windows\System32\wbem\WmiPrvSE.exe   │            6 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-504  │ RD01             │ WDAGUtilityAccount │ C:\Windows\System32\CompatTelRunner.exe │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴───────────────────────────────────────────────┴──────────────────┴────────────────────┴─────────────────────────────────────────┴──────────────┘
  15 rows                                                                                                                                                                                                           9 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4798' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.CallerProcessId   │          409 │
│ $.Event.EventData.CallerProcessName │          409 │
│ $.Event.EventData.SubjectDomainName │          409 │
│ $.Event.EventData.SubjectLogonId    │          409 │
│ $.Event.EventData.SubjectUserName   │          409 │
│ $.Event.EventData.SubjectUserSid    │          409 │
│ $.Event.EventData.TargetDomainName  │          409 │
│ $.Event.EventData.TargetSid         │          409 │
│ $.Event.EventData.TargetUserName    │          409 │
└─────────────────────────────────────┴──────────────┘
```



## 4799 (A security-enabled local group membership was enumerated)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.TargetDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.CallerProcessName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4799
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetSid;
┌─────────────────────┬────────────────────────────────────────────────┬──────────────────┬─────────────────┬──────────────┬──────────────────┬─────────────────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ TargetDomainName │ SubjectUserName │  TargetSid   │ TargetDomainName │     TargetUserName      │                                                    CallerProcessName                                                     │ count_star() │
│       varchar       │                    varchar                     │     varchar      │     varchar     │   varchar    │     varchar      │         varchar         │                                                         varchar                                                          │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼──────────────────┼─────────────────┼──────────────┼──────────────────┼─────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\VSSVC.exe                                                                                            │          226 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\SrTasks.exe                                                                                          │          150 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\svchost.exe                                                                                          │           57 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\CompatTelRunner.exe                                                                                  │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\services.exe                                                                                         │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\consent.exe                                                                                          │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\SearchIndexer.exe                                                                                    │            6 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-545 │ Builtin          │ Users                   │ C:\Program Files (x86)\Microsoft\EdgeUpdate\Install\{8CA2A3C8-C01F-485B-8EDB-4C85DAAF8640}\EDGEMITMP_206C7.tmp\setup.exe │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-545 │ Builtin          │ Users                   │ C:\Program Files (x86)\Microsoft\EdgeUpdate\Install\{BC9BFFFC-52C5-4D1D-8C5F-3D0DC621C3F4}\EDGEMITMP_AB5AB.tmp\setup.exe │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-545 │ Builtin          │ Users                   │ C:\Program Files (x86)\Microsoft\EdgeUpdate\Install\{22B5026D-75BC-47A1-86CE-339A587518F8}\EDGEMITMP_22B8E.tmp\setup.exe │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-545 │ Builtin          │ Users                   │ C:\Windows\System32\CompatTelRunner.exe                                                                                  │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-546 │ Builtin          │ Guests                  │ C:\Windows\System32\CompatTelRunner.exe                                                                                  │            1 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-551 │ Builtin          │ Backup Operators        │ C:\Windows\System32\VSSVC.exe                                                                                            │          226 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-551 │ Builtin          │ Backup Operators        │ C:\Windows\System32\SrTasks.exe                                                                                          │          150 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-551 │ Builtin          │ Backup Operators        │ C:\Windows\System32\svchost.exe                                                                                          │            9 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-551 │ Builtin          │ Backup Operators        │ C:\Windows\System32\SearchIndexer.exe                                                                                    │            6 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ Builtin          │ RD01$           │ S-1-5-32-573 │ Builtin          │ Event Log Readers       │ C:\Windows\System32\services.exe                                                                                         │           35 │
│ rd01.shieldbase.com │ S-1-5-20                                       │ Builtin          │ RD01$           │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\svchost.exe                                                                                          │            4 │
│ rd01.shieldbase.com │ S-1-5-20                                       │ Builtin          │ RD01$           │ S-1-5-32-551 │ Builtin          │ Backup Operators        │ C:\Windows\System32\svchost.exe                                                                                          │            4 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ Builtin          │ rsydow-a        │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\net1.exe                                                                                             │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ Builtin          │ rsydow-a        │ S-1-5-32-544 │ Builtin          │ Administrators          │ -                                                                                                                        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ Builtin          │ rsydow-a        │ S-1-5-32-555 │ Builtin          │ Remote Desktop Users    │ -                                                                                                                        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ Builtin          │ rsydow-a        │ S-1-5-32-562 │ Builtin          │ Distributed COM Users   │ -                                                                                                                        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ Builtin          │ rsydow-a        │ S-1-5-32-580 │ Builtin          │ Remote Management Users │ -                                                                                                                        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ Builtin          │ wacsvc          │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\svchost.exe                                                                                          │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ Builtin          │ wacsvc          │ S-1-5-32-544 │ Builtin          │ Administrators          │ C:\Windows\System32\dllhost.exe                                                                                          │           67 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ Builtin          │ wacsvc          │ S-1-5-32-551 │ Builtin          │ Backup Operators        │ C:\Windows\System32\dllhost.exe                                                                                          │           67 │
└─────────────────────┴────────────────────────────────────────────────┴──────────────────┴─────────────────┴──────────────┴──────────────────┴─────────────────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
  27 rows                                                                                                                                                                                                                                                                                               9 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4799' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.CallerProcessId   │         6625 │
│ $.Event.EventData.CallerProcessName │         6625 │
│ $.Event.EventData.SubjectDomainName │         6625 │
│ $.Event.EventData.SubjectLogonId    │         6625 │
│ $.Event.EventData.SubjectUserName   │         6625 │
│ $.Event.EventData.SubjectUserSid    │         6625 │
│ $.Event.EventData.TargetDomainName  │         6625 │
│ $.Event.EventData.TargetSid         │         6625 │
│ $.Event.EventData.TargetUserName    │         6625 │
└─────────────────────────────────────┴──────────────┘
```



## 4800 (The workstation was locked)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.SessionId, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4800
                GROUP BY ALL
                ORDER BY Event.EventData.TargetUserSid;
┌─────────────────────┬────────────────────────────────────────────────┬──────────────────┬────────────────┬───────────┬──────────────┐
│      Computer       │                 TargetUserSid                  │ TargetDomainName │ TargetUserName │ SessionId │ count_star() │
│       varchar       │                    varchar                     │     varchar      │    varchar     │   int64   │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼──────────────────┼────────────────┼───────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan        │        13 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan        │        14 │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan        │         4 │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc         │        12 │            8 │
└─────────────────────┴────────────────────────────────────────────────┴──────────────────┴────────────────┴───────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4800' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.SessionId        │           36 │
│ $.Event.EventData.TargetDomainName │           36 │
│ $.Event.EventData.TargetLogonId    │           36 │
│ $.Event.EventData.TargetUserName   │           36 │
│ $.Event.EventData.TargetUserSid    │           36 │
└────────────────────────────────────┴──────────────┘
```



## 4801 (The workstation was unlocked)
```sql
security-rd08 D SELECT Event.System.Computer, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.SessionId, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd08.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 4801
                GROUP BY ALL
                ORDER BY Event.EventData.TargetUserSid;
┌─────────────────────┬────────────────────────────────────────────────┬──────────────────┬────────────────┬───────────┬──────────────┐
│      Computer       │                 TargetUserSid                  │ TargetDomainName │ TargetUserName │ SessionId │ count_star() │
│       varchar       │                    varchar                     │     varchar      │    varchar     │   int64   │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼──────────────────┼────────────────┼───────────┼──────────────┤
│ rd08.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ shieldbase       │ slevine        │         4 │            2 │
│ rd08.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ shieldbase       │ slevine        │         2 │            6 │
└─────────────────────┴────────────────────────────────────────────────┴──────────────────┴────────────────┴───────────┴──────────────┘
```
fullkey
```sql
security-rd08 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd08.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '4801' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.SessionId        │           15 │
│ $.Event.EventData.TargetDomainName │           15 │
│ $.Event.EventData.TargetLogonId    │           15 │
│ $.Event.EventData.TargetUserName   │           15 │
│ $.Event.EventData.TargetUserSid    │           15 │
└────────────────────────────────────┴──────────────┘
```



## 4802 (The screen saver was invoked)



## 4803 (The screen saver was dismissed)



## 5136 (A directory service object was modified)
```sql
security-dc01 D SELECT Event.System.Computer, Event.EventData.ObjectClass, Event.EventData.AttributeLDAPDisplayName, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND json_extract_string(Event.System.EventID, '$') = '5136'
                GROUP BY ALL
                ORDER BY ALL;
┌─────────────────────┬──────────────────────┬────────────────────────────────┬──────────────┐
│      Computer       │     ObjectClass      │    AttributeLDAPDisplayName    │ count_star() │
│       varchar       │       varchar        │            varchar             │    int64     │
├─────────────────────┼──────────────────────┼────────────────────────────────┼──────────────┤
│ dc01.shieldbase.com │ computer             │ servicePrincipalName           │            2 │
│ dc01.shieldbase.com │ domainDNS            │ gPLink                         │            4 │
│ dc01.shieldbase.com │ groupPolicyContainer │ cn                             │            1 │
│ dc01.shieldbase.com │ groupPolicyContainer │ displayName                    │            3 │
│ dc01.shieldbase.com │ groupPolicyContainer │ flags                          │            1 │
│ dc01.shieldbase.com │ groupPolicyContainer │ gPCFileSysPath                 │            1 │
│ dc01.shieldbase.com │ groupPolicyContainer │ gPCFunctionalityVersion        │            1 │
│ dc01.shieldbase.com │ groupPolicyContainer │ gPCMachineExtensionNames       │            1 │
│ dc01.shieldbase.com │ groupPolicyContainer │ nTSecurityDescriptor           │           22 │
│ dc01.shieldbase.com │ groupPolicyContainer │ objectClass                    │            1 │
│ dc01.shieldbase.com │ groupPolicyContainer │ versionNumber                  │           11 │
│ dc01.shieldbase.com │ organizationalUnit   │ gPLink                         │           12 │
│ dc01.shieldbase.com │ user                 │ deliverAndRedirect             │            2 │
│ dc01.shieldbase.com │ user                 │ msExchGenericForwardingAddress │            1 │
└─────────────────────┴──────────────────────┴────────────────────────────────┴──────────────┘
  14 rows                                                                          4 columns
```
```sql
security-dc01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.DSName, Event.EventData.DSType, Event.EventData.ObjectClass, Event.EventData.ObjectGUID, Event.EventData.ObjectDN, Event.EventData.AttributeLDAPDisplayName, Event.EventData.AttributeSyntaxOID, Event.EventData.AttributeValue, Event.EventData.OperationType, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND json_extract_string(Event.System.EventID, '$') = '5136'
                AND Event.EventData.ObjectClass IN ('computer', 'user')
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬────────────────┬─────────┬─────────────┬──────────────────────────────────────┬────────────────────────────────────────────────────┬────────────────────────────────┬────────────────────┬─────────────────────────────────────────────┬───────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │     DSName     │ DSType  │ ObjectClass │              ObjectGUID              │                      ObjectDN                      │    AttributeLDAPDisplayName    │ AttributeSyntaxOID │               AttributeValue                │ OperationType │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │    varchar     │ varchar │   varchar   │                 uuid                 │                      varchar                       │            varchar             │      varchar       │                   varchar                   │    varchar    │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────┼─────────┼─────────────┼──────────────────────────────────────┼────────────────────────────────────────────────────┼────────────────────────────────┼────────────────────┼─────────────────────────────────────────────┼───────────────┼──────────────┤
│ dc01.shieldbase.com │ S-1-5-18                                       │ NT AUTHORITY      │ SYSTEM          │ shieldbase.com │ %%14676 │ computer    │ eeed7e83-636b-48b3-8411-fbf0b50bdd72 │ CN=DC01,OU=Domain Controllers,DC=shieldbase,DC=com │ servicePrincipalName           │ 2.5.5.12           │ TERMSRV/dc01.shieldbase.com                 │ %%14674       │            1 │
│ dc01.shieldbase.com │ S-1-5-18                                       │ NT AUTHORITY      │ SYSTEM          │ shieldbase.com │ %%14676 │ computer    │ eeed7e83-636b-48b3-8411-fbf0b50bdd72 │ CN=DC01,OU=Domain Controllers,DC=shieldbase,DC=com │ servicePrincipalName           │ 2.5.5.12           │ TERMSRV/DC01                                │ %%14674       │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1148 │ shieldbase        │ EXCHANGE01$     │ shieldbase.com │ %%14676 │ user        │ dbe46a94-9a3b-4469-90cf-f8f32f1886c5 │ CN=Exchange Admin,CN=Users,DC=shieldbase,DC=com    │ msExchGenericForwardingAddress │ 2.5.5.12           │ smtp:marketing@mail.engagementsynergies.com │ %%14674       │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1148 │ shieldbase        │ EXCHANGE01$     │ shieldbase.com │ %%14676 │ user        │ dbe46a94-9a3b-4469-90cf-f8f32f1886c5 │ CN=Exchange Admin,CN=Users,DC=shieldbase,DC=com    │ deliverAndRedirect             │ 2.5.5.8            │ TRUE                                        │ %%14675       │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1148 │ shieldbase        │ EXCHANGE01$     │ shieldbase.com │ %%14676 │ user        │ dbe46a94-9a3b-4469-90cf-f8f32f1886c5 │ CN=Exchange Admin,CN=Users,DC=shieldbase,DC=com    │ deliverAndRedirect             │ 2.5.5.8            │ TRUE                                        │ %%14674       │            1 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴────────────────┴─────────┴─────────────┴──────────────────────────────────────┴────────────────────────────────────────────────────┴────────────────────────────────┴────────────────────┴─────────────────────────────────────────────┴───────────────┴──────────────┘
```
fullkey
```sql
security-dc01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-dc01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '5136' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌────────────────────────────────────────────┬──────────────┐
│                  fullkey                   │ count_star() │
│                  varchar                   │    int64     │
├────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AppCorrelationID         │           63 │
│ $.Event.EventData.AttributeLDAPDisplayName │           63 │
│ $.Event.EventData.AttributeSyntaxOID       │           63 │
│ $.Event.EventData.AttributeValue           │           63 │
│ $.Event.EventData.DSName                   │           63 │
│ $.Event.EventData.DSType                   │           63 │
│ $.Event.EventData.ObjectClass              │           63 │
│ $.Event.EventData.ObjectDN                 │           63 │
│ $.Event.EventData.ObjectGUID               │           63 │
│ $.Event.EventData.OpCorrelationID          │           63 │
│ $.Event.EventData.OperationType            │           63 │
│ $.Event.EventData.SubjectDomainName        │           63 │
│ $.Event.EventData.SubjectLogonId           │           63 │
│ $.Event.EventData.SubjectUserName          │           63 │
│ $.Event.EventData.SubjectUserSid           │           63 │
└────────────────────────────────────────────┴──────────────┘
  15 rows                                         2 columns
```
ObjectClass  
https://learn.microsoft.com/en-us/windows/win32/adschema/classes-all  
AttributeLDAPDisplayName  
https://learn.microsoft.com/en-us/windows/win32/adschema/attributes-all  
AttributeSyntaxOID
```
2.5.5.0:	Undefined:	Not a legal syntax.
2.5.5.1:	Object(DN-DN):	The fully qualified name of an object in the directory.
2.5.5.2:	String(Object-Identifier):	The object identifier.
2.5.5.3:	Case-Sensitive: String	General String.
2.5.5.4:	CaseIgnoreString(Teletex):	Differentiates uppercase and lowercase.
2.5.5.5:	String(Printable), String(IA5):	Teletex. Doesn't differentiate uppercase and lowercase.
2.5.5.6:	String(Numeric):	Printable string or IA5-String.
2.5.5.7:	Object(DN-Binary):	Both character sets are case-sensitive.
2.5.5.8:	Boolean:	A sequence of digits.
2.5.5.9:	Integer, Enumeration:	A distinguished name plus a binary large object.
2.5.5.10:	String(Octet):	TRUE or FALSE values.
2.5.5.11:	String(UTC-Time), String(Generalized-Time):	A 32-bit number or enumeration.
2.5.5.12:	String(Unicode):	A string of bytes.
2.5.5.13:	Object(Presentation-Address):	UTC Time or Generalized-Time.
2.5.5.14:	Object(DN-String):	Unicode string.
2.5.5.15:	String(NT-Sec-Desc):	Presentation address.
2.5.5.16:	LargeInteger:	A DN-String plus a Unicode string.
2.5.5.17:	String(Sid):	A Microsoft® Windows NT® Security descriptor.
```



## 5137 (A directory service object was created)
```sql
security-dc01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.DSName, Event.EventData.DSType, Event.EventData.ObjectClass, Event.EventData.ObjectGUID, Event.EventData.ObjectDN, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND json_extract_string(Event.System.EventID, '$') = '5137'
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬────────────────┬─────────┬──────────────────────┬──────────────────────────────────────┬──────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │     DSName     │ DSType  │     ObjectClass      │              ObjectGUID              │                                       ObjectDN                                       │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │    varchar     │ varchar │       varchar        │                 uuid                 │                                       varchar                                        │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────┼─────────┼──────────────────────┼──────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase        │ rsydow-a        │ shieldbase.com │ %%14676 │ groupPolicyContainer │ e1c172d0-5f61-42b2-82fe-73cd767fe1cd │ CN={6EEADF6A-579D-4D1B-8AC8-69CD74D191DE},CN=Policies,CN=System,DC=shieldbase,DC=com │            1 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴────────────────┴─────────┴──────────────────────┴──────────────────────────────────────┴──────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
security-dc01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-dc01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '5137' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AppCorrelationID  │            1 │
│ $.Event.EventData.DSName            │            1 │
│ $.Event.EventData.DSType            │            1 │
│ $.Event.EventData.ObjectClass       │            1 │
│ $.Event.EventData.ObjectDN          │            1 │
│ $.Event.EventData.ObjectGUID        │            1 │
│ $.Event.EventData.OpCorrelationID   │            1 │
│ $.Event.EventData.SubjectDomainName │            1 │
│ $.Event.EventData.SubjectLogonId    │            1 │
│ $.Event.EventData.SubjectUserName   │            1 │
│ $.Event.EventData.SubjectUserSid    │            1 │
└─────────────────────────────────────┴──────────────┘
  11 rows                                  2 columns
```



## 5138 (A directory service object was undeleted)



## 5139 (A directory service object was moved)



## 5140 (A network share object was accessed)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ShareName, Event.EventData.ShareLocalPath, Event.EventData.ObjectType, Event.EventData.IpAddress, Event.EventData.AccessMask, Event.EventData.AccessList, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 5140
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.ShareName, Event.EventData.IpAddress;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬──────────────┬────────────────┬────────────┬───────────────────────────┬────────────┬────────────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │  ShareName   │ ShareLocalPath │ ObjectType │         IpAddress         │ AccessMask │     AccessList     │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │   varchar    │    varchar     │  varchar   │          varchar          │  varchar   │      varchar       │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────┼────────────────┼────────────┼───────────────────────────┼────────────┼────────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase        │ cbarton-a       │ \\*\IPC$     │                │ File       │ 172.16.5.25               │ 0x1        │ %%4416\r\n\t\t\t\t │            7 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase        │ cbarton-a       │ \\*\TempFRes │ \??\C:\        │ File       │ 172.16.5.25               │ 0x1        │ %%4416\r\n\t\t\t\t │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase        │ rsydow-a        │ \\*\C$       │ \??\C:\        │ File       │ 172.16.6.18               │ 0x1        │ %%4416\r\n\t\t\t\t │            3 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase        │ rsydow-a        │ \\*\IPC$     │                │ File       │ 172.16.6.18               │ 0x1        │ %%4416\r\n\t\t\t\t │            3 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ \\*\C$       │ \??\C:\        │ File       │ ::1                       │ 0x1        │ %%4416\r\n\t\t\t\t │           12 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ \\*\IPC$     │                │ File       │ ::1                       │ 0x1        │ %%4416\r\n\t\t\t\t │            9 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ \\*\IPC$     │                │ File       │ fe80::7e6b:763c:b405:22b4 │ 0x1        │ %%4416\r\n\t\t\t\t │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1159 │ shieldbase        │ HUNT01$         │ \\*\TempFRes │ \??\C:\        │ File       │ 172.16.5.25               │ 0x1        │ %%4416\r\n\t\t\t\t │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ shieldbase        │ slevine         │ \\*\IPC$     │                │ File       │ 172.16.6.18               │ 0x1        │ %%4416\r\n\t\t\t\t │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ shieldbase        │ DEV01$          │ \\*\C$       │ \??\C:\        │ File       │ 172.16.4.9                │ 0x1        │ %%4416\r\n\t\t\t\t │            4 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ shieldbase        │ DEV01$          │ \\*\IPC$     │                │ File       │ 172.16.4.9                │ 0x1        │ %%4416\r\n\t\t\t\t │            4 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ \\*\IPC$     │                │ File       │ 172.16.6.18               │ 0x1        │ %%4416\r\n\t\t\t\t │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ \\*\IPC$     │                │ File       │ fe80::7e6b:763c:b405:22b4 │ 0x1        │ %%4416\r\n\t\t\t\t │            1 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴──────────────┴────────────────┴────────────┴───────────────────────────┴────────────┴────────────────────┴──────────────┘
  13 rows                                                                                                                                                                                                                   11 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '5140' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AccessList        │           71 │
│ $.Event.EventData.AccessMask        │           71 │
│ $.Event.EventData.IpAddress         │           71 │
│ $.Event.EventData.IpPort            │           71 │
│ $.Event.EventData.ObjectType        │           71 │
│ $.Event.EventData.ShareLocalPath    │           71 │
│ $.Event.EventData.ShareName         │           71 │
│ $.Event.EventData.SubjectDomainName │           71 │
│ $.Event.EventData.SubjectLogonId    │           71 │
│ $.Event.EventData.SubjectUserName   │           71 │
│ $.Event.EventData.SubjectUserSid    │           71 │
└─────────────────────────────────────┴──────────────┘
  11 rows                                  2 columns
```



## 5141 (A directory service object was deleted)



## 5142 (A network share object was added)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ShareName, Event.EventData.ShareLocalPath,  COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 5142
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.ShareName;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬──────────────┬────────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │  ShareName   │ ShareLocalPath │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │   varchar    │    varchar     │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────┼────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ \\*\ADMIN$   │ C:\Windows     │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ \\*\C$       │ C:\            │            4 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ \\*\IPC$     │                │            4 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase        │ cbarton-a       │ \\*\TempFRes │ C:\            │            3 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴──────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '5142' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.ShareLocalPath    │           42 │
│ $.Event.EventData.ShareName         │           42 │
│ $.Event.EventData.SubjectDomainName │           42 │
│ $.Event.EventData.SubjectLogonId    │           42 │
│ $.Event.EventData.SubjectUserName   │           42 │
│ $.Event.EventData.SubjectUserSid    │           42 │
└─────────────────────────────────────┴──────────────┘
```



## 5143 (A network share object was modified)
```sql
security-file01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ShareName, Event.EventData.ShareLocalPath, Event.EventData.ObjectType, Event.EventData.OldShareFlags, Event.EventData.OldRemark, Event.EventData.OldMaxUsers, Event.EventData.OldSD, Event.EventData.NewShareFlags, Event.EventData.NewRemark, Event.EventData.NewMaxUsers, Event.EventData.NewSD, COUNT(*)
                  FROM read_json('C:/Users/SANSDFIR/security-file01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                  WHERE json_extract_string(Event.System.EventID, '$') = '5143'
                  GROUP BY ALL
                  ORDER BY Event.EventData.SubjectUserSid, Event.EventData.ShareName;
┌───────────────────────┬────────────────┬───────────────────┬─────────────────┬──────────────────────┬────────────────────────────┬────────────┬───────────────┬───────────┬─────────────┬─────────┬───────────────┬───────────┬─────────────┬─────────┬──────────────┐
│       Computer        │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │      ShareName       │       ShareLocalPath       │ ObjectType │ OldShareFlags │ OldRemark │ OldMaxUsers │  OldSD  │ NewShareFlags │ NewRemark │ NewMaxUsers │  NewSD  │ count_star() │
│        varchar        │    varchar     │      varchar      │     varchar     │       varchar        │          varchar           │  varchar   │    varchar    │  varchar  │   varchar   │ varchar │    varchar    │  varchar  │   varchar   │ varchar │    int64     │
├───────────────────────┼────────────────┼───────────────────┼─────────────────┼──────────────────────┼────────────────────────────┼────────────┼───────────────┼───────────┼─────────────┼─────────┼───────────────┼───────────┼─────────────┼─────────┼──────────────┤
│ file01.shieldbase.com │ S-1-5-18       │ shieldbase        │ FILE01$         │ \\*\Installers       │ C:\Shares\Installers       │ Directory  │ 0x0           │ N/A       │ 0xffffffff  │ NULL    │ 0x0           │ N/A       │ 0xffffffff  │ NULL    │            1 │
│ file01.shieldbase.com │ S-1-5-18       │ shieldbase        │ FILE01$         │ \\*\SRL-Admin        │ C:\Shares\SRL-Admin        │ Directory  │ 0x0           │ N/A       │ 0xffffffff  │ NULL    │ 0x0           │ N/A       │ 0xffffffff  │ NULL    │            1 │
│ file01.shieldbase.com │ S-1-5-18       │ shieldbase        │ FILE01$         │ \\*\shieldbase-share │ C:\Shares\shieldbase-share │ Directory  │ 0x0           │ N/A       │ 0xffffffff  │ NULL    │ 0x0           │ N/A       │ 0xffffffff  │ NULL    │            1 │
└───────────────────────┴────────────────┴───────────────────┴─────────────────┴──────────────────────┴────────────────────────────┴────────────┴───────────────┴───────────┴─────────────┴─────────┴───────────────┴───────────┴─────────────┴─────────┴──────────────┘
```
fullkey
```sql
security-file01 D SELECT je.fullkey, COUNT(*)
                  FROM read_json_objects('C:/Users/SANSDFIR/security-file01.json') AS e, json_tree(e.json) AS je
                  WHERE e.json->>'$.Event.System.EventID' = '5143' AND je.fullkey LIKE '$.Event.EventData.%'
                  GROUP BY je.fullkey
                  ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.NewMaxUsers       │            3 │
│ $.Event.EventData.NewRemark         │            3 │
│ $.Event.EventData.NewSD             │            3 │
│ $.Event.EventData.NewShareFlags     │            3 │
│ $.Event.EventData.ObjectType        │            3 │
│ $.Event.EventData.OldMaxUsers       │            3 │
│ $.Event.EventData.OldRemark         │            3 │
│ $.Event.EventData.OldSD             │            3 │
│ $.Event.EventData.OldShareFlags     │            3 │
│ $.Event.EventData.ShareLocalPath    │            3 │
│ $.Event.EventData.ShareName         │            3 │
│ $.Event.EventData.SubjectDomainName │            3 │
│ $.Event.EventData.SubjectLogonId    │            3 │
│ $.Event.EventData.SubjectUserName   │            3 │
│ $.Event.EventData.SubjectUserSid    │            3 │
└─────────────────────────────────────┴──────────────┘
  15 rows                                  2 columns
```



## 5144 (A network share object was deleted)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ShareName, Event.EventData.ShareLocalPath,  COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 5144
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid, Event.EventData.ShareName;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬──────────────┬────────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │  ShareName   │ ShareLocalPath │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │   varchar    │    varchar     │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────┼────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase        │ cbarton-a       │ \\*\TempFRes │ C:\            │            3 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴──────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '5144' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.ShareLocalPath    │            3 │
│ $.Event.EventData.ShareName         │            3 │
│ $.Event.EventData.SubjectDomainName │            3 │
│ $.Event.EventData.SubjectLogonId    │            3 │
│ $.Event.EventData.SubjectUserName   │            3 │
│ $.Event.EventData.SubjectUserSid    │            3 │
└─────────────────────────────────────┴──────────────┘
```



## 5145 (Network share object access)



## 5152 (The Windows Filtering Platform has blocked a packet)



## 5154 (The Windows Filtering Platform has permitted an application or service to listen on a port for incoming connections)



## 5156 (The Windows Filtering Platform has allowed a connection)



## 5157 (The Windows Filtering Platform has blocked a connection)



## 5158 (The Windows Filtering Platform has permitted a bind to a local port)



## 5159 (The Windows Filtering Platform has permitted an application or service to listen on a port for incoming connections)



## 5379 (Credential Manager credentials were read)
```sql
security-rd01 D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetName, Event.EventData.ReadOperation, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND Event.System.EventID = 5379
                GROUP BY ALL
                ORDER BY Event.EventData.SubjectUserSid , Event.EventData.TargetName;
┌─────────────────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬───────────────────────────────────────────────────────────────────────┬───────────────┬──────────────┐
│      Computer       │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │                              TargetName                               │ ReadOperation │ count_star() │
│       varchar       │                    varchar                     │      varchar      │     varchar     │                                varchar                                │    varchar    │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼───────────────────────────────────────────────────────────────────────┼───────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ MicrosoftAccount:user=02ljejifphxphkae                                │ %%8100        │        18860 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ WindowsLive:(cert):name=02ljejifphxphkae;serviceuri=*                 │ %%8100        │        11503 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ WindowsLive:(token):name=02ljejifphxphkae;serviceuri=*                │ %%8100        │        11503 │
│ rd01.shieldbase.com │ S-1-5-18                                       │ shieldbase        │ RD01$           │ WindowsLive:target=virtualapp/didlogical                              │ %%8100        │         2130 │
│ rd01.shieldbase.com │ S-1-5-19                                       │ NT AUTHORITY      │ LOCAL SERVICE   │ MicrosoftAccount:user=02nyzpzsqlsoqfjn                                │ %%8100        │          160 │
│ rd01.shieldbase.com │ S-1-5-19                                       │ NT AUTHORITY      │ LOCAL SERVICE   │ WindowsLive:(cert):name=02nyzpzsqlsoqfjn;serviceuri=*                 │ %%8100        │          120 │
│ rd01.shieldbase.com │ S-1-5-19                                       │ NT AUTHORITY      │ LOCAL SERVICE   │ WindowsLive:(token):name=02nyzpzsqlsoqfjn;serviceuri=*                │ %%8100        │          120 │
│ rd01.shieldbase.com │ S-1-5-19                                       │ NT AUTHORITY      │ LOCAL SERVICE   │ WindowsLive:target=virtualapp/didlogical                              │ %%8100        │           49 │
│ rd01.shieldbase.com │ S-1-5-20                                       │ shieldbase        │ RD01$           │ MicrosoftAccount:user=02etqktgjtgolqnz                                │ %%8100        │           67 │
│ rd01.shieldbase.com │ S-1-5-20                                       │ shieldbase        │ RD01$           │ WindowsLive:(cert):name=02etqktgjtgolqnz;serviceuri=*                 │ %%8100        │           46 │
│ rd01.shieldbase.com │ S-1-5-20                                       │ shieldbase        │ RD01$           │ WindowsLive:(token):name=02etqktgjtgolqnz;serviceuri=*                │ %%8100        │           46 │
│ rd01.shieldbase.com │ S-1-5-20                                       │ shieldbase        │ RD01$           │ WindowsLive:target=virtualapp/didlogical                              │ %%8100        │           23 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ Microsoft OneDrive Generic Data - Personal Vault VHD Info             │ %%8100        │           14 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ MicrosoftAccount:user=02mxdhnbqjkqpsrt                                │ %%8100        │          245 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ MicrosoftAccount:user=tdungan@stark-research-labs.com                 │ %%8100        │        11554 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ MicrosoftAccount:user=tdungan@stark-research-labs.com                 │ %%8099        │          218 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ MicrosoftOffice*                                                      │ %%8100        │           26 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ MicrosoftOffice16_Data:ADAL:*                                         │ %%8100        │           52 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ MicrosoftOffice16_Data:ADALWAM:*                                      │ %%8100        │           52 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ MicrosoftOffice16_Data:SSPI:*                                         │ %%8100        │           52 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ MicrosoftOffice16_Data:live:cid=*                                     │ %%8100        │           27 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ OneDrive-INTCachedCredential*                                         │ %%8100        │            6 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ OneDriveCachedCredential*                                             │ %%8100        │            6 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ SkyDrive Cached Credential                                            │ %%8099        │            3 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ SkyDrive-INT Cached Credential                                        │ %%8099        │            3 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ WindowsLive:(cert):name=02mxdhnbqjkqpsrt;serviceuri=*                 │ %%8100        │          398 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ WindowsLive:(cert):name=tdungan@stark-research-labs.com;serviceuri=*  │ %%8100        │          165 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ WindowsLive:(token):name=02mxdhnbqjkqpsrt;serviceuri=*                │ %%8100        │          398 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ WindowsLive:(token):name=tdungan@stark-research-labs.com;serviceuri=* │ %%8100        │          165 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ WindowsLive:target=virtualapp/didlogical                              │ %%8100        │          349 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ MicrosoftAccount:user=02bdyeokokcxzhbc                                │ %%8100        │           97 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ MicrosoftAccount:user=02bdyeokokcxzhbc                                │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ MicrosoftOffice*                                                      │ %%8100        │            9 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ MicrosoftOffice16_Data:ADAL:*                                         │ %%8100        │           24 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ MicrosoftOffice16_Data:ADALWAM:*                                      │ %%8100        │           24 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ MicrosoftOffice16_Data:SSPI:*                                         │ %%8100        │           24 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ MicrosoftOffice16_Data:live:cid=*                                     │ %%8100        │           11 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ OneDrive Cached Credential                                            │ %%8100        │            5 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ OneDrive Cached Credential*                                           │ %%8100        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ OneDrive-INTCachedCredential*                                         │ %%8100        │            8 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ OneDriveCachedCredential*                                             │ %%8100        │            8 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ SkyDrive Cached Credential                                            │ %%8099        │            3 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ SkyDrive-INT Cached Credential                                        │ %%8099        │            3 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/d                                                             │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/de                                                            │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev                                                           │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev0                                                          │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01                                                         │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.                                                        │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.s                                                       │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.sh                                                      │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shi                                                     │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shie                                                    │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shiel                                                   │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shield                                                  │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shieldb                                                 │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shieldba                                                │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shieldbas                                               │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shieldbase                                              │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shieldbase.                                             │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shieldbase.c                                            │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shieldbase.co                                           │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ TERMSRV/dev01.shieldbase.com                                          │ %%8099        │            6 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:(cert):name=02bdyeokokcxzhbc;serviceuri=*                 │ %%8100        │          152 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:(cert):name=02gcgnqpbtfgqeym;serviceuri=*                 │ %%8100        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:(cert):name=02qbckrjqyeclccf;serviceuri=*                 │ %%8100        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:(cert):name=02vxkihhmncrdtya;serviceuri=*                 │ %%8100        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:(token):name=02bdyeokokcxzhbc;serviceuri=*                │ %%8100        │          152 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:(token):name=02gcgnqpbtfgqeym;serviceuri=*                │ %%8100        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:(token):name=02qbckrjqyeclccf;serviceuri=*                │ %%8100        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:(token):name=02vxkihhmncrdtya;serviceuri=*                │ %%8100        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:name=*                                                    │ %%8100        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:target=virtualapp/didlogical                              │ %%8100        │          137 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ WindowsLive:user=*                                                    │ %%8100        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ d                                                                     │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ de                                                                    │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev                                                                   │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev0                                                                  │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01                                                                 │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.                                                                │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.s                                                               │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.sh                                                              │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shi                                                             │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shie                                                            │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shiel                                                           │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shield                                                          │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shieldb                                                         │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shieldba                                                        │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shieldbas                                                       │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shieldbase                                                      │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shieldbase.                                                     │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shieldbase.c                                                    │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shieldbase.co                                                   │ %%8099        │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ dev01.shieldbase.com                                                  │ %%8099        │            6 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=d                       │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=de                      │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev                     │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev0                    │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01                   │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.                  │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.s                 │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.sh                │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shi               │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shie              │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shiel             │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shield            │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shieldb           │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shieldba          │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shieldbas         │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shieldbase        │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shieldbase.       │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shieldbase.c      │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shieldbase.co     │ %%8099        │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ {28bfc32f-10f6-4738-98d1-1ac061df716a}:target=dev01.shieldbase.com    │ %%8099        │            3 │
└─────────────────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴───────────────────────────────────────────────────────────────────────┴───────────────┴──────────────┘
  114 rows                                                                                                                                                                                                7 columns
```
fullkey
```sql
security-rd01 D SELECT je.fullkey, COUNT(*)
                FROM read_json_objects('C:/Users/SANSDFIR/security-rd01.json') AS e, json_tree(e.json) AS je
                WHERE e.json->>'$.Event.System.EventID' = '5379' AND je.fullkey LIKE '$.Event.EventData.%'
                GROUP BY je.fullkey
                ORDER BY je.fullkey;
┌──────────────────────────────────────────────┬──────────────┐
│                   fullkey                    │ count_star() │
│                   varchar                    │    int64     │
├──────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.ClientProcessId            │        81294 │
│ $.Event.EventData.CountOfCredentialsReturned │        81294 │
│ $.Event.EventData.ProcessCreationTime        │        81294 │
│ $.Event.EventData.ReadOperation              │        81294 │
│ $.Event.EventData.ReturnCode                 │        81294 │
│ $.Event.EventData.SubjectDomainName          │        81294 │
│ $.Event.EventData.SubjectLogonId             │        81294 │
│ $.Event.EventData.SubjectUserName            │        81294 │
│ $.Event.EventData.SubjectUserSid             │        81294 │
│ $.Event.EventData.TargetName                 │        81294 │
│ $.Event.EventData.Type                       │        81294 │
└──────────────────────────────────────────────┴──────────────┘
  11 rows                                           2 columns
```



## 6272 (The Windows Filtering Platform has permitted an application or service to listen on a port for incoming connections)



## 6273 (Network Policy Server denied access to a user)



## 6416 (A new external device was recognized by the system)
