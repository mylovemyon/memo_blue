```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f security.json E:\Windows\System32\winevt\Logs\Security.evtx
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
security D
.databases
┌───────────────────┐
│     databases     │
│                   │
│ security (memory) │
└───────────────────┘
```

## .tables
```sql
security D
.tables
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
security D
DESCRIBE;
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

## EventId
```sql
security D
SELECT Event.System.Computer, Event.System.EventID, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
GROUP BY ALL
ORDER BY ALL;
┌─────────────────────┬─────────┬──────────────┐
│      Computer       │ EventID │ count_star() │
│       varchar       │  int64  │    int64     │
├─────────────────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │    1100 │            4 │
│ rd01.shieldbase.com │    4608 │            4 │
│ rd01.shieldbase.com │    4610 │            4 │
│ rd01.shieldbase.com │    4611 │          146 │
│ rd01.shieldbase.com │    4614 │            8 │
│ rd01.shieldbase.com │    4616 │            6 │
│ rd01.shieldbase.com │    4622 │           40 │
│ rd01.shieldbase.com │    4624 │         5470 │
│ rd01.shieldbase.com │    4625 │           20 │
│ rd01.shieldbase.com │    4634 │         1179 │
│ rd01.shieldbase.com │    4647 │           18 │
│ rd01.shieldbase.com │    4648 │          214 │
│ rd01.shieldbase.com │    4672 │         5319 │
│ rd01.shieldbase.com │    4688 │           44 │
│ rd01.shieldbase.com │    4692 │            2 │
│ rd01.shieldbase.com │    4694 │            3 │
│ rd01.shieldbase.com │    4695 │           45 │
│ rd01.shieldbase.com │    4696 │            4 │
│ rd01.shieldbase.com │    4697 │          430 │
│ rd01.shieldbase.com │    4724 │            1 │
│ rd01.shieldbase.com │    4725 │            1 │
│ rd01.shieldbase.com │    4738 │            2 │
│ rd01.shieldbase.com │    4776 │           14 │
│ rd01.shieldbase.com │    4778 │           12 │
│ rd01.shieldbase.com │    4779 │           13 │
│ rd01.shieldbase.com │    4793 │            1 │
│ rd01.shieldbase.com │    4797 │           70 │
│ rd01.shieldbase.com │    4798 │          141 │
│ rd01.shieldbase.com │    4799 │         1025 │
│ rd01.shieldbase.com │    4800 │           13 │
│ rd01.shieldbase.com │    4826 │            4 │
│ rd01.shieldbase.com │    4902 │            4 │
│ rd01.shieldbase.com │    4904 │            4 │
│ rd01.shieldbase.com │    4905 │            4 │
│ rd01.shieldbase.com │    4907 │            1 │
│ rd01.shieldbase.com │    4944 │            4 │
│ rd01.shieldbase.com │    4945 │          943 │
│ rd01.shieldbase.com │    4946 │          530 │
│ rd01.shieldbase.com │    4947 │           29 │
│ rd01.shieldbase.com │    4948 │          177 │
│ rd01.shieldbase.com │    4954 │            6 │
│ rd01.shieldbase.com │    4956 │            4 │
│ rd01.shieldbase.com │    5061 │           50 │
│ rd01.shieldbase.com │    5140 │           51 │
│ rd01.shieldbase.com │    5142 │           15 │
│ rd01.shieldbase.com │    5144 │            3 │
│ rd01.shieldbase.com │    5379 │        59138 │
│ rd01.shieldbase.com │    5381 │            1 │
│ rd01.shieldbase.com │    5382 │          108 │
│ rd01.shieldbase.com │    5478 │            4 │
└─────────────────────┴─────────┴──────────────┘
  50 rows                            3 columns
```



## 4608
```sql
security D
SELECT Event.System.Computer,Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1)
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
security
D SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '4608' AND je.fullkey LIKE '$.Event.EventData.%'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌─────────┬──────────────┐
│ fullkey │ count_star() │
│ varchar │    int64     │
└─────────┴──────────────┘
          0 rows
