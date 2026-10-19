```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f system.json C:\Windows\System32\winevt\Logs\System.evtx
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off"
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
memory D
```



## EventID
```sql
memory D WITH evtx AS (
             SELECT Event.System.Computer AS Computer, Event.System.Provider."#attributes".Name, Event.System.TimeCreated."#attributes".SystemTime,
                 TRY_CAST(
                     CASE
                         WHEN json_type(Event.System.EventID) = 'OBJECT'
                         THEN json_extract_string(Event.System.EventID, '$."#text"')
                         ELSE json_extract_string(Event.System.EventID, '$')
                     END AS BIGINT
                 ) AS EventID
             FROM read_json('.\system.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         )
         SELECT Name, EventID, COUNT(*)
         FROM evtx
         WHERE SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY EventID;
┌────────────────────────────────────────────────┬─────────┬──────────────┐
│                      Name                      │ EventID │ count_star() │
│                    varchar                     │  int64  │    int64     │
├────────────────────────────────────────────────┼─────────┼──────────────┤
│ Microsoft-Windows-Kernel-General               │       1 │          143 │
│ Microsoft-Windows-FilterManager                │       1 │           96 │
│ Virtual Disk Service                           │       3 │           11 │
│ Virtual Disk Service                           │       4 │            4 │
│ Volsnap                                        │       6 │            1 │
│ Microsoft-Windows-FilterManager                │       6 │         1053 │
│ Microsoft-Windows-Kernel-General               │      11 │          188 │
│ Microsoft-Windows-UserModePowerService         │      12 │           60 │
│ Microsoft-Windows-Time-Service                 │      12 │            5 │
│ Microsoft-Windows-Kernel-General               │      12 │           96 │
│ Microsoft-Windows-Kernel-General               │      13 │           91 │
│ Microsoft-Windows-Wininit                      │      14 │           96 │
│ Microsoft-Windows-Kernel-General               │      15 │          289 │
│ Microsoft-Windows-Kernel-General               │      16 │         2673 │
│ Microsoft-Windows-HAL                          │      16 │           93 │
│ Microsoft-Windows-Kernel-Boot                  │      18 │           96 │
│ Microsoft-Windows-WindowsUpdateClient          │      19 │           27 │
│ Microsoft-Windows-Kernel-Boot                  │      20 │           96 │
│ Microsoft-Windows-Kernel-General               │      20 │           96 │
│ Microsoft-Windows-Time-Service                 │      24 │            6 │
│ Microsoft-Windows-Kernel-General               │      24 │          230 │
│ Microsoft-Windows-Kernel-Boot                  │      25 │           96 │
│ Application Popup                              │      26 │           47 │
│ Microsoft-Windows-Kernel-Boot                  │      27 │           96 │
│ vxn                                            │      31 │            3 │
│ Microsoft-Windows-Kernel-Boot                  │      32 │           96 │
│ Volsnap                                        │      33 │          215 │
│ Microsoft-Windows-Time-Service                 │      35 │          153 │
│ Microsoft-Windows-Time-Service                 │      36 │           33 │
│ Volsnap                                        │      36 │           52 │
│ Microsoft-Windows-Time-Service                 │      37 │          330 │
│ Microsoft-Windows-Kernel-Power                 │      41 │            2 │
│ Microsoft-Windows-WindowsUpdateClient          │      43 │           27 │
│ Microsoft-Windows-WindowsUpdateClient          │      44 │           63 │
│ Microsoft-Windows-Kernel-Processor-Power       │      55 │          216 │
│ Microsoft-Windows-Ntfs                         │      98 │          200 │
│ Application Management Group Policy            │     101 │         3836 │
│ Application Management Group Policy            │     103 │         3836 │
│ Microsoft-Windows-Eventlog                     │     104 │            3 │
│ Application Management Group Policy            │     108 │         2267 │
│ Microsoft-Windows-Kernel-Power                 │     109 │           92 │
│ Microsoft-Windows-Time-Service                 │     129 │            2 │
│ Microsoft-Windows-Time-Service                 │     134 │           15 │
│ Microsoft-Windows-Time-Service                 │     139 │            4 │
│ Microsoft-Windows-Time-Service                 │     143 │            4 │
│ Microsoft-Windows-Time-Service                 │     144 │            4 │
│ Microsoft-Windows-Kernel-Boot                  │     153 │           96 │
│ Microsoft-Windows-Kernel-Power                 │     172 │           96 │
│ Win32k                                         │     243 │            2 │
│ Microsoft-Windows-Hyper-V-VmSwitch             │     277 │            2 │
│ Application Management Group Policy            │     301 │           51 │
│ Application Management Group Policy            │     302 │           51 │
│ Application Management Group Policy            │     303 │           24 │
│ Application Management Group Policy            │     308 │           57 │
│ Microsoft-Windows-DNS-Client                   │    1014 │        29123 │
│ Microsoft-Windows-GroupPolicy                  │    1030 │            1 │
│ Microsoft-Windows-GroupPolicy                  │    1053 │            2 │
│ Microsoft-Windows-GroupPolicy                  │    1054 │            6 │
│ Microsoft-Windows-GroupPolicy                  │    1068 │            2 │
│ User32                                         │    1074 │           95 │
│ User32                                         │    1077 │            1 │
│ Microsoft-Windows-GroupPolicy                  │    1085 │          321 │
│ Microsoft-Windows-GroupPolicy                  │    1112 │         2267 │
│ Microsoft-Windows-GroupPolicy                  │    1125 │            1 │
│ Microsoft-Windows-GroupPolicy                  │    1127 │            5 │
│ Microsoft-Windows-GroupPolicy                  │    1129 │            3 │
│ Microsoft-Windows-GroupPolicy                  │    1500 │         8121 │
│ Microsoft-Windows-GroupPolicy                  │    1501 │         7539 │
│ Microsoft-Windows-GroupPolicy                  │    1502 │          322 │
│ Microsoft-Windows-GroupPolicy                  │    1503 │           31 │
│ Microsoft-Windows-Resource-Exhaustion-Detector │    2004 │         2338 │
│ Microsoft-Windows-WAS                          │    5011 │            5 │
│ Microsoft-Windows-WAS                          │    5074 │            2 │
│ ena                                            │    5200 │           57 │
│ ena                                            │    5202 │            8 │
│ ena                                            │    5203 │           65 │
│ Microsoft-Windows-WAS                          │    5211 │            2 │
│ NETLOGON                                       │    5513 │            3 │
│ NETLOGON                                       │    5719 │            7 │
│ NETLOGON                                       │    5774 │            1 │
│ NETLOGON                                       │    5823 │           18 │
│ NETLOGON                                       │    5836 │            4 │
│ EventLog                                       │    6005 │           96 │
│ EventLog                                       │    6006 │           94 │
│ EventLog                                       │    6008 │            2 │
│ EventLog                                       │    6009 │           96 │
│ EventLog                                       │    6013 │          979 │
│ LsaSrv                                         │    6038 │           70 │
│ LsaSrv                                         │    6148 │           83 │
│ Service Control Manager                        │    7000 │           37 │
│ Microsoft-Windows-Winlogon                     │    7001 │          137 │
│ Microsoft-Windows-Winlogon                     │    7002 │          148 │
│ Service Control Manager                        │    7009 │           50 │
│ Service Control Manager                        │    7011 │            9 │
│ Service Control Manager                        │    7023 │           26 │
│ Service Control Manager                        │    7024 │           58 │
│ Service Control Manager                        │    7026 │           96 │
│ Service Control Manager                        │    7031 │         1380 │
│ Service Control Manager                        │    7034 │            8 │
│ VfpExt                                         │    7036 │            4 │
│ Service Control Manager                        │    7036 │        36111 │
│ Service Control Manager                        │    7038 │            8 │
│ Service Control Manager                        │    7040 │          938 │
│ Service Control Manager                        │    7043 │            3 │
│ Service Control Manager                        │    7045 │          245 │
│ Service Control Manager                        │    7046 │            1 │
│ Microsoft-Windows-DNS-Client                   │    8015 │           33 │
│ Microsoft-Windows-DNS-Client                   │    8020 │            2 │
│ Microsoft-Windows-DNS-Client                   │    8033 │            1 │
│ Microsoft-Windows-DistributedCOM               │   10000 │           22 │
│ Microsoft-Windows-DistributedCOM               │   10001 │            9 │
│ Microsoft-Windows-DistributedCOM               │   10005 │            5 │
│ Microsoft-Windows-DistributedCOM               │   10010 │          189 │
│ Microsoft-Windows-DistributedCOM               │   10016 │         1660 │
│ Microsoft-Windows-DriverFrameworks-UserMode    │   10110 │            1 │
│ Microsoft-Windows-DriverFrameworks-UserMode    │   10111 │            1 │
│ Microsoft-Windows-WinRM                        │   10121 │           79 │
│ Microsoft-Windows-WinRM                        │   10148 │          102 │
│ Microsoft-Windows-WinRM                        │   10149 │           84 │
│ Microsoft-Windows-WinRM                        │   10154 │           13 │
│ Microsoft-Windows-DfsSvc                       │   14531 │            4 │
│ Microsoft-Windows-DfsSvc                       │   14533 │            4 │
│ Microsoft-Windows-Directory-Services-SAM       │   16962 │           96 │
│ Microsoft-Windows-Directory-Services-SAM       │   16977 │          101 │
│ Microsoft-Windows-Directory-Services-SAM       │   16983 │           96 │
│ Schannel                                       │   36871 │          128 │
│ LsaSrv                                         │   40960 │            1 │
│ Microsoft-Windows-Dhcp-Client                  │   50036 │          153 │
│ Microsoft-Windows-Dhcp-Client                  │   50037 │           94 │
│ Microsoft-Windows-Dhcp-Client                  │   50103 │           96 │
│ Microsoft-Windows-Dhcp-Client                  │   50104 │           94 │
│ Microsoft-Windows-Dhcp-Client                  │   50105 │           94 │
│ Microsoft-Windows-Dhcp-Client                  │   50106 │           94 │
│ Microsoft-Windows-DHCPv6-Client                │   51046 │           96 │
│ Microsoft-Windows-DHCPv6-Client                │   51047 │           94 │
│ Microsoft-Windows-DHCPv6-Client                │   51057 │           94 │
└────────────────────────────────────────────────┴─────────┴──────────────┘
  136 rows                                                      3 columns
```



