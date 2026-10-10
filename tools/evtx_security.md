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
 ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── security ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── main ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                           file                                                                                                                           │
│                                                                                                                                                                                                                                                          │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid varchar)), eventid bigint, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated stru… │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                         security                                                                                                                         │
│                                                                                                                                                                                                                                                          │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid varchar)), eventid bigint, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated stru… │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
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

## EventId
```sql
security D
SELECT Event.System.EventID,COUNT(*)
FROM read_json_auto('C:/Users/SANSDFIR/security.json')
GROUP BY Event.System.EventID
ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│    1100 │           24 │
│    4608 │           23 │
│    4610 │           13 │
│    4611 │          346 │
│    4614 │           26 │
│    4616 │           24 │
│    4622 │          130 │
│    4624 │        17544 │
│    4625 │           25 │
│    4634 │         5024 │
│    4647 │           43 │
│    4648 │         1315 │
│    4672 │        17113 │
│    4688 │          287 │
│    4692 │            5 │
│    4694 │            8 │
│    4695 │          126 │
│    4696 │           26 │
│    4697 │          955 │
│    4717 │           10 │
│    4718 │            9 │
│    4719 │           32 │
│    4720 │            3 │
│    4722 │            3 │
│    4724 │           18 │
│    4725 │            4 │
│    4726 │            1 │
│    4728 │            3 │
│    4729 │            1 │
│    4731 │           11 │
│    4732 │           12 │
│    4733 │            3 │
│    4735 │           49 │
│    4737 │            2 │
│    4738 │           35 │
│    4739 │            4 │
│    4776 │           16 │
│    4778 │           35 │
│    4779 │           37 │
│    4781 │           20 │
│    4793 │            1 │
│    4797 │          105 │
│    4798 │          409 │
│    4799 │         6625 │
│    4800 │           36 │
│    4825 │            7 │
│    4826 │           26 │
│    4902 │           23 │
│    4904 │           16 │
│    4905 │           16 │
│    4907 │        30602 │
│    4944 │           13 │
│    4945 │         2728 │
│    4946 │         1014 │
│    4947 │           74 │
│    4948 │          690 │
│    4954 │           24 │
│    4956 │           13 │
│    5024 │           10 │
│    5033 │           10 │
│    5058 │           47 │
│    5059 │           26 │
│    5061 │          157 │
│    5140 │           71 │
│    5142 │           42 │
│    5144 │            3 │
│    5379 │        81294 │
│    5381 │           23 │
│    5382 │          460 │
│    5478 │           13 │
└─────────┴──────────────┘
  70 rows      2 columns
```