```




## 4616
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ProcessName, Event.EventData.PreviousTime, Event.EventData.NewTime, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
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
security D 
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4624
```sql
security D
SELECT Event.System.Computer, Event.EventData.LogonType, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TargetOutboundDomainName, Event.EventData.TargetOutboundUserName, Event.EventData.WorkstationName, Event.EventData.IpAddress, Event.EventData.ProcessName, Event.EventData.AuthenticationPackageName, Event.EventData.LogonProcessName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4624
GROUP BY ALL
ORDER BY  Event.EventData.LogonType, Event.EventData.SubjectUserSid , Event.EventData.TargetUserSid, Event.EventData.IpAddress;
┌─────────────────────┬───────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬────────────────────────────────────────────────┬──────────────────┬─────────────────┬──────────────────────────┬────────────────────────┬─────────────────┬───────────────────────────┬──────────────────────────────────────────────────────────────┬───────────────────────────┬──────────────────┬──────────────┐
│      Computer       │ LogonType │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │                 TargetUserSid                  │ TargetDomainName │ TargetUserName  │ TargetOutboundDomainName │ TargetOutboundUserName │ WorkstationName │         IpAddress         │                         ProcessName                          │ AuthenticationPackageName │ LogonProcessName │ count_star() │
│       varchar       │   int64   │                    varchar                     │      varchar      │     varchar     │                    varchar                     │     varchar      │     varchar     │         varchar          │        varchar         │     varchar     │          varchar          │                           varchar                            │          varchar          │     varchar      │    int64     │
├─────────────────────┼───────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────┼──────────────────┼─────────────────┼──────────────────────────┼────────────────────────┼─────────────────┼───────────────────────────┼──────────────────────────────────────────────────────────────┼───────────────────────────┼──────────────────┼──────────────┤
│ rd01.shieldbase.com │         0 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-18                                       │ NT AUTHORITY     │ SYSTEM          │ -                        │ -                      │ -               │ -                         │                                                              │ -                         │ -                │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-1                                   │ Window Manager   │ DWM-1           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            8 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-10                                  │ Window Manager   │ DWM-10          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-11                                  │ Window Manager   │ DWM-11          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            8 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-12                                  │ Window Manager   │ DWM-12          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-13                                  │ Window Manager   │ DWM-13          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-14                                  │ Window Manager   │ DWM-14          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │           20 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-15                                  │ Window Manager   │ DWM-15          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │           10 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-2                                   │ Window Manager   │ DWM-2           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            8 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-3                                   │ Window Manager   │ DWM-3           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-4                                   │ Window Manager   │ DWM-4           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-5                                   │ Window Manager   │ DWM-5           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-6                                   │ Window Manager   │ DWM-6           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-7                                   │ Window Manager   │ DWM-7           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-8                                   │ Window Manager   │ DWM-8           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-9                                   │ Window Manager   │ DWM-9           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-0                                   │ Font Driver Host │ UMFD-0          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\wininit.exe                              │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-1                                   │ Font Driver Host │ UMFD-1          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-10                                  │ Font Driver Host │ UMFD-10         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-11                                  │ Font Driver Host │ UMFD-11         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-12                                  │ Font Driver Host │ UMFD-12         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-13                                  │ Font Driver Host │ UMFD-13         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-14                                  │ Font Driver Host │ UMFD-14         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │           10 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-15                                  │ Font Driver Host │ UMFD-15         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            5 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-2                                   │ Font Driver Host │ UMFD-2          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-3                                   │ Font Driver Host │ UMFD-3          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-4                                   │ Font Driver Host │ UMFD-4          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-5                                   │ Font Driver Host │ UMFD-5          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-6                                   │ Font Driver Host │ UMFD-6          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-7                                   │ Font Driver Host │ UMFD-7          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-8                                   │ Font Driver Host │ UMFD-8          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-9                                   │ Font Driver Host │ UMFD-9          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-18                                       │ SHIELDBASE.COM   │ RD01$           │ -                        │ -                      │ -               │ -                         │ -                                                            │ Kerberos                  │ Kerberos         │           81 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-18                                       │ SHIELDBASE.COM   │ RD01$           │ -                        │ -                      │ -               │ ::1                       │ -                                                            │ Kerberos                  │ Kerberos         │          736 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ SHIELDBASE.COM   │ cbarton-a       │ -                        │ -                      │ -               │ 172.16.5.25               │ -                                                            │ Kerberos                  │ Kerberos         │           26 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a        │ -                        │ -                      │ -               │ 172.16.4.4                │ -                                                            │ Kerberos                  │ Kerberos         │           25 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a        │ -                        │ -                      │ WAC01           │ 172.16.4.7                │ -                                                            │ NTLM                      │ NtLmSsp          │            4 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a        │ -                        │ -                      │ -               │ 172.16.4.7                │ -                                                            │ Kerberos                  │ Kerberos         │           48 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a        │ -                        │ -                      │ RD08            │ 172.16.6.18               │ -                                                            │ NTLM                      │ NtLmSsp          │            3 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a        │ -                        │ -                      │ -               │ 172.16.6.18               │ -                                                            │ Kerberos                  │ Kerberos         │           82 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.14              │ -                                                            │ NTLM                      │ NtLmSsp          │            6 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.20              │ -                                                            │ NTLM                      │ NtLmSsp          │           14 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.23              │ -                                                            │ NTLM                      │ NtLmSsp          │            5 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.3               │ -                                                            │ NTLM                      │ NtLmSsp          │            8 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.8               │ -                                                            │ NTLM                      │ NtLmSsp          │           14 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1159 │ SHIELDBASE.COM   │ HUNT01$         │ -                        │ -                      │ -               │ 172.16.5.25               │ -                                                            │ Kerberos                  │ Kerberos         │            2 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ SHIELDBASE.COM   │ slevine         │ -                        │ -                      │ -               │ 172.16.6.18               │ -                                                            │ Kerberos                  │ Kerberos         │            1 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ SHIELDBASE.COM   │ DEV01$          │ -                        │ -                      │ -               │ 172.16.4.9                │ -                                                            │ Kerberos                  │ Kerberos         │            3 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ shieldbase       │ DEV01$          │ -                        │ -                      │ DEV01           │ 172.16.4.9                │ -                                                            │ NTLM                      │ NtLmSsp          │            1 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc          │ -                        │ -                      │ -               │ 172.16.6.18               │ -                                                            │ Kerberos                  │ Kerberos         │            1 │
│ rd01.shieldbase.com │         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc          │ -                        │ -                      │ -               │ fe80::7e6b:763c:b405:22b4 │ -                                                            │ Kerberos                  │ Kerberos         │            1 │
│ rd01.shieldbase.com │         5 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-18                                       │ NT AUTHORITY     │ SYSTEM          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\services.exe                             │ Negotiate                 │ Advapi           │         4217 │
│ rd01.shieldbase.com │         5 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-19                                       │ NT AUTHORITY     │ LOCAL SERVICE   │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\services.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         5 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-20                                       │ NT AUTHORITY     │ NETWORK SERVICE │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\services.exe                             │ Negotiate                 │ Advapi           │            4 │
│ rd01.shieldbase.com │         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ shieldbase               │ wacsvc                 │ -               │ -                         │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ shieldbase               │ wacsvc                 │ -               │ -                         │ C:\Windows\explorer.exe                                      │ Negotiate                 │ Advapi           │            1 │
│ rd01.shieldbase.com │         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ shieldbase               │ wacsvc                 │ -               │ ::1                       │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ seclogo          │           17 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.14              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            3 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.20              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            5 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.23              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            2 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.3               │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            4 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.8               │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            7 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc          │ -                        │ -                      │ RD01            │ 172.16.4.9                │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            2 │
│ rd01.shieldbase.com │        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc          │ -                        │ -                      │ RD01            │ 172.16.6.18               │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │           12 │
│ rd01.shieldbase.com │        11 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ ::1                       │ C:\Windows\System32\consent.exe                              │ Negotiate                 │ CredPro          │            1 │
│ rd01.shieldbase.com │        12 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.20              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            1 │
└─────────────────────┴───────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴────────────────────────────────────────────────┴──────────────────┴─────────────────┴──────────────────────────┴────────────────────────┴─────────────────┴───────────────────────────┴──────────────────────────────────────────────────────────────┴───────────────────────────┴──────────────────┴──────────────┘
  66 rows                                                                                                                                                                                                                                                                                                                                                                                                                          16 columns