## 26 (Application Error)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Caption, Event.EventData.Message, COUNT(*)
         FROM read_json('.\system-rd05.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Application Popup' AND (Event.System.EventID->>'$' = '26')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬──────────────────────────────────────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │                   Caption                    │                                                                              Message                                                                               │ count_star() │
│       varchar       │                   varchar                    │                                                                              varchar                                                                               │    int64     │
├─────────────────────┼──────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd05.shieldbase.com │ CompatTelRunner.exe - Fatal Application Exit │ Exception Processing Message 0x40000015 - Unexpected parameters                                                                                                    │            1 │
│ rd05.shieldbase.com │ PhoneExperienceHost.exe - Application Error  │ The exception unknown software exception (0xe0434352) occurred in the application at location 0x00007FFC6495428C.\r\n                                              │            1 │
│ rd05.shieldbase.com │ Windows - Out of Virtual Memory              │ Your system is low on virtual memory. To ensure that Windows runs properly, increase the size of your virtual memory paging file. For more information, see Help.  │            1 │
└─────────────────────┴──────────────────────────────────────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd05.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Application Popup') AND (e.json->>'$.Event.System.EventID' = '26') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────┬──────────────┐
│          fullkey          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ $.Event.EventData.Caption │            3 │
│ $.Event.EventData.Message │            3 │
└───────────────────────────┴──────────────┘
```



## 104 (Event log cleared)
```sql
memory D SELECT Event.System.Computer, Event.UserData.LogFileCleared.SubjectDomainName, Event.UserData.LogFileCleared.SubjectUserName, Event.UserData.LogFileCleared.Channel, COUNT(*)
         FROM read_json('.\system-dev01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-Eventlog' AND (Event.System.EventID->>'$' = '104')
         GROUP BY ALL
         ORDER BY ALL;
┌──────────────────────┬───────────────────┬─────────────────┬──────────────────────────────────────────┬──────────────┐
│       Computer       │ SubjectDomainName │ SubjectUserName │                 Channel                  │ count_star() │
│       varchar        │      varchar      │     varchar     │                 varchar                  │    int64     │
├──────────────────────┼───────────────────┼─────────────────┼──────────────────────────────────────────┼──────────────┤
│ dev01.shieldbase.com │ shieldbase        │ wacsvc          │ Microsoft-Windows-PowerShell/Operational │            1 │
│ dev01.shieldbase.com │ shieldbase        │ wacsvc          │ Microsoft-Windows-Sysmon/Operational     │            1 │
└──────────────────────┴───────────────────┴─────────────────┴──────────────────────────────────────────┴──────────────┘
```
fullkey
```
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-dev01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-Eventlog') AND (e.json->>'$.Event.System.EventID' = '104') AND je.fullkey LIKE '$.Event.UserData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────────────────────────┬──────────────┐
│                      fullkey                      │ count_star() │
│                      varchar                      │    int64     │
├───────────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.LogFileCleared                   │            5 │
│ $.Event.UserData.LogFileCleared.#attributes       │            5 │
│ $.Event.UserData.LogFileCleared.#attributes.xmlns │            5 │
│ $.Event.UserData.LogFileCleared.BackupPath        │            5 │
│ $.Event.UserData.LogFileCleared.Channel           │            5 │
│ $.Event.UserData.LogFileCleared.SubjectDomainName │            5 │
│ $.Event.UserData.LogFileCleared.SubjectUserName   │            5 │
└───────────────────────────────────────────────────┴──────────────┘
```



## 1116 (Microsoft Antimalware Detection)
Provider: "Microsoft Antimalware"



## 1129 (Absence of network connectivity)
```sql
memory D SELECT Event.System.Computer, Event.EventData.SupportInfo1, Event.EventData.SupportInfo2, Event.EventData.ProcessingMode, Event.EventData.ProcessingTimeInMilliseconds, Event.EventData.ErrorCode, Event.EventData.ErrorDescription, COUNT(*)
         FROM read_json('.\system-rd09.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-GroupPolicy' AND (Event.System.EventID->>'$' = '1129')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬──────────────┬──────────────┬────────────────┬──────────────────────────────┬───────────┬─────────────────────────────────────────────┬──────────────┐
│      Computer       │ SupportInfo1 │ SupportInfo2 │ ProcessingMode │ ProcessingTimeInMilliseconds │ ErrorCode │              ErrorDescription               │ count_star() │
│       varchar       │    int64     │    int64     │     int64      │            int64             │   int64   │                   varchar                   │    int64     │
├─────────────────────┼──────────────┼──────────────┼────────────────┼──────────────────────────────┼───────────┼─────────────────────────────────────────────┼──────────────┤
│ rd09.shieldbase.com │            1 │         2057 │              2 │                          422 │      1222 │ The network is not present or not started.  │            1 │
│ rd09.shieldbase.com │            1 │         2057 │              2 │                          609 │      1222 │ The network is not present or not started.  │            1 │
│ rd09.shieldbase.com │            1 │         2057 │              2 │                         1125 │      1222 │ The network is not present or not started.  │            1 │
└─────────────────────┴──────────────┴──────────────┴────────────────┴──────────────────────────────┴───────────┴─────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd09.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-GroupPolicy') AND (e.json->>'$.Event.System.EventID' = '1129') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────────────────┬──────────────┐
│                    fullkey                     │ count_star() │
│                    varchar                     │    int64     │
├────────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.ErrorCode                    │            3 │
│ $.Event.EventData.ErrorDescription             │            3 │
│ $.Event.EventData.ProcessingMode               │            3 │
│ $.Event.EventData.ProcessingTimeInMilliseconds │            3 │
│ $.Event.EventData.SupportInfo1                 │            3 │
│ $.Event.EventData.SupportInfo2                 │            3 │
└────────────────────────────────────────────────┴──────────────┘
```



## 6005 (The Event log service was started)
```sql
memory D SELECT Event.System.Computer, Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'EventLog' AND (Event.System.EventID->>'$."#text"' = '6005')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────────────┬──────────────┐
│      Computer       │         SystemTime         │ count_star() │
│       varchar       │         timestamp          │    int64     │
├─────────────────────┼────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-02 18:24:28.478925 │            1 │
│ rd01.shieldbase.com │ 2023-01-23 14:51:18.603897 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:19:08.712955 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:38:30.479009 │            1 │
└─────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'EventLog') AND (e.json->>'$.Event.System.EventID."#text"' = '6005') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────────┬──────────────┐
│           fullkey            │ count_star() │
│           varchar            │    int64     │
├──────────────────────────────┼──────────────┤
│ $.Event.EventData.Binary     │           26 │
│ $.Event.EventData.Data       │           26 │
│ $.Event.EventData.Data.#text │           26 │
└──────────────────────────────┴──────────────┘
```



## 6006 (The Event log service was stopped)
```sql
memory D SELECT Event.System.Computer, Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'EventLog' AND (Event.System.EventID->>'$."#text"' = '6006')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────────────┬──────────────┐
│      Computer       │         SystemTime         │ count_star() │
│       varchar       │         timestamp          │    int64     │
├─────────────────────┼────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-02 18:24:07.618005 │            1 │
│ rd01.shieldbase.com │ 2023-01-23 14:50:54.20556  │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:18:51.756586 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:33:34.463867 │            1 │
└─────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'EventLog') AND (e.json->>'$.Event.System.EventID."#text"' = '6006') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────────┬──────────────┐
│           fullkey            │ count_star() │
│           varchar            │    int64     │
├──────────────────────────────┼──────────────┤
│ $.Event.EventData.Binary     │           24 │
│ $.Event.EventData.Data       │           24 │
│ $.Event.EventData.Data.#text │           24 │
└──────────────────────────────┴──────────────┘
```



## 6008 (Unexpected system shutdown)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Data."#text"[0], Event.EventData.Data."#text"[1], COUNT(*)
         FROM read_json('.\system-rd05.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'EventLog' AND (Event.System.EventID->>'$."#text"' = '6008')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────────────────────────────────┬─────────────────────────────────────┬──────────────┐
│      Computer       │ "Event".EventData."Data"."#text"[0] │ "Event".EventData."Data"."#text"[1] │ count_star() │
│       varchar       │                json                 │                json                 │    int64     │
├─────────────────────┼─────────────────────────────────────┼─────────────────────────────────────┼──────────────┤
│ rd05.shieldbase.com │ "7:38:24 PM"                        │ "‎1/‎27/‎2023"                         │            1 │
└─────────────────────┴─────────────────────────────────────┴─────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd05.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'EventLog') AND (e.json->>'$.Event.System.EventID."#text"' = '6008') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────┬──────────────┐
│             fullkey             │ count_star() │
│             varchar             │    int64     │
├─────────────────────────────────┼──────────────┤
│ $.Event.EventData.Binary        │            1 │
│ $.Event.EventData.Data          │            1 │
│ $.Event.EventData.Data.#text    │            1 │
│ $.Event.EventData.Data.#text[0] │            1 │
│ $.Event.EventData.Data.#text[1] │            1 │
│ $.Event.EventData.Data.#text[2] │            1 │
│ $.Event.EventData.Data.#text[3] │            1 │
│ $.Event.EventData.Data.#text[4] │            1 │
│ $.Event.EventData.Data.#text[5] │            1 │
│ $.Event.EventData.Data.#text[6] │            1 │
└─────────────────────────────────┴──────────────┘
  10 rows                              2 columns
```



## 6011 (System Name Changed)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Data."#text"[0], Event.EventData.Data."#text"[1], COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.Provider."#attributes".Name = 'EventLog' AND (Event.System.EventID->>'$."#text"' = '6011')
         GROUP BY ALL
         ORDER BY ALL;
┌────────────┬─────────────────────────────────────┬─────────────────────────────────────┬──────────────┐
│  Computer  │ "Event".EventData."Data"."#text"[0] │ "Event".EventData."Data"."#text"[1] │ count_star() │
│  varchar   │                json                 │                json                 │    int64     │
├────────────┼─────────────────────────────────────┼─────────────────────────────────────┼──────────────┤
│ rd01       │ "TPL-PACKER"                        │ "RD01"                              │            1 │
│ tpl-packer │ "OFFDEVS-TUHMGJE"                   │ "TPL-PACKER"                        │            1 │
└────────────┴─────────────────────────────────────┴─────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'EventLog') AND (e.json->>'$.Event.System.EventID."#text"' = '6011') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────┬──────────────┐
│             fullkey             │ count_star() │
│             varchar             │    int64     │
├─────────────────────────────────┼──────────────┤
│ $.Event.EventData.Binary        │            2 │
│ $.Event.EventData.Data          │            2 │
│ $.Event.EventData.Data.#text    │            2 │
│ $.Event.EventData.Data.#text[0] │            2 │
│ $.Event.EventData.Data.#text[1] │            2 │
└─────────────────────────────────┴──────────────┘
```



## 6013 (System uptime)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Data."#text"[4], Event.EventData.Data."#text"[6], replace(replace(CAST(from_hex(Event.EventData.Binary) AS VARCHAR), '\x00\x00', '|'), '\x00', ''), COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'EventLog' AND (Event.System.EventID->>'$."#text"' = '6013')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────────────────────────────────┬─────────────────────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │ "Event".EventData."Data"."#text"[4] │ "Event".EventData."Data"."#text"[6] │                                        "replace"("replace"(CAST(from_hex("Event".EventData."Binary") AS VARCHAR), '\x00\x00', '|'), '\x00', '')                                         │ count_star() │
│       varchar       │                json                 │                json                 │                                                                                         varchar                                                                                         │    int64     │
├─────────────────────┼─────────────────────────────────────┼─────────────────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ "1031741"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "11"                                │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1118141"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1204541"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1290941"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1377341"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1463741"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1532642"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1550142"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1619043"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1636541"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "167741"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "1722941"                           │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "254141"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "340541"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "4"                                 │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "426941"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "5"                                 │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "513341"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "599741"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "686141"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "772541"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "7733"                              │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "8"                                 │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "81342"                             │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "8496"                              │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "858941"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "94133"                             │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
│ rd01.shieldbase.com │ "945341"                            │ "300 Eastern Standard Time"         │ 1.1|0|Windows 10 Pro for Workstations|10.0.22000 Build 22000  |Multiprocessor Free|22000.co_release.210604-1628|630f9c10|Not Available|Not Available|9|2|4036|409|rd01.shieldbase.com|| │            1 │
└─────────────────────┴─────────────────────────────────────┴─────────────────────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
  29 rows                                                                                                                                                                                                                                                                                        5 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'EventLog') AND (e.json->>'$.Event.System.EventID."#text"' = '6013') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────┬──────────────┐
│             fullkey             │ count_star() │
│             varchar             │    int64     │
├─────────────────────────────────┼──────────────┤
│ $.Event.EventData.Binary        │          144 │
│ $.Event.EventData.Data          │          144 │
│ $.Event.EventData.Data.#text    │          144 │
│ $.Event.EventData.Data.#text[0] │          144 │
│ $.Event.EventData.Data.#text[1] │          144 │
│ $.Event.EventData.Data.#text[2] │          144 │
│ $.Event.EventData.Data.#text[3] │          144 │
│ $.Event.EventData.Data.#text[4] │          144 │
│ $.Event.EventData.Data.#text[5] │          144 │
│ $.Event.EventData.Data.#text[6] │          144 │
└─────────────────────────────────┴──────────────┘
  10 rows                              2 columns
```



