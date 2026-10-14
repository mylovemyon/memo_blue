```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f security.json C:\Windows\System32\winevt\Logs\Security.evtx
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off" .\security.json
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
security D
```

## .databases
```sql
security D .databases
┌───────────────────┐
│     databases     │
│                   │
│ security (memory) │
└───────────────────┘
```

## .tables
```sql
security D .tables
 ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── security ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── main ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                                                                                                                                                                                                              security                                                                                                                                                                                                                                                                                                               │
│                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid varchar)), eventid bigint, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated struct("#attributes" struct(systemtime timestamp)), eventrecordid bigint, correlation struct("#attributes" struct(activityid uuid)), execution struct("#attributes" struct(processid bigint, threadid bigint)), channel varchar, computer varchar, "security" json), eventdata map(varchar, json), userdata struct(serviceshutdown struct("#attributes" struct(xmlns varchar)))) │
└─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                                                                                                                                                                                                                file                                                                                                                                                                                                                                                                                                                 │
│                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid varchar)), eventid bigint, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated struct("#attributes" struct(systemtime timestamp)), eventrecordid bigint, correlation struct("#attributes" struct(activityid uuid)), execution struct("#attributes" struct(processid bigint, threadid bigint)), channel varchar, computer varchar, "security" json), eventdata map(varchar, json), userdata struct(serviceshutdown struct("#attributes" struct(xmlns varchar)))) │
└─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## DESCRIBE
```sql
security D DESCRIBE;
┌──────────┬─────────┬──────────┬──────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────┐
│ database │ schema  │   name   │ column_names │                                                                                         column_types                                                                                          │ temporary │
│ varchar  │ varchar │ varchar  │  varchar[]   │                                                                                           varchar[]                                                                                           │  boolean  │
├──────────┼─────────┼──────────┼──────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────┤
│ security │ main    │ file     │ [Event]      │ ['STRUCT("#attributes" STRUCT(xmlns VARCHAR), "System" STRUCT(Provider STRUCT("#attributes" STRUCT("Name" VARCHAR, Guid VARCHAR)), EventID BIGINT, "Version" BIGINT, "Level" BIGINT, Task BIG │ false     │
│          │         │          │              │ INT, Opcode BIGINT, Keywords VARCHAR, TimeCreated STRUCT("#attributes" STRUCT(SystemTime TIMESTAMP)), EventRecordID BIGINT, Correlation STRUCT("#attributes" STRUCT(ActivityID UUID)), Execut │           │
│          │         │          │              │ ion STRUCT("#attributes" STRUCT(ProcessID BIGINT, ThreadID BIGINT)), Channel VARCHAR, Computer VARCHAR, "Security" JSON), EventData MAP(VARCHAR, JSON), UserData STRUCT(ServiceShutdown STRUC │           │
│          │         │          │              │ T("#attributes" STRUCT(xmlns VARCHAR))))']                                                                                                                                                    │           │
├──────────┼─────────┼──────────┼──────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────┤
│ security │ main    │ security │ [Event]      │ ['STRUCT("#attributes" STRUCT(xmlns VARCHAR), "System" STRUCT(Provider STRUCT("#attributes" STRUCT("Name" VARCHAR, Guid VARCHAR)), EventID BIGINT, "Version" BIGINT, "Level" BIGINT, Task BIG │ false     │
│          │         │          │              │ INT, Opcode BIGINT, Keywords VARCHAR, TimeCreated STRUCT("#attributes" STRUCT(SystemTime TIMESTAMP)), EventRecordID BIGINT, Correlation STRUCT("#attributes" STRUCT(ActivityID UUID)), Execut │           │
│          │         │          │              │ ion STRUCT("#attributes" STRUCT(ProcessID BIGINT, ThreadID BIGINT)), Channel VARCHAR, Computer VARCHAR, "Security" JSON), EventData MAP(VARCHAR, JSON), UserData STRUCT(ServiceShutdown STRUC │           │
│          │         │          │              │ T("#attributes" STRUCT(xmlns VARCHAR))))']                                                                                                                                                    │           │
└──────────┴─────────┴──────────┴──────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────┘
```

## Computer
```sql
security D SELECT Event.System.Computer, COUNT(*)
           FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
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
security D SELECT Event.System.EventID, COUNT(*)
           FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
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
security-rd01 D SELECT Event.System.Computer,Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
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
security-dev01 D SELECT Event.System.Computer,Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
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
security-rd01 D SELECT Event.System.Computer,Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
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
VirtualAccount
```
%%1842: yes
%%1843: no
```
ImpersonationLevel
```
%%1832: Identification
%%1833: Impersonation
%%1840: Delegation
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
AuditPolicyChanges
```
%%8448: Success removed (the system stopped logging successful attempts for the subcategory)
%%8449: Success added (the system started logging successful attempts for the subcategory)
%%8450: Failure removed (the system stopped logging failed attempts for the subcategory)
%%8451: Failure added (the system started logging failed attempts for the subcategory)
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
```
%%1793: value not set
%%1794: never
%%1997: ALL
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



## 4768 (A Kerberos authentication ticket (TGT) was requested)
```sql
security-dc01 D SELECT Event.System.Computer, Event.EventData.ServiceSid, Event.EventData.ServiceName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.PreAuthType, Event.EventData.TicketOptions, Event.EventData.TicketEncryptionType, Event.EventData.Status, Event.EventData.IpAddress, Event.EventData.CertIssuerName, Event.EventData.CertSerialNumber, Event.EventData.CertThumbprint, COUNT(*)
                FROM read_json('C:/Users/SANSDFIR/security-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                AND json_extract_string(Event.System.EventID,'$') = '4768'
                GROUP BY ALL
                ORDER BY Event.EventData.TargetSid, Event.EventData.IpAddress;
┌─────────────────────┬───────────────────────────────────────────────┬─────────────┬────────────────────────────────────────────────┬──────────────────┬──────────────────────┬─────────────┬───────────────┬──────────────────────┬─────────┬────────────────────┬────────────────┬──────────────────┬────────────────┬──────────────┐
│      Computer       │                  ServiceSid                   │ ServiceName │                   TargetSid                    │ TargetDomainName │    TargetUserName    │ PreAuthType │ TicketOptions │ TicketEncryptionType │ Status  │     IpAddress      │ CertIssuerName │ CertSerialNumber │ CertThumbprint │ count_star() │
│       varchar       │                    varchar                    │   varchar   │                    varchar                     │     varchar      │       varchar        │   varchar   │    varchar    │       varchar        │ varchar │      varchar       │    varchar     │     varchar      │    varchar     │    int64     │
├─────────────────────┼───────────────────────────────────────────────┼─────────────┼────────────────────────────────────────────────┼──────────────────┼──────────────────────┼─────────────┼───────────────┼──────────────────────┼─────────┼────────────────────┼────────────────┼──────────────────┼────────────────┼──────────────┤
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1001 │ SHIELDBASE.COM   │ DC01$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::1                │                │                  │                │           74 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1001 │ shieldbase.com   │ DC01$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::1                │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1105 │ SHIELDBASE.COM   │ Jblake               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.13 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1106 │ SHIELDBASE.COM   │ bnicholson           │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.12 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1106 │ shieldbase.com   │ bnicholson           │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.12 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1107 │ shieldbase.com   │ cbarton              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1107 │ SHIELDBASE.COM   │ cbarton              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase       │ cbarton-a            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase.com   │ cbarton-a            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ SHIELDBASE.COM   │ cbarton-a            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ SHIELDBASE       │ cbarton-a            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │          310 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1109 │ shieldbase.com   │ czelman              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.13 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1109 │ SHIELDBASE.COM   │ czelman              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.13 │                │                  │                │            9 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1110 │ shieldbase.com   │ dwatson              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.19 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1110 │ SHIELDBASE.COM   │ dwatson              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.19 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1113 │ SHIELDBASE.COM   │ jpallen              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.12 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1115 │ SHIELDBASE.COM   │ jzoch                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.17 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1115 │ shieldbase.com   │ jzoch                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.17 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1116 │ SHIELDBASE.COM   │ kespinoza            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.16 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1116 │ shieldbase.com   │ kespinoza            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.16 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1118 │ shieldbase.com   │ mhill                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1119 │ shieldbase.com   │ nfury                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1119 │ SHIELDBASE.COM   │ nfury                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1120 │ shieldbase.com   │ nromanoff            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.14 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1120 │ SHIELDBASE.COM   │ nromanoff            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.14 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1122 │ SHIELDBASE.COM   │ rdodson              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.15 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1122 │ shieldbase.com   │ rdodson              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.15 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1124 │ shieldbase       │ rsydow               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1124 │ shieldbase.com   │ rsydow               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1124 │ SHIELDBASE.COM   │ rsydow               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::1                │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::1                │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.7  │                │                  │                │           11 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1126 │ SHIELDBASE.COM   │ smckee               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.20 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1132 │ shieldbase.com   │ sreese               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.18 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1132 │ SHIELDBASE.COM   │ sreese               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.18 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1134 │ SHIELDBASE.COM   │ srogers              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.19 │                │                  │                │           10 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1134 │ shieldbase.com   │ srogers              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.19 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1135 │ SHIELDBASE.COM   │ tanderson            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.20 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1135 │ shieldbase.com   │ tanderson            │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.20 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ SHIELDBASE.COM   │ tdungan              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase.com   │ tdungan              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           11 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1137 │ SHIELDBASE.COM   │ toslund              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.16 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1138 │ SHIELDBASE.COM   │ zdorsey              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.14 │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1138 │ shieldbase.com   │ zdorsey              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.14 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1139 │ shieldbase.com   │ RD03$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.13 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1139 │ SHIELDBASE.COM   │ RD03$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.13 │                │                  │                │           41 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1140 │ SHIELDBASE.COM   │ RD05$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.15 │                │                  │                │           58 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1140 │ shieldbase.com   │ RD05$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.15 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1141 │ SHIELDBASE.COM   │ ADMIN01$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │           40 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1141 │ shieldbase.com   │ ADMIN01$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.26 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1142 │ shieldbase.com   │ RD08$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1142 │ SHIELDBASE.COM   │ RD08$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1143 │ shieldbase.com   │ RD09$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.19 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1143 │ SHIELDBASE.COM   │ RD09$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.19 │                │                  │                │           43 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1144 │ SHIELDBASE.COM   │ RD10$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.20 │                │                  │                │           41 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1144 │ shieldbase.com   │ RD10$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.20 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1145 │ SHIELDBASE.COM   │ RD06$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.16 │                │                  │                │           56 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1145 │ shieldbase.com   │ RD06$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.16 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1146 │ SHIELDBASE.COM   │ RD02$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.12 │                │                  │                │           40 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1146 │ shieldbase.com   │ RD02$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.12 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1147 │ shieldbase.com   │ RD01$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           11 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1147 │ SHIELDBASE.COM   │ RD01$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           41 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1148 │ shieldbase.com   │ EXCHANGE01$          │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1148 │ SHIELDBASE.COM   │ EXCHANGE01$          │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │           79 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1149 │ SHIELDBASE.COM   │ FILE01$              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.5  │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1149 │ shieldbase.com   │ FILE01$              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.5  │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1150 │ SHIELDBASE.COM   │ WKSTN06$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.16 │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1150 │ shieldbase.com   │ WKSTN06$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.16 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1151 │ SHIELDBASE.COM   │ WKSTN05$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1151 │ shieldbase.com   │ WKSTN05$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1152 │ shieldbase.com   │ WKSTN04$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.14 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1152 │ SHIELDBASE.COM   │ WKSTN04$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.14 │                │                  │                │           42 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1153 │ shieldbase.com   │ RD04$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.14 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1153 │ SHIELDBASE.COM   │ RD04$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.14 │                │                  │                │           37 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1154 │ shieldbase.com   │ WKSTN08$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.18 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1154 │ SHIELDBASE.COM   │ WKSTN08$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.18 │                │                  │                │           43 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1155 │ SHIELDBASE.COM   │ WKSTN03$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.13 │                │                  │                │           60 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1155 │ shieldbase.com   │ WKSTN03$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.13 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1156 │ SHIELDBASE.COM   │ WKSTN09$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.19 │                │                  │                │           40 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1156 │ shieldbase.com   │ WKSTN09$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.19 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1157 │ SHIELDBASE.COM   │ WKSTN10$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.20 │                │                  │                │           47 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1157 │ shieldbase.com   │ WKSTN10$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.20 │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1158 │ SHIELDBASE.COM   │ WKSTN02$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.12 │                │                  │                │           50 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1158 │ shieldbase.com   │ WKSTN02$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.12 │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1159 │ shieldbase.com   │ HUNT01$              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1159 │ SHIELDBASE.COM   │ HUNT01$              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.25 │                │                  │                │           41 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1160 │ shieldbase.com   │ RD07$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.17 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1160 │ SHIELDBASE.COM   │ RD07$                │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.17 │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1161 │ shieldbase.com   │ WKSTN01$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1161 │ SHIELDBASE.COM   │ WKSTN01$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1162 │ SHIELDBASE.COM   │ WKSTN07$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.17 │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1162 │ shieldbase.com   │ WKSTN07$             │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.17 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1194 │ SHIELDBASE.COM   │ HealthMailboxc8fd8db │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │         9986 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1195 │ SHIELDBASE.COM   │ HealthMailbox8d23eed │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1206 │ shieldbase.com   │ ELF01$               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.21 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1206 │ SHIELDBASE.COM   │ ELF01$               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.5.21 │                │                  │                │           38 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1212 │ shieldbase.com   │ scarter              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.17 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1212 │ SHIELDBASE.COM   │ scarter              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.17 │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ shieldbase.com   │ slevine              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ SHIELDBASE.COM   │ slevine              │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            1 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1214 │ SHIELDBASE.COM   │ WAC01$               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.7  │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1214 │ shieldbase.com   │ WAC01$               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.7  │                │                  │                │            4 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1215 │ shieldbase.com   │ SQL01$               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.8  │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1215 │ SHIELDBASE.COM   │ SQL01$               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.8  │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ SHIELDBASE.COM   │ DEV01$               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │           39 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ shieldbase.com   │ DEV01$               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            2 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.6  │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.4.9  │                │                  │                │            6 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           11 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.11 │                │                  │                │           14 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            5 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.6.18 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │            8 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.11 │                │                  │                │            7 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            3 │
│ dc01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-502 │ krbtgt      │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc               │ 2           │ 0x40810010    │ 0x12                 │ 0x0     │ ::ffff:172.16.7.15 │                │                  │                │            4 │
└─────────────────────┴───────────────────────────────────────────────┴─────────────┴────────────────────────────────────────────────┴──────────────────┴──────────────────────┴─────────────┴───────────────┴──────────────────────┴─────────┴────────────────────┴────────────────┴──────────────────┴────────────────┴──────────────┘
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



## 4776
```sql
security D
SELECT Event.System.Computer, Event.EventData.Workstation, Event.EventData.TargetUserName, Event.EventData.Status, Event.EventData.PackageName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4778
```sql
security D
SELECT Event.System.Computer, Event.EventData.AccountDomain, Event.EventData.AccountName, Event.EventData.ClientName, Event.EventData.ClientAddress, Event.EventData.SessionName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4779
```sql
security D
SELECT Event.System.Computer, Event.EventData.AccountDomain, Event.EventData.AccountName, Event.EventData.ClientName, Event.EventData.ClientAddress, Event.EventData.SessionName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4799
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.TargetDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.CallerProcessName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 5140
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ShareName, Event.EventData.ShareLocalPath, Event.EventData.ObjectType, Event.EventData.IpAddress, Event.EventData.AccessMask, Event.EventData.AccessList, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 5142
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ShareName, Event.EventData.ShareLocalPath,  COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 5144
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ShareName, Event.EventData.ShareLocalPath,  COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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