```
fullkey
```sql
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4625
```sql
security D
SELECT Event.System.Computer, Event.EventData.LogonType, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.WorkstationName, Event.EventData.IpAddress, Event.EventData.ProcessName, Event.EventData.AuthenticationPackageName, Event.EventData.LogonProcessName, Event.EventData.Status, Event.EventData.SubStatus, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4625
GROUP BY ALL
ORDER BY  Event.EventData.LogonType, Event.EventData.SubjectUserSid , Event.EventData.TargetUserSid, Event.EventData.IpAddress;
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4634
```sql
security D
SELECT Event.System.Computer, Event.EventData.LogonType, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4647
```sql
security D
SELECT Event.System.Computer, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4648
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TargetServerName, Event.EventData.TargetInfo, Event.EventData.ProcessName, Event.EventData.IpAddress, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4672
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.PrivilegeList, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4688
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TokenElevationType, Event.EventData.MandatoryLabel, Event.EventData.ParentProcessName, Event.EventData.NewProcessName, Event.EventData.CommandLine, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4697
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.ServiceAccount, Event.EventData.ServiceName, Event.EventData.ServiceFileName, Event.EventData.ServiceType, Event.EventData.ServiceStartType, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1)
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
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4724
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4724
GROUP BY ALL
ORDER BY ALL;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬───────────────────────────────────────────────┬──────────────────┬────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                   TargetSid                   │ TargetDomainName │ TargetUserName │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │                    varchar                    │     varchar      │    varchar     │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼───────────────────────────────────────────────┼──────────────────┼────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ RD01             │ SRLAdmin       │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴───────────────────────────────────────────────┴──────────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4725
```sql
security D SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, COUNT(*)
           FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
           WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
           AND Event.System.EventID = 4725
           GROUP BY ALL
           ORDER BY ALL;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬──────────────────────────────────────────────┬──────────────────┬────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                  TargetSid                   │ TargetDomainName │ TargetUserName │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │                   varchar                    │     varchar      │    varchar     │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼──────────────────────────────────────────────┼──────────────────┼────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-500 │ RD01             │ Administrator  │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴──────────────────────────────────────────────┴──────────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security D SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4738