## 4624
```sql
security D
security D
SELECT Event.EventData.LogonType, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TargetOutboundDomainName, Event.EventData.TargetOutboundUserName, Event.EventData.WorkstationName, Event.EventData.IpAddress, Event.EventData.ProcessName, Event.EventData.AuthenticationPackageName, Event.EventData.LogonProcessName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4624
GROUP BY ALL
ORDER BY  Event.EventData.LogonType, Event.EventData.SubjectUserSid , Event.EventData.TargetUserSid, Event.EventData.IpAddress;
┌───────────┬────────────────────────────────────────────────┬───────────────────┬─────────────────┬────────────────────────────────────────────────┬──────────────────┬─────────────────┬──────────────────────────┬────────────────────────┬─────────────────┬───────────────────────────┬──────────────────────────────────────────────────────────────┬───────────────────────────┬──────────────────┬──────────────┐
│ LogonType │                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │                 TargetUserSid                  │ TargetDomainName │ TargetUserName  │ TargetOutboundDomainName │ TargetOutboundUserName │ WorkstationName │         IpAddress         │                         ProcessName                          │ AuthenticationPackageName │ LogonProcessName │ count_star() │
│   int64   │                    varchar                     │      varchar      │     varchar     │                    varchar                     │     varchar      │     varchar     │         varchar          │        varchar         │     varchar     │          varchar          │                           varchar                            │          varchar          │     varchar      │    int64     │
├───────────┼────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────┼──────────────────┼─────────────────┼──────────────────────────┼────────────────────────┼─────────────────┼───────────────────────────┼──────────────────────────────────────────────────────────────┼───────────────────────────┼──────────────────┼──────────────┤
│         0 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-18                                       │ NT AUTHORITY     │ SYSTEM          │ -                        │ -                      │ -               │ -                         │                                                              │ -                         │ -                │            4 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-1                                   │ Window Manager   │ DWM-1           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            8 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-10                                  │ Window Manager   │ DWM-10          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-11                                  │ Window Manager   │ DWM-11          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            8 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-12                                  │ Window Manager   │ DWM-12          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-13                                  │ Window Manager   │ DWM-13          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-14                                  │ Window Manager   │ DWM-14          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │           20 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-15                                  │ Window Manager   │ DWM-15          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │           10 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-2                                   │ Window Manager   │ DWM-2           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            8 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-3                                   │ Window Manager   │ DWM-3           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-4                                   │ Window Manager   │ DWM-4           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-5                                   │ Window Manager   │ DWM-5           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-6                                   │ Window Manager   │ DWM-6           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-7                                   │ Window Manager   │ DWM-7           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-8                                   │ Window Manager   │ DWM-8           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-90-0-9                                   │ Window Manager   │ DWM-9           │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-0                                   │ Font Driver Host │ UMFD-0          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\wininit.exe                              │ Negotiate                 │ Advapi           │            4 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-1                                   │ Font Driver Host │ UMFD-1          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-10                                  │ Font Driver Host │ UMFD-10         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-11                                  │ Font Driver Host │ UMFD-11         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-12                                  │ Font Driver Host │ UMFD-12         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-13                                  │ Font Driver Host │ UMFD-13         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-14                                  │ Font Driver Host │ UMFD-14         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │           10 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-15                                  │ Font Driver Host │ UMFD-15         │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            5 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-2                                   │ Font Driver Host │ UMFD-2          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            4 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-3                                   │ Font Driver Host │ UMFD-3          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-4                                   │ Font Driver Host │ UMFD-4          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-5                                   │ Font Driver Host │ UMFD-5          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            2 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-6                                   │ Font Driver Host │ UMFD-6          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-7                                   │ Font Driver Host │ UMFD-7          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-8                                   │ Font Driver Host │ UMFD-8          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│         2 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-96-0-9                                   │ Font Driver Host │ UMFD-9          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\winlogon.exe                             │ Negotiate                 │ Advapi           │            1 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-18                                       │ SHIELDBASE.COM   │ RD01$           │ -                        │ -                      │ -               │ -                         │ -                                                            │ Kerberos                  │ Kerberos         │           81 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-18                                       │ SHIELDBASE.COM   │ RD01$           │ -                        │ -                      │ -               │ ::1                       │ -                                                            │ Kerberos                  │ Kerberos         │          736 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1108 │ SHIELDBASE.COM   │ cbarton-a       │ -                        │ -                      │ -               │ 172.16.5.25               │ -                                                            │ Kerberos                  │ Kerberos         │           26 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a        │ -                        │ -                      │ -               │ 172.16.4.4                │ -                                                            │ Kerberos                  │ Kerberos         │           25 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a        │ -                        │ -                      │ -               │ 172.16.4.7                │ -                                                            │ Kerberos                  │ Kerberos         │           48 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a        │ -                        │ -                      │ WAC01           │ 172.16.4.7                │ -                                                            │ NTLM                      │ NtLmSsp          │            4 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase       │ rsydow-a        │ -                        │ -                      │ RD08            │ 172.16.6.18               │ -                                                            │ NTLM                      │ NtLmSsp          │            3 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1125 │ SHIELDBASE.COM   │ rsydow-a        │ -                        │ -                      │ -               │ 172.16.6.18               │ -                                                            │ Kerberos                  │ Kerberos         │           82 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.14              │ -                                                            │ NTLM                      │ NtLmSsp          │            6 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.20              │ -                                                            │ NTLM                      │ NtLmSsp          │           14 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.23              │ -                                                            │ NTLM                      │ NtLmSsp          │            5 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.3               │ -                                                            │ NTLM                      │ NtLmSsp          │            8 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ DUNGANATOR      │ 172.16.30.8               │ -                                                            │ NTLM                      │ NtLmSsp          │           14 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1159 │ SHIELDBASE.COM   │ HUNT01$         │ -                        │ -                      │ -               │ 172.16.5.25               │ -                                                            │ Kerberos                  │ Kerberos         │            2 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1213 │ SHIELDBASE.COM   │ slevine         │ -                        │ -                      │ -               │ 172.16.6.18               │ -                                                            │ Kerberos                  │ Kerberos         │            1 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ SHIELDBASE.COM   │ DEV01$          │ -                        │ -                      │ -               │ 172.16.4.9                │ -                                                            │ Kerberos                  │ Kerberos         │            3 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1216 │ shieldbase       │ DEV01$          │ -                        │ -                      │ DEV01           │ 172.16.4.9                │ -                                                            │ NTLM                      │ NtLmSsp          │            1 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc          │ -                        │ -                      │ -               │ 172.16.6.18               │ -                                                            │ Kerberos                  │ Kerberos         │            1 │
│         3 │ S-1-0-0                                        │ -                 │ -               │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ SHIELDBASE.COM   │ wacsvc          │ -                        │ -                      │ -               │ fe80::7e6b:763c:b405:22b4 │ -                                                            │ Kerberos                  │ Kerberos         │            1 │
│         5 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-18                                       │ NT AUTHORITY     │ SYSTEM          │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\services.exe                             │ Negotiate                 │ Advapi           │         4217 │
│         5 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-19                                       │ NT AUTHORITY     │ LOCAL SERVICE   │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\services.exe                             │ Negotiate                 │ Advapi           │            4 │
│         5 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-20                                       │ NT AUTHORITY     │ NETWORK SERVICE │ -                        │ -                      │ -               │ -                         │ C:\Windows\System32\services.exe                             │ Negotiate                 │ Advapi           │            4 │
│         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ shieldbase               │ wacsvc                 │ -               │ -                         │ C:\Windows\explorer.exe                                      │ Negotiate                 │ Advapi           │            1 │
│         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ shieldbase               │ wacsvc                 │ -               │ -                         │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ Negotiate                 │ Advapi           │            1 │
│         9 │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase        │ tdungan         │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ shieldbase               │ wacsvc                 │ -               │ ::1                       │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ seclogo          │           17 │
│        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.14              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            3 │
│        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.20              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            5 │
│        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.23              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            2 │
│        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.3               │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            4 │
│        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.8               │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            7 │
│        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc          │ -                        │ -                      │ RD01            │ 172.16.4.9                │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            2 │
│        10 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase       │ wacsvc          │ -                        │ -                      │ RD01            │ 172.16.6.18               │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │           12 │
│        11 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ ::1                       │ C:\Windows\System32\consent.exe                              │ Negotiate                 │ CredPro          │            1 │
│        12 │ S-1-5-18                                       │ shieldbase        │ RD01$           │ S-1-5-21-2838623409-1327563992-2591358621-1136 │ shieldbase       │ tdungan         │ -                        │ -                      │ RD01            │ 172.16.30.20              │ C:\Windows\System32\svchost.exe                              │ Negotiate                 │ User32           │            1 │
└───────────┴────────────────────────────────────────────────┴───────────────────┴─────────────────┴────────────────────────────────────────────────┴──────────────────┴─────────────────┴──────────────────────────┴────────────────────────┴─────────────────┴───────────────────────────┴──────────────────────────────────────────────────────────────┴───────────────────────────┴──────────────────┴──────────────┘
  66 rows                                                                                                                                                                                                                                                                                                                                                                                                    15 columns
```
fullkey
```sql
security D 
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '4624'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌───────────────────────────────────────────────────┬──────────────┐
│                      fullkey                      │ count_star() │
│                      varchar                      │    int64     │
├───────────────────────────────────────────────────┼──────────────┤
│ $                                                 │        17544 │
│ $.Event                                           │        17544 │
│ $.Event.#attributes                               │        17544 │
│ $.Event.#attributes.xmlns                         │        17544 │
│ $.Event.EventData                                 │        17544 │
│ $.Event.EventData.AuthenticationPackageName       │        17544 │
│ $.Event.EventData.ElevatedToken                   │        17544 │
│ $.Event.EventData.ImpersonationLevel              │        17544 │
│ $.Event.EventData.IpAddress                       │        17544 │
│ $.Event.EventData.IpPort                          │        17544 │
│ $.Event.EventData.KeyLength                       │        17544 │
│ $.Event.EventData.LmPackageName                   │        17544 │
│ $.Event.EventData.LogonGuid                       │        17544 │
│ $.Event.EventData.LogonProcessName                │        17544 │
│ $.Event.EventData.LogonType                       │        17544 │
│ $.Event.EventData.ProcessId                       │        17544 │
│ $.Event.EventData.ProcessName                     │        17544 │
│ $.Event.EventData.RestrictedAdminMode             │        17544 │
│ $.Event.EventData.SubjectDomainName               │        17544 │
│ $.Event.EventData.SubjectLogonId                  │        17544 │
│ $.Event.EventData.SubjectUserName                 │        17544 │
│ $.Event.EventData.SubjectUserSid                  │        17544 │
│ $.Event.EventData.TargetDomainName                │        17544 │
│ $.Event.EventData.TargetLinkedLogonId             │        17544 │
│ $.Event.EventData.TargetLogonId                   │        17544 │
│ $.Event.EventData.TargetOutboundDomainName        │        17544 │
│ $.Event.EventData.TargetOutboundUserName          │        17544 │
│ $.Event.EventData.TargetUserName                  │        17544 │
│ $.Event.EventData.TargetUserSid                   │        17544 │
│ $.Event.EventData.TransmittedServices             │        17544 │
│ $.Event.EventData.VirtualAccount                  │        17544 │
│ $.Event.EventData.WorkstationName                 │        17544 │
│ $.Event.System                                    │        17544 │
│ $.Event.System.Channel                            │        17544 │
│ $.Event.System.Computer                           │        17544 │
│ $.Event.System.Correlation                        │        17544 │
│ $.Event.System.Correlation.#attributes            │        17521 │
│ $.Event.System.Correlation.#attributes.ActivityID │        17521 │
│ $.Event.System.EventID                            │        17544 │
│ $.Event.System.EventRecordID                      │        17544 │
│ $.Event.System.Execution                          │        17544 │
│ $.Event.System.Execution.#attributes              │        17544 │
│ $.Event.System.Execution.#attributes.ProcessID    │        17544 │
│ $.Event.System.Execution.#attributes.ThreadID     │        17544 │
│ $.Event.System.Keywords                           │        17544 │
│ $.Event.System.Level                              │        17544 │
│ $.Event.System.Opcode                             │        17544 │
│ $.Event.System.Provider                           │        17544 │
│ $.Event.System.Provider.#attributes               │        17544 │
│ $.Event.System.Provider.#attributes.Guid          │        17544 │
│ $.Event.System.Provider.#attributes.Name          │        17544 │
│ $.Event.System.Security                           │        17544 │
│ $.Event.System.Task                               │        17544 │
│ $.Event.System.TimeCreated                        │        17544 │
│ $.Event.System.TimeCreated.#attributes            │        17544 │
│ $.Event.System.TimeCreated.#attributes.SystemTime │        17544 │
│ $.Event.System.Version                            │        17544 │
└───────────────────────────────────────────────────┴──────────────┘
  57 rows                                                2 columns
```