## 10000 (Device driver installations begins (Device connection))
Provider: "Microsoft-Windows-DriverFrameworks-UserMode"



## 10028 (DCOM was unable to communicate with the computer)
```sql
memory D SELECT Event.System.Computer, Event.EventData.param1, Event.EventData.param2, Event.EventData.param3, Event.EventData.param4, replace(replace(CAST(from_hex(Event.EventData.Binary) AS VARCHAR), '\x00\x00', '|'), '\x00', ''), COUNT(*)
         FROM read_json('.\system-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.Provider."#attributes".Name = 'Microsoft-Windows-DistributedCOM' AND (Event.System.EventID->>'$."#text"' = '10028')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────┬──────────┬───────────────────────────────────────────────────────────┬────────────────────────────────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │ param1  │  param2  │                          param3                           │                 param4                 │                                                                                                                                                                                             "replace"("replace"(CAST(from_hex("Event".EventData."Binary") AS VARCHAR), '\x00\x00', '|'), '\x00', '')                                                                                                                                                                                              │ count_star() │
│       varchar       │ varchar │ varchar  │                          varchar                          │                varchar                 │                                                                                                                                                                                                                                              varchar                                                                                                                                                                                                                                              │    int64     │
├─────────────────────┼─────────┼──────────┼───────────────────────────────────────────────────────────┼────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ dc01.shieldbase.com │ %I      │       f4 │ C:\Windows\System32\Wbem\WMIC.exe                         │ {8BC3F05E-D86B-11D0-A075-00C04FB68820} │ <Record#1: Computer=(null);Pid=868;10/18/2022 22:46:46:343;Status=1722;Gencomp=2;Detloc=1710;Flags=0;Params=1;{Param#0:0}><Record#2: Computer=(null);Pid=868;10/18/2022 22:46:46:343;Status=1722;Gencomp=18;Detloc=1442;Flags=0;Params=1;{Param#0:%I}><Record#3: Computer=(null);Pid=868;10/18/2022 22:46:46:343;Status=1722;Gencomp=18;Detloc=322;Flags=0;Params=0;><Record#4: Computer=(null);Pid=868;10/18/2022 22:46:46:343;Status=11001;Gencomp=18;Detloc=320;Flags=0;Params=1;{Param#0:%I}> │            1 │
│ dc01.shieldbase.com │ rd10    │     100c │ C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe │ {8BC3F05E-D86B-11D0-A075-00C04FB68820} │ <Record#1: Computer=(null);Pid=888;10/2/2022 16:42:31:976;Status=1747;Gencomp=2;Detloc=701;Flags=0;Params=2;{Param#0:8}{Param#1:0}>                                                                                                                                                                                                                                                                                                                                                               │            1 │
└─────────────────────┴─────────┴──────────┴───────────────────────────────────────────────────────────┴────────────────────────────────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-dc01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-DistributedCOM') AND (e.json->>'$.Event.System.EventID."#text"' = '10028') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────┬──────────────┐
│         fullkey          │ count_star() │
│         varchar          │    int64     │
├──────────────────────────┼──────────────┤
│ $.Event.EventData.Binary │            2 │
│ $.Event.EventData.param1 │            2 │
│ $.Event.EventData.param2 │            2 │
│ $.Event.EventData.param3 │            2 │
│ $.Event.EventData.param4 │            2 │
└──────────────────────────┴──────────────┘
```