```sql
security D
SELECT Event.System.Computer,Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.OldUacValue, Event.EventData.NewUacValue, Event.EventData.UserAccountControl, Event.EventData.SamAccountName, Event.EventData.PasswordLastSet, Event.EventData.PrimaryGroupId, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4738
GROUP BY ALL
ORDER BY ALL;
┌─────────────────────┬────────────────┬───────────────────┬─────────────────┬───────────────────────────────────────────────┬──────────────────┬────────────────┬─────────────┬─────────────┬────────────────────┬────────────────┬─────────────────────┬────────────────┬──────────────┐
│      Computer       │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │                   TargetSid                   │ TargetDomainName │ TargetUserName │ OldUacValue │ NewUacValue │ UserAccountControl │ SamAccountName │   PasswordLastSet   │ PrimaryGroupId │ count_star() │
│       varchar       │    varchar     │      varchar      │     varchar     │                    varchar                    │     varchar      │    varchar     │   varchar   │   varchar   │      varchar       │    varchar     │       varchar       │    varchar     │    int64     │
├─────────────────────┼────────────────┼───────────────────┼─────────────────┼───────────────────────────────────────────────┼──────────────────┼────────────────┼─────────────┼─────────────┼────────────────────┼────────────────┼─────────────────────┼────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-1001 │ RD01             │ SRLAdmin       │ 0x10        │ 0x10        │ -                  │ SRLAdmin       │ 1/2/2023 2:59:48 PM │ 513            │            1 │
│ rd01.shieldbase.com │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-5-21-2908624845-2463485410-257172065-500  │ RD01             │ Administrator  │ 0x210       │ 0x211       │ \r\n\t\t%%2080     │ -              │ -                   │ -              │            1 │
└─────────────────────┴────────────────┴───────────────────┴─────────────────┴───────────────────────────────────────────────┴──────────────────┴────────────────┴─────────────┴─────────────┴────────────────────┴────────────────┴─────────────────────┴────────────────┴──────────────┘
```
fullkey
```sql
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
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



## 4776
```sql
security D
SELECT Event.System.Computer, Event.EventData.Workstation, Event.EventData.TargetUserName, Event.EventData.Status, Event.EventData.PackageName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4776
GROUP BY ALL
ORDER BY ALL;
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



## 4799
```sql
security D
SELECT Event.System.Computer, Event.EventData.SubjectUserSid, Event.EventData.TargetDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.CallerProcessName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', sample_size = -1, map_inference_threshold = -1)
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