## 4625
```sql
security D
SELECT Event.EventData.LogonType, Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TargetOutboundDomainName, Event.EventData.TargetOutboundUserName, Event.EventData.WorkstationName, Event.EventData.IpAddress, Event.EventData.ProcessName, Event.EventData.AuthenticationPackageName, Event.EventData.LogonProcessName, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4625
GROUP BY ALL
ORDER BY  Event.EventData.LogonType, Event.EventData.SubjectUserSid , Event.EventData.TargetUserSid, Event.EventData.IpAddress;
┌───────────┬────────────────┬───────────────────┬─────────────────┬───────────────┬──────────────────┬────────────────┬──────────────────────────┬────────────────────────┬─────────────────┬──────────────┬─────────────────────────────────┬───────────────────────────────────────┬──────────────────┬──────────────┐
│ LogonType │ SubjectUserSid │ SubjectDomainName │ SubjectUserName │ TargetUserSid │ TargetDomainName │ TargetUserName │ TargetOutboundDomainName │ TargetOutboundUserName │ WorkstationName │  IpAddress   │           ProcessName           │       AuthenticationPackageName       │ LogonProcessName │ count_star() │
│   int64   │    varchar     │      varchar      │     varchar     │    varchar    │     varchar      │    varchar     │         varchar          │        varchar         │     varchar     │   varchar    │             varchar             │                varchar                │     varchar      │    int64     │
├───────────┼────────────────┼───────────────────┼─────────────────┼───────────────┼──────────────────┼────────────────┼──────────────────────────┼────────────────────────┼─────────────────┼──────────────┼─────────────────────────────────┼───────────────────────────────────────┼──────────────────┼──────────────┤
│         3 │ S-1-0-0        │ -                 │ -               │ S-1-0-0       │ shieldbase       │ tdungan        │ NULL                     │ NULL                   │ DUNGANATOR      │ 172.16.30.20 │ -                               │ NTLM                                  │ NtLmSsp          │            4 │
│         3 │ S-1-5-20       │ shieldbase        │ RD01$           │ S-1-0-0       │ shieldbase       │ tdungan        │ NULL                     │ NULL                   │ RD01            │ -            │ C:\Windows\System32\svchost.exe │ Negotiate                             │ Advapi           │            1 │
│         3 │ S-1-5-20       │ shieldbase        │ RD01$           │ S-1-0-0       │                  │ sprx           │ NULL                     │ NULL                   │ RD01            │ -            │ C:\Windows\System32\svchost.exe │ MICROSOFT_AUTHENTICATION_PACKAGE_V1_0 │ Advapi           │           14 │
│        10 │ S-1-5-18       │ shieldbase        │ RD01$           │ S-1-0-0       │ shieldbase       │ tdungan        │ NULL                     │ NULL                   │ RD01            │ 172.16.30.20 │ C:\Windows\System32\svchost.exe │ Negotiate                             │ User32           │            1 │
└───────────┴────────────────┴───────────────────┴─────────────────┴───────────────┴──────────────────┴────────────────┴──────────────────────────┴────────────────────────┴─────────────────┴──────────────┴─────────────────────────────────┴───────────────────────────────────────┴──────────────────┴──────────────┘
```