## 40960 (Security authentication error)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Protocol, Event.EventData.Target, Event.EventData.Error, COUNT(*)
         FROM read_json('.\system-rd06.json', columns = {Event: 'JSON'})
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = '"LsaSrv"' AND (Event.System.EventID->>'$' = '40960')
         GROUP BY ALL
         ORDER BY ALL;
┌───────────────────────┬────────────┬──────────────────────────────────┬─────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│       Computer        │  Protocol  │              Target              │                                      Error                                      │ count_star() │
│         json          │    json    │               json               │                                      json                                       │    int64     │
├───────────────────────┼────────────┼──────────────────────────────────┼─────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ "rd06.shieldbase.com" │ "Kerberos" │ "HTTP/exchange01.shieldbase.com" │ "\"Insufficient system resources exist to complete the API.\r\n (0xc000009a)\"" │            1 │
└───────────────────────┴────────────┴──────────────────────────────────┴─────────────────────────────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd06.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'LsaSrv') AND (e.json->>'$.Event.System.EventID' = '40960') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────┬──────────────┐
│          fullkey           │ count_star() │
│          varchar           │    int64     │
├────────────────────────────┼──────────────┤
│ $.Event.EventData.Error    │            1 │
│ $.Event.EventData.Protocol │            1 │
│ $.Event.EventData.Target   │            1 │
└────────────────────────────┴──────────────┘
```



## 45057 (Account disabled)
Provider: LsaSrv