## 4648
```sql
security D
SELECT Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TargetServerName, Event.EventData.TargetInfo, Event.EventData.ProcessName, Event.EventData.IpAddress, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4648
GROUP BY ALL
ORDER BY Event.EventData.SubjectUserSid, Event.EventData.TargetUserName, Event.EventData.IpAddress;
┌─────────────────────────────────────────┬───────────────────┬─────────────────┬──────────────────┬────────────────┬────────────────────────┬──────────────────────────────┬───────────────────────────────────┬───────────────────────────┬──────────────┐
│             SubjectUserSid              │ SubjectDomainName │ SubjectUserName │ TargetDomainName │ TargetUserName │    TargetServerName    │          TargetInfo          │            ProcessName            │         IpAddress         │ count_star() │
│                 varchar                 │      varchar      │     varchar     │     varchar      │    varchar     │        varchar         │           varchar            │              varchar              │          varchar          │    int64     │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-1          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-10         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-11         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-12         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-13         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-14         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │           10 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-15         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            5 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-2          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-3          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-4          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-5          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-6          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-7          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-8          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Window Manager   │ DWM-9          │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ SHIELDBASE.COM   │ RD01$          │ rd01$                  │ rd01$                        │ C:\Windows\System32\taskhostw.exe │ -                         │           81 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-0         │ localhost              │ localhost                    │ C:\Windows\System32\wininit.exe   │ -                         │            4 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-1         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-10        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-11        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-12        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-13        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-14        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │           10 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-15        │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            5 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-2         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            4 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-3         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-4         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-5         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            2 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-6         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-7         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-8         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-9         │ localhost              │ localhost                    │ C:\Windows\System32\winlogon.exe  │ -                         │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.14              │            3 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.20              │            6 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.23              │            2 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.3               │            4 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.30.8               │            7 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ localhost              │ localhost                    │ C:\Windows\System32\consent.exe   │ ::1                       │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ shieldbase       │ wacsvc         │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.4.9                │            1 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-18                                │ shieldbase        │ RD01$           │ shieldbase       │ wacsvc         │ localhost              │ localhost                    │ C:\Windows\System32\svchost.exe   │ 172.16.6.18               │            6 │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ shieldbase       │ wacsvc         │ file01.shieldbase.com  │ file01.shieldbase.com        │                                   │ 172.16.4.5                │            1 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ dev01.shieldbase.com   │ cifs/dev01.shieldbase.com    │                                   │ 172.16.4.9                │            4 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ shieldbase       │ wacsvc         │ rd02.shieldbase.com    │ rd02.shieldbase.com          │                                   │ 172.16.6.12               │            2 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ rd04.shieldbase.com    │ cifs/rd04.shieldbase.com     │                                   │ 172.16.6.14               │            1 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ rd10.shieldbase.com    │ cifs/rd10.shieldbase.com     │                                   │ 172.16.6.20               │            1 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ shieldbase       │ wacsvc         │ wkstn01.shieldbase.com │ wkstn01.shieldbase.com       │ C:\Windows\System32\wbem\WMIC.exe │ 172.16.7.11               │            1 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ wkstn01.shieldbase.com │ cifs/wkstn01.shieldbase.com  │                                   │ 172.16.7.11               │            4 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ wkstn01.shieldbase.com │ host/wkstn01.shieldbase.com  │ C:\Windows\System32\wbem\WMIC.exe │ 172.16.7.11               │            1 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ wkstn01.shieldbase.com │ RPCSS/wkstn01.shieldbase.com │ C:\Windows\System32\svchost.exe   │ 172.16.7.11               │            1 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ tdungan         │ SHIELDBASE.COM   │ wacsvc         │ rd01                   │ cifs/rd01                    │                                   │ fe80::7e6b:763c:b405:22b4 │            1 │
│ 21-1136                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
├─────────────────────────────────────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼────────────────────────┼──────────────────────────────┼───────────────────────────────────┼───────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-25913586 │ shieldbase        │ wacsvc          │ SHIELDBASE.COM   │ wacsvc         │ dev01.shieldbase.com   │ TERMSRV/dev01.shieldbase.com │ C:\Windows\System32\lsass.exe     │ -                         │            2 │
│ 21-1220                                 │                   │                 │                  │                │                        │                              │                                   │                           │              │
└─────────────────────────────────────────┴───────────────────┴─────────────────┴──────────────────┴────────────────┴────────────────────────┴──────────────────────────────┴───────────────────────────────────┴───────────────────────────┴──────────────┘
  51 rows                                                                                                                                                                                                                                       10 columns
```
fullkey
```sql
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '4648' AND je.fullkey NOT LIKE '$.Event.System%'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $                                   │         1315 │
│ $.Event                             │         1315 │
│ $.Event.#attributes                 │         1315 │
│ $.Event.#attributes.xmlns           │         1315 │
│ $.Event.EventData                   │         1315 │
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
  19 rows                                  2 columns
```



## 4672
```sql
security D
SELECT Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.PrivilegeList, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4672
GROUP BY ALL
ORDER BY Event.EventData.SubjectUserSid, Event.EventData.PrivilegeList;
┌────────────────────────────────────────────────┬───────────────────┬─────────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│                 SubjectUserSid                 │ SubjectDomainName │ SubjectUserName │                                                                   PrivilegeList                                                                    │ count_star() │
│                    varchar                     │      varchar      │     varchar     │                                                                      varchar                                                                       │    int64     │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-18                                       │ NT AUTHORITY      │ SYSTEM          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeTcbPrivilege\r\n\t\t\tSeSecurityPrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeLoadDriverPrivileg │         4217 │
│                                                │                   │                 │ e\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeSystemEnvironmentPrivilege │              │
│                                                │                   │                 │ \r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege                                                                │              │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-18                                       │ shieldbase        │ RD01$           │ SeSecurityPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSe │          817 │
│                                                │                   │                 │ SystemEnvironmentPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege       │              │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-19                                       │ NT AUTHORITY      │ LOCAL SERVICE   │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            4 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-20                                       │ NT AUTHORITY      │ NETWORK SERVICE │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            4 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-2591358621-1108 │ shieldbase        │ cbarton-a       │ SeSecurityPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSe │           26 │
│                                                │                   │                 │ SystemEnvironmentPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege       │              │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-2591358621-1125 │ shieldbase        │ rsydow-a        │ SeSecurityPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSe │          162 │
│                                                │                   │                 │ SystemEnvironmentPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege       │              │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ SeSecurityPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeDebugPrivilege\r\n\t\t\tSe │            2 │
│                                                │                   │                 │ SystemEnvironmentPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege       │              │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-21-2838623409-1327563992-2591358621-1220 │ shieldbase        │ wacsvc          │ SeSecurityPrivilege\r\n\t\t\tSeTakeOwnershipPrivilege\r\n\t\t\tSeLoadDriverPrivilege\r\n\t\t\tSeBackupPrivilege\r\n\t\t\tSeRestorePrivilege\r\n\t\ │            7 │
│                                                │                   │                 │ t\tSeDebugPrivilege\r\n\t\t\tSeSystemEnvironmentPrivilege\r\n\t\t\tSeImpersonatePrivilege\r\n\t\t\tSeDelegateSessionUserImpersonatePrivilege       │              │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-1                                   │ Window Manager    │ DWM-1           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            4 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-1                                   │ Window Manager    │ DWM-1           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            4 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-10                                  │ Window Manager    │ DWM-10          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-10                                  │ Window Manager    │ DWM-10          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-11                                  │ Window Manager    │ DWM-11          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            4 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-11                                  │ Window Manager    │ DWM-11          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            4 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-12                                  │ Window Manager    │ DWM-12          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-12                                  │ Window Manager    │ DWM-12          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-13                                  │ Window Manager    │ DWM-13          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-13                                  │ Window Manager    │ DWM-13          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-14                                  │ Window Manager    │ DWM-14          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │           10 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-14                                  │ Window Manager    │ DWM-14          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │           10 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-15                                  │ Window Manager    │ DWM-15          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            5 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-15                                  │ Window Manager    │ DWM-15          │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            5 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-2                                   │ Window Manager    │ DWM-2           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            4 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-2                                   │ Window Manager    │ DWM-2           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            4 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-3                                   │ Window Manager    │ DWM-3           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            2 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-3                                   │ Window Manager    │ DWM-3           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            2 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-4                                   │ Window Manager    │ DWM-4           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            2 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-4                                   │ Window Manager    │ DWM-4           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            2 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-5                                   │ Window Manager    │ DWM-5           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            2 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-5                                   │ Window Manager    │ DWM-5           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            2 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-6                                   │ Window Manager    │ DWM-6           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-6                                   │ Window Manager    │ DWM-6           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-7                                   │ Window Manager    │ DWM-7           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-7                                   │ Window Manager    │ DWM-7           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-8                                   │ Window Manager    │ DWM-8           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-8                                   │ Window Manager    │ DWM-8           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-9                                   │ Window Manager    │ DWM-9           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege                                                                                            │            1 │
├────────────────────────────────────────────────┼───────────────────┼─────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ S-1-5-90-0-9                                   │ Window Manager    │ DWM-9           │ SeAssignPrimaryTokenPrivilege\r\n\t\t\tSeAuditPrivilege\r\n\t\t\tSeImpersonatePrivilege                                                            │            1 │
└────────────────────────────────────────────────┴───────────────────┴─────────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
  38 rows                                                                                                                                                                                                                                        5 columns
```
fullkey
```sql
security D 
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '4672' AND je.fullkey NOT LIKE '$.Event.System%'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $                                   │        17113 │
│ $.Event                             │        17113 │
│ $.Event.#attributes                 │        17113 │
│ $.Event.#attributes.xmlns           │        17113 │
│ $.Event.EventData                   │        17113 │
│ $.Event.EventData.PrivilegeList     │        17113 │
│ $.Event.EventData.SubjectDomainName │        17113 │
│ $.Event.EventData.SubjectLogonId    │        17113 │
│ $.Event.EventData.SubjectUserName   │        17113 │
│ $.Event.EventData.SubjectUserSid    │        17113 │
└─────────────────────────────────────┴──────────────┘
  10 rows                                  2 columns
```

## 4688
```sql
security D
SELECT Event.EventData.SubjectUserSid, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetUserSid, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.TokenElevationType, Event.EventData.MandatoryLabel, Event.EventData.ParentProcessName, Event.EventData.NewProcessName, Event.EventData.CommandLine, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 4688
GROUP BY ALL
ORDER BY Event.EventData.ParentProcessName, Event.EventData.NewProcessName;
┌────────────────┬───────────────────┬─────────────────┬───────────────┬──────────────────┬────────────────┬────────────────────┬────────────────┬─────────────────────────────────┬──────────────────────────────────┬─────────────┬──────────────┐
│ SubjectUserSid │ SubjectDomainName │ SubjectUserName │ TargetUserSid │ TargetDomainName │ TargetUserName │ TokenElevationType │ MandatoryLabel │        ParentProcessName        │          NewProcessName          │ CommandLine │ count_star() │
│    varchar     │      varchar      │     varchar     │    varchar    │     varchar      │    varchar     │      varchar       │    varchar     │             varchar             │             varchar              │   varchar   │    int64     │
├────────────────┼───────────────────┼─────────────────┼───────────────┼──────────────────┼────────────────┼────────────────────┼────────────────┼─────────────────────────────────┼──────────────────────────────────┼─────────────┼──────────────┤
│ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │                                 │ C:\Windows\System32\smss.exe     │             │            4 │
│ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │                                 │ Registry                         │             │            4 │
│ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\autochk.exe  │             │            4 │
│ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\csrss.exe    │             │            8 │
│ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\smss.exe     │             │            8 │
│ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\wininit.exe  │             │            4 │
│ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\smss.exe    │ C:\Windows\System32\winlogon.exe │             │            4 │
│ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\wininit.exe │ C:\Windows\System32\lsass.exe    │             │            4 │
│ S-1-5-18       │ -                 │ -               │ S-1-0-0       │ -                │ -              │ %%1936             │ S-1-16-16384   │ C:\Windows\System32\wininit.exe │ C:\Windows\System32\services.exe │             │            4 │
└────────────────┴───────────────────┴─────────────────┴───────────────┴──────────────────┴────────────────┴────────────────────┴────────────────┴─────────────────────────────────┴──────────────────────────────────┴─────────────┴──────────────┘
```
fullkey
```sql
security D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '4688' AND je.fullkey NOT LIKE '$.Event.System%'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌──────────────────────────────────────┬──────────────┐
│               fullkey                │ count_star() │
│               varchar                │    int64     │
├──────────────────────────────────────┼──────────────┤
│ $                                    │          287 │
│ $.Event                              │          287 │
│ $.Event.#attributes                  │          287 │
│ $.Event.#attributes.xmlns            │          287 │
│ $.Event.EventData                    │          287 │
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
  20 rows                                   2 columns
```
