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



## 1 (The system time was changed)
```sql
memory D SELECT Event.System.Computer, Event.EventData.OldTime, Event.EventData.NewTime, Event.EventData.Reason, Event.EventData.ProcessName, Event.EventData.TimeZoneBias, Event.EventData.RealTimeIsUniversal, Event.EventData.SystemInCmosMode, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-Kernel-General' AND (Event.System.EventID->>'$' = '1')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────────────┬────────────────────────────┬────────┬──────────────────────────────────────────────────────┬──────────────┬─────────────────────┬──────────────────┬──────────────┐
│      Computer       │          OldTime           │          NewTime           │ Reason │                     ProcessName                      │ TimeZoneBias │ RealTimeIsUniversal │ SystemInCmosMode │ count_star() │
│       varchar       │         timestamp          │         timestamp          │ int64  │                       varchar                        │    int64     │       boolean       │     boolean      │    int64     │
├─────────────────────┼────────────────────────────┼────────────────────────────┼────────┼──────────────────────────────────────────────────────┼──────────────┼─────────────────────┼──────────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-01 05:00:09.167503 │ 2023-01-01 05:00:09.167503 │      3 │                                                      │          300 │ true                │ false            │            1 │
│ rd01.shieldbase.com │ 2023-01-02 18:24:07.586327 │ 2023-01-02 18:24:07.590471 │      1 │ \Device\HarddiskVolume1\Windows\System32\svchost.exe │          300 │ true                │ false            │            1 │
│ rd01.shieldbase.com │ 2023-01-23 14:50:54.138366 │ 2023-01-23 14:50:54.152033 │      1 │ \Device\HarddiskVolume1\Windows\System32\svchost.exe │          300 │ true                │ false            │            1 │
│ rd01.shieldbase.com │ 2023-01-23 14:51:51.381957 │ 2023-01-23 14:51:51.382336 │      1 │ \Device\HarddiskVolume1\Windows\System32\svchost.exe │          300 │ true                │ false            │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:18:51.681411 │ 2023-01-25 14:18:51.681409 │      1 │ \Device\HarddiskVolume1\Windows\System32\svchost.exe │          300 │ true                │ false            │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:33:34.369995 │ 2023-01-25 14:33:34.371775 │      1 │ \Device\HarddiskVolume1\Windows\System32\svchost.exe │          300 │ true                │ false            │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:39:21.612376 │ 2023-01-25 14:39:21.612623 │      1 │ \Device\HarddiskVolume1\Windows\System32\svchost.exe │          300 │ true                │ false            │            1 │
└─────────────────────┴────────────────────────────┴────────────────────────────┴────────┴──────────────────────────────────────────────────────┴──────────────┴─────────────────────┴──────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-Kernel-General') AND (e.json->>'$.Event.System.EventID' = '1') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────────────┬──────────────┐
│                fullkey                │ count_star() │
│                varchar                │    int64     │
├───────────────────────────────────────┼──────────────┤
│ $.Event.EventData.CmosTime            │           29 │
│ $.Event.EventData.NewTime             │           29 │
│ $.Event.EventData.OldTime             │           29 │
│ $.Event.EventData.ProcessID           │           29 │
│ $.Event.EventData.ProcessName         │           29 │
│ $.Event.EventData.RealTimeIsUniversal │           29 │
│ $.Event.EventData.Reason              │           29 │
│ $.Event.EventData.SystemInCmosMode    │           29 │
│ $.Event.EventData.TimeZoneBias        │           29 │
└───────────────────────────────────────┴──────────────┘
```
Reason
```
1: An application or system component changed the time.
2: System time synchronized with the hardware clock.
3: System time adjusted to the new time zone.
```



## 1 (Sleep/wake events)
Provider: Microsoft-Windows-Power-Troubleshooter



## 2 (An attempt to exploit a known vulnerability detected)
Provider: Microsoft-Windows-Audit-CVE



## 12 (OS was started)
```sql
memory D SELECT Event.System.Computer, Event.EventData.StartTime, Event.EventData.BootMode, Event.EventData.BuildVersion, Event.EventData.QfeVersion, Event.EventData.ServiceVersion, Event.EventData.MajorVersion, Event.EventData.MinorVersion, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-Kernel-General' AND (Event.System.EventID->>'$' = '12')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬───────────────────────┬──────────┬──────────────┬────────────┬────────────────┬──────────────┬──────────────┬──────────────┐
│      Computer       │       StartTime       │ BootMode │ BuildVersion │ QfeVersion │ ServiceVersion │ MajorVersion │ MinorVersion │ count_star() │
│       varchar       │       timestamp       │  int64   │    int64     │   int64    │     int64      │    int64     │    int64     │    int64     │
├─────────────────────┼───────────────────────┼──────────┼──────────────┼────────────┼────────────────┼──────────────┼──────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-02 18:24:19.5 │        0 │        22000 │       1335 │              0 │           10 │            0 │            1 │
│ rd01.shieldbase.com │ 2023-01-23 14:51:07.5 │        0 │        22000 │       1335 │              0 │           10 │            0 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:19:03.5 │        0 │        22000 │       1335 │              0 │           10 │            0 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:38:25.5 │        0 │        22000 │       1335 │              0 │           10 │            0 │            1 │
└─────────────────────┴───────────────────────┴──────────┴──────────────┴────────────┴────────────────┴──────────────┴──────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-Kernel-General') AND (e.json->>'$.Event.System.EventID' = '12') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────────────┬──────────────┐
│             fullkey              │ count_star() │
│             varchar              │    int64     │
├──────────────────────────────────┼──────────────┤
│ $.Event.EventData.BootMode       │           26 │
│ $.Event.EventData.BuildVersion   │           26 │
│ $.Event.EventData.MajorVersion   │           26 │
│ $.Event.EventData.MinorVersion   │           26 │
│ $.Event.EventData.QfeVersion     │           26 │
│ $.Event.EventData.ServiceVersion │           26 │
│ $.Event.EventData.StartTime      │           26 │
└──────────────────────────────────┴──────────────┘
```
BootMode
```
0: Normal boot
1: Safe Mode boot
```



## 13 (OS was shutdown)
```sql
memory D SELECT Event.System.Computer, Event.EventData.StopTime, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-Kernel-General' AND (Event.System.EventID->>'$' = '13')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────────────┬──────────────┐
│      Computer       │          StopTime          │ count_star() │
│       varchar       │         timestamp          │    int64     │
├─────────────────────┼────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-02 18:24:15.298357 │            1 │
│ rd01.shieldbase.com │ 2023-01-23 14:51:02.797474 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:18:59.196235 │            1 │
│ rd01.shieldbase.com │ 2023-01-25 14:33:41.962038 │            1 │
└─────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-Kernel-General') AND (e.json->>'$.Event.System.EventID' = '13') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────┬──────────────┐
│          fullkey           │ count_star() │
│          varchar           │    int64     │
├────────────────────────────┼──────────────┤
│ $.Event.EventData.StopTime │           24 │
└────────────────────────────┴──────────────┘
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



## 35 (Synchronizing system time from time service)
```sql
memory D SELECT Event.System.Computer, Event.EventData.TimeSource, Event.EventData.TimeSourceRefId, Event.EventData.CurrentStratumNumber, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-Time-Service' AND (Event.System.EventID->>'$' = '35')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────────────────────────────────────────────────────┬─────────────────┬──────────────────────┬──────────────┐
│      Computer       │                       TimeSource                        │ TimeSourceRefId │ CurrentStratumNumber │ count_star() │
│       varchar       │                         varchar                         │      int64      │        int64         │    int64     │
├─────────────────────┼─────────────────────────────────────────────────────────┼─────────────────┼──────────────────────┼──────────────┤
│ rd01.shieldbase.com │ dc01.shieldbase.com (ntp.d|0.0.0.0:123->172.16.4.4:123) │        67375276 │                    5 │            4 │
└─────────────────────┴─────────────────────────────────────────────────────────┴─────────────────┴──────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-Time-Service') AND (e.json->>'$.Event.System.EventID' = '37') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.#attributes      │           47 │
│ $.Event.EventData.#attributes.Name │           47 │
│ $.Event.EventData.TimeSource       │           47 │
└────────────────────────────────────┴──────────────┘
```



## 37 (Synchronizing system time from time service)
```sql
memory D SELECT Event.System.Computer, Event.EventData.TimeSource, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-Time-Service' AND (Event.System.EventID->>'$' = '37')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │                       TimeSource                        │ count_star() │
│       varchar       │                         varchar                         │    int64     │
├─────────────────────┼─────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ dc01.shieldbase.com (ntp.d|0.0.0.0:123->172.16.4.4:123) │           12 │
└─────────────────────┴─────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-Time-Service') AND (e.json->>'$.Event.System.EventID' = '37') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.#attributes      │           47 │
│ $.Event.EventData.#attributes.Name │           47 │
│ $.Event.EventData.TimeSource       │           47 │
└────────────────────────────────────┴──────────────┘
```



## 42 (Sleep/wake events)
Provider: Microsoft-Windows-Kernel-Power



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
```sql
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



## 105 (Power source change)
Provider: Microsoft-Windows-Kernel-Power



## 1074 (A user initiated a system restart)
```sql
memory D SELECT Event.System.Computer, Event.EventData.param2, Event.EventData.param7, Event.EventData.param1, Event.EventData.param5, Event.EventData.param3, Event.EventData.param4, Event.EventData.param6, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'User32' AND (Event.System.EventID->>'$."#text"' = '1074')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────┬────────────────────┬──────────────────────────────────────────────┬───────────┬───────────────────┬─────────┬─────────┬──────────────┐
│      Computer       │ param2  │       param7       │                    param1                    │  param5   │      param3       │ param4  │ param6  │ count_star() │
│       varchar       │ varchar │      varchar       │                   varchar                    │  varchar  │      varchar      │ varchar │ varchar │    int64     │
├─────────────────────┼─────────┼────────────────────┼──────────────────────────────────────────────┼───────────┼───────────────────┼─────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │ RD01    │ shieldbase\tdungan │ C:\Windows\Explorer.EXE (RD01)               │ restart   │ Other (Unplanned) │ 0x0     │         │            1 │
│ rd01.shieldbase.com │ RD01    │ shieldbase\tdungan │ C:\Windows\System32\RuntimeBroker.exe (RD01) │ power off │ Other (Unplanned) │ 0x0     │         │            1 │
│ rd01.shieldbase.com │ RD01    │ shieldbase\tdungan │ C:\Windows\System32\RuntimeBroker.exe (RD01) │ restart   │ Other (Unplanned) │ 0x0     │         │            2 │
└─────────────────────┴─────────┴────────────────────┴──────────────────────────────────────────────┴───────────┴───────────────────┴─────────┴─────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'User32') AND (e.json->>'$.Event.System.EventID."#text"' = '1074') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────┬──────────────┐
│         fullkey          │ count_star() │
│         varchar          │    int64     │
├──────────────────────────┼──────────────┤
│ $.Event.EventData.param1 │           25 │
│ $.Event.EventData.param2 │           25 │
│ $.Event.EventData.param3 │           25 │
│ $.Event.EventData.param4 │           25 │
│ $.Event.EventData.param5 │           25 │
│ $.Event.EventData.param6 │           25 │
│ $.Event.EventData.param7 │           25 │
└──────────────────────────┴──────────────┘
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



## 1130 (Group Policy Script Failure)
Provider: Microsoft-Windows-GroupPolicy



## 1500 (Group Policy Settings processed successfully for COMPUTER)
```sql
memory D SELECT Event.System.Computer, Event.EventData.SupportInfo1, Event.EventData.SupportInfo2, Event.EventData.ProcessingMode, Event.EventData.DCName, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-GroupPolicy' AND (Event.System.EventID->>'$' = '1500')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬──────────────┬──────────────┬────────────────┬───────────────────────┬──────────────┐
│      Computer       │ SupportInfo1 │ SupportInfo2 │ ProcessingMode │        DCName         │ count_star() │
│       varchar       │    int64     │    int64     │     int64      │        varchar        │    int64     │
├─────────────────────┼──────────────┼──────────────┼────────────────┼───────────────────────┼──────────────┤
│ rd01.shieldbase.com │            1 │         4214 │              0 │ \\dc01.shieldbase.com │          360 │
└─────────────────────┴──────────────┴──────────────┴────────────────┴───────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-GroupPolicy') AND (e.json->>'$.Event.System.EventID' = '1500') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────────────────┬──────────────┐
│                    fullkey                     │ count_star() │
│                    varchar                     │    int64     │
├────────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.DCName                       │         1592 │
│ $.Event.EventData.ProcessingMode               │         1592 │
│ $.Event.EventData.ProcessingTimeInMilliseconds │         1592 │
│ $.Event.EventData.SupportInfo1                 │         1592 │
│ $.Event.EventData.SupportInfo2                 │         1592 │
└────────────────────────────────────────────────┴──────────────┘
```



## 1501 (Group Policy Settings processed successfully for USER)
```sql
memory D SELECT Event.System.Computer, Event.EventData.SupportInfo1, Event.EventData.SupportInfo2, Event.EventData.ProcessingMode, Event.EventData.DCName, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-GroupPolicy' AND (Event.System.EventID->>'$' = '1501')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬──────────────┬──────────────┬────────────────┬───────────────────────┬──────────────┐
│      Computer       │ SupportInfo1 │ SupportInfo2 │ ProcessingMode │        DCName         │ count_star() │
│       varchar       │    int64     │    int64     │     int64      │        varchar        │    int64     │
├─────────────────────┼──────────────┼──────────────┼────────────────┼───────────────────────┼──────────────┤
│ rd01.shieldbase.com │            1 │         4214 │              0 │ \\dc01.shieldbase.com │          211 │
└─────────────────────┴──────────────┴──────────────┴────────────────┴───────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-GroupPolicy') AND (e.json->>'$.Event.System.EventID' = '1501') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────────────────┬──────────────┐
│                    fullkey                     │ count_star() │
│                    varchar                     │    int64     │
├────────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.DCName                       │          961 │
│ $.Event.EventData.ProcessingMode               │          961 │
│ $.Event.EventData.ProcessingTimeInMilliseconds │          961 │
│ $.Event.EventData.SupportInfo1                 │          961 │
│ $.Event.EventData.SupportInfo2                 │          961 │
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



## 7001 (User logon)
```sql
memory D SELECT Event.System.Computer, Event.EventData.UserSid, Event.EventData.TSId, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-Winlogon' AND (Event.System.EventID->>'$' = '7001')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────────────────────────────────┬───────┬──────────────┐
│      Computer       │                    UserSid                     │ TSId  │ count_star() │
│       varchar       │                    varchar                     │ int64 │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     2 │            4 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     3 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     4 │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     5 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     6 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     7 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     8 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     9 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │    10 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │    11 │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │    13 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │    14 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │     3 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │    12 │            1 │
└─────────────────────┴────────────────────────────────────────────────┴───────┴──────────────┘
  14 rows                                                                           4 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-Winlogon') AND (e.json->>'$.Event.System.EventID' = '7001') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────┬──────────────┐
│          fullkey          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ $.Event.EventData.TSId    │           46 │
│ $.Event.EventData.UserSid │           46 │
└───────────────────────────┴──────────────┘
```



## 7002 (User logoff)
```sql
memory D SELECT Event.System.Computer, Event.EventData.UserSid, Event.EventData.TSId, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Microsoft-Windows-Winlogon' AND (Event.System.EventID->>'$' = '7002')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────────────────────────────────┬───────┬──────────────┐
│      Computer       │                    UserSid                     │ TSId  │ count_star() │
│       varchar       │                    varchar                     │ int64 │    int64     │
├─────────────────────┼────────────────────────────────────────────────┼───────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     2 │            3 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     3 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     4 │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     5 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     6 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     7 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     8 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │     9 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │    10 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │    11 │            2 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │    13 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1136 │    14 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │     3 │            1 │
│ rd01.shieldbase.com │ S-1-5-21-2838623409-1327563992-2591358621-1220 │    12 │            1 │
└─────────────────────┴────────────────────────────────────────────────┴───────┴──────────────┘
  14 rows                                                                           4 columnsf
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-Winlogon') AND (e.json->>'$.Event.System.EventID' = '7002') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────┬──────────────┐
│          fullkey          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ $.Event.EventData.TSId    │           45 │
│ $.Event.EventData.UserSid │           45 │
└───────────────────────────┴──────────────┘
```



## 7031 (Service crashed unexpectedly)
```sql
memory D SELECT Event.System.Computer, Event.EventData.param1, Event.EventData.param2, Event.EventData.param3, Event.EventData.param4, Event.EventData.param5, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Service Control Manager' AND (Event.System.EventID->>'$."#text"' = '7031')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬───────────────────────────────────┬─────────┬─────────┬─────────┬─────────────────────┬──────────────┐
│      Computer       │              param1               │ param2  │ param3  │ param4  │       param5        │ count_star() │
│       varchar       │              varchar              │ varchar │ varchar │ varchar │       varchar       │    int64     │
├─────────────────────┼───────────────────────────────────┼─────────┼─────────┼─────────┼─────────────────────┼──────────────┤
│ rd01.shieldbase.com │ System Event Notification Service │ 1       │ 120000  │ 1       │ Restart the service │            1 │
└─────────────────────┴───────────────────────────────────┴─────────┴─────────┴─────────┴─────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Service Control Manager') AND (e.json->>'$.Event.System.EventID."#text"' = '7031') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────┬──────────────┐
│         fullkey          │ count_star() │
│         varchar          │    int64     │
├──────────────────────────┼──────────────┤
│ $.Event.EventData.Binary │            4 │
│ $.Event.EventData.param1 │            4 │
│ $.Event.EventData.param2 │            4 │
│ $.Event.EventData.param3 │            4 │
│ $.Event.EventData.param4 │            4 │
│ $.Event.EventData.param5 │            4 │
└──────────────────────────┴──────────────┘
```



## 7034 (Service crashed unexpectedly)
```sql
memory D SELECT Event.System.Computer, Event.EventData.param1, Event.EventData.param2, COUNT(*)
         FROM read_json('.\system-rd05.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Service Control Manager' AND (Event.System.EventID->>'$."#text"' = '7034')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬──────────────────────────────────────────┬─────────┬──────────────┐
│      Computer       │                  param1                  │ param2  │ count_star() │
│       varchar       │                 varchar                  │ varchar │    int64     │
├─────────────────────┼──────────────────────────────────────────┼─────────┼──────────────┤
│ rd05.shieldbase.com │ Connected User Experiences and Telemetry │ 3       │            1 │
└─────────────────────┴──────────────────────────────────────────┴─────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd05.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Service Control Manager') AND (e.json->>'$.Event.System.EventID."#text"' = '7034') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────┬──────────────┐
│         fullkey          │ count_star() │
│         varchar          │    int64     │
├──────────────────────────┼──────────────┤
│ $.Event.EventData.Binary │            1 │
│ $.Event.EventData.param1 │            1 │
│ $.Event.EventData.param2 │            1 │
└──────────────────────────┴──────────────┘
```



## 7035 (Service sent a Start/Stop control)
Provider: Service Control Manager



## 7036 (Service started or stopped)
```sql
memory D SELECT Event.System.Computer, Event.EventData.param1, Event.EventData.param2, replace(CAST(from_hex(Event.EventData.Binary) AS VARCHAR), '\x00', ''), COUNT(*)
         FROM read_json('.\system-dc01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Service Control Manager' AND (Event.System.EventID->>'$."#text"' = '7036')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬──────────────────────────────────────────────────┬─────────┬──────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │                      param1                      │ param2  │ "replace"(CAST(from_hex("Event".EventData."Binary") AS VARCHAR), '\x00', '') │ count_star() │
│       varchar       │                     varchar                      │ varchar │                                   varchar                                    │    int64     │
├─────────────────────┼──────────────────────────────────────────────────┼─────────┼──────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ dc01.shieldbase.com │ ADWS                                             │ running │ ADWS/4                                                                       │            4 │
│ dc01.shieldbase.com │ AWS Lite Guest Agent                             │ stopped │ AWSLiteAgent/1                                                               │            4 │
│ dc01.shieldbase.com │ AWSLiteAgent                                     │ running │ AWSLiteAgent/4                                                               │            4 │
│ dc01.shieldbase.com │ Active Directory Domain Services                 │ stopped │ NTDS/1                                                                       │            3 │
│ dc01.shieldbase.com │ Active Directory Web Services                    │ stopped │ ADWS/1                                                                       │            4 │
│ dc01.shieldbase.com │ Amazon EC2Launch                                 │ running │ Amazon EC2Launch/4                                                           │            4 │
│ dc01.shieldbase.com │ Amazon EC2Launch                                 │ stopped │ Amazon EC2Launch/1                                                           │            4 │
│ dc01.shieldbase.com │ AmazonSSMAgent                                   │ running │ AmazonSSMAgent/4                                                             │            4 │
│ dc01.shieldbase.com │ AppX Deployment Service (AppXSVC)                │ running │ AppXSvc/4                                                                    │          187 │
│ dc01.shieldbase.com │ AppX Deployment Service (AppXSVC)                │ stopped │ AppXSvc/1                                                                    │          187 │
│ dc01.shieldbase.com │ Application Information                          │ running │ Appinfo/4                                                                    │            4 │
│ dc01.shieldbase.com │ BFE                                              │ running │ BFE/4                                                                        │            4 │
│ dc01.shieldbase.com │ Background Intelligent Transfer Service          │ running │ BITS/4                                                                       │           27 │
│ dc01.shieldbase.com │ Background Intelligent Transfer Service          │ stopped │ BITS/1                                                                       │           27 │
│ dc01.shieldbase.com │ BrokerInfrastructure                             │ running │ BrokerInfrastructure/4                                                       │            4 │
│ dc01.shieldbase.com │ CNG Key Isolation                                │ running │ KeyIso/4                                                                     │            4 │
│ dc01.shieldbase.com │ Capability Access Manager Service                │ running │ camsvc/4                                                                     │            4 │
│ dc01.shieldbase.com │ Capability Access Manager Service                │ stopped │ camsvc/1                                                                     │            4 │
│ dc01.shieldbase.com │ CertPropSvc                                      │ running │ CertPropSvc/4                                                                │            4 │
│ dc01.shieldbase.com │ Certificate Propagation                          │ stopped │ CertPropSvc/1                                                                │            4 │
│ dc01.shieldbase.com │ Client License Service (ClipSVC)                 │ running │ ClipSVC/4                                                                    │           30 │
│ dc01.shieldbase.com │ Client License Service (ClipSVC)                 │ stopped │ ClipSVC/1                                                                    │           30 │
│ dc01.shieldbase.com │ Clipboard User Service_1d51b9                    │ running │ cbdhsvc_1d51b9/4                                                             │            1 │
│ dc01.shieldbase.com │ Clipboard User Service_1d51b9                    │ stopped │ cbdhsvc_1d51b9/1                                                             │            1 │
│ dc01.shieldbase.com │ Clipboard User Service_25a11f                    │ running │ cbdhsvc_25a11f/4                                                             │            1 │
│ dc01.shieldbase.com │ Clipboard User Service_25a11f                    │ stopped │ cbdhsvc_25a11f/1                                                             │            1 │
│ dc01.shieldbase.com │ Clipboard User Service_3da776c                   │ running │ cbdhsvc_3da776c/4                                                            │            1 │
│ dc01.shieldbase.com │ Clipboard User Service_3da776c                   │ stopped │ cbdhsvc_3da776c/1                                                            │            1 │
│ dc01.shieldbase.com │ Clipboard User Service_43970d4                   │ running │ cbdhsvc_43970d4/4                                                            │            1 │
│ dc01.shieldbase.com │ Clipboard User Service_43970d4                   │ stopped │ cbdhsvc_43970d4/1                                                            │            1 │
│ dc01.shieldbase.com │ Clipboard User Service_8623a                     │ running │ cbdhsvc_8623a/4                                                              │            1 │
│ dc01.shieldbase.com │ Clipboard User Service_8623a                     │ stopped │ cbdhsvc_8623a/1                                                              │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform Service               │ running │ CDPSvc/4                                                                     │            4 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_1d51b9   │ running │ CDPUserSvc_1d51b9/4                                                          │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_1d51b9   │ stopped │ CDPUserSvc_1d51b9/1                                                          │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_25a11f   │ running │ CDPUserSvc_25a11f/4                                                          │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_25a11f   │ stopped │ CDPUserSvc_25a11f/1                                                          │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_3da776c  │ running │ CDPUserSvc_3da776c/4                                                         │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_3da776c  │ stopped │ CDPUserSvc_3da776c/1                                                         │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_43970d4  │ running │ CDPUserSvc_43970d4/4                                                         │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_43970d4  │ stopped │ CDPUserSvc_43970d4/1                                                         │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_8623a    │ running │ CDPUserSvc_8623a/4                                                           │            1 │
│ dc01.shieldbase.com │ Connected Devices Platform User Service_8623a    │ stopped │ CDPUserSvc_8623a/1                                                           │            1 │
│ dc01.shieldbase.com │ CoreMessagingRegistrar                           │ running │ CoreMessagingRegistrar/4                                                     │            4 │
│ dc01.shieldbase.com │ CryptSvc                                         │ running │ CryptSvc/4                                                                   │            4 │
│ dc01.shieldbase.com │ Cryptographic Services                           │ stopped │ CryptSvc/1                                                                   │            3 │
│ dc01.shieldbase.com │ DFS Replication                                  │ stopped │ DFSR/1                                                                       │            3 │
│ dc01.shieldbase.com │ DFSR                                             │ running │ DFSR/4                                                                       │            4 │
│ dc01.shieldbase.com │ DHCP Client                                      │ stopped │ Dhcp/1                                                                       │            4 │
│ dc01.shieldbase.com │ DNS                                              │ running │ DNS/4                                                                        │            1 │
│ dc01.shieldbase.com │ DNS Server                                       │ running │ DNS/4                                                                        │            3 │
│ dc01.shieldbase.com │ DNS Server                                       │ stopped │ DNS/1                                                                        │            4 │
│ dc01.shieldbase.com │ Data Sharing Service                             │ running │ DsSvc/4                                                                      │            2 │
│ dc01.shieldbase.com │ Data Sharing Service                             │ stopped │ DsSvc/1                                                                      │            3 │
│ dc01.shieldbase.com │ DcomLaunch                                       │ running │ DcomLaunch/4                                                                 │            4 │
│ dc01.shieldbase.com │ Delivery Optimization                            │ running │ DoSvc/4                                                                      │          130 │
│ dc01.shieldbase.com │ Delivery Optimization                            │ stopped │ DoSvc/1                                                                      │          130 │
│ dc01.shieldbase.com │ Device Setup Manager                             │ running │ DsmSvc/4                                                                     │            4 │
│ dc01.shieldbase.com │ Device Setup Manager                             │ stopped │ DsmSvc/1                                                                     │            8 │
│ dc01.shieldbase.com │ Dfs                                              │ running │ Dfs/4                                                                        │            4 │
│ dc01.shieldbase.com │ Dhcp                                             │ running │ Dhcp/4                                                                       │            4 │
│ dc01.shieldbase.com │ Diagnostic Policy Service                        │ running │ DPS/4                                                                        │            4 │
│ dc01.shieldbase.com │ Diagnostic Policy Service                        │ stopped │ DPS/1                                                                        │            3 │
│ dc01.shieldbase.com │ Diagnostic System Host                           │ running │ WdiSystemHost/4                                                              │            5 │
│ dc01.shieldbase.com │ Diagnostic System Host                           │ stopped │ WdiSystemHost/1                                                              │            5 │
│ dc01.shieldbase.com │ Display Policy Service                           │ running │ DispBrokerDesktopSvc/4                                                       │            4 │
│ dc01.shieldbase.com │ Distributed Transaction Coordinator              │ running │ MSDTC/4                                                                      │            4 │
│ dc01.shieldbase.com │ Distributed Transaction Coordinator              │ stopped │ MSDTC/1                                                                      │            4 │
│ dc01.shieldbase.com │ Dnscache                                         │ running │ Dnscache/4                                                                   │            4 │
│ dc01.shieldbase.com │ DsmSvc                                           │ running │ DsmSvc/4                                                                     │            3 │
│ dc01.shieldbase.com │ EFS                                              │ running │ EFS/4                                                                        │            4 │
│ dc01.shieldbase.com │ EventLog                                         │ running │ EventLog/4                                                                   │            4 │
│ dc01.shieldbase.com │ EventSystem                                      │ running │ EventSystem/4                                                                │            4 │
│ dc01.shieldbase.com │ F-Response Subject Service                       │ running │ F-Response Subject Service/4                                                 │            2 │
│ dc01.shieldbase.com │ F-Response Subject Service                       │ stopped │ F-Response Subject Service/1                                                 │            2 │
│ dc01.shieldbase.com │ FontCache                                        │ running │ FontCache/4                                                                  │            4 │
│ dc01.shieldbase.com │ Function Discovery Provider Host                 │ running │ fdPHost/4                                                                    │            1 │
│ dc01.shieldbase.com │ Function Discovery Provider Host                 │ stopped │ fdPHost/1                                                                    │            1 │
│ dc01.shieldbase.com │ Group Policy Client                              │ stopped │ gpsvc/1                                                                      │            4 │
│ dc01.shieldbase.com │ IKE and AuthIP IPsec Keying Modules              │ stopped │ IKEEXT/1                                                                     │            4 │
│ dc01.shieldbase.com │ IKEEXT                                           │ running │ IKEEXT/4                                                                     │            4 │
│ dc01.shieldbase.com │ IPsec Policy Agent                               │ stopped │ PolicyAgent/1                                                                │            4 │
│ dc01.shieldbase.com │ Intersite Messaging                              │ stopped │ IsmServ/1                                                                    │            4 │
│ dc01.shieldbase.com │ IsmServ                                          │ running │ IsmServ/4                                                                    │            4 │
│ dc01.shieldbase.com │ Kdc                                              │ running │ Kdc/4                                                                        │            4 │
│ dc01.shieldbase.com │ LSM                                              │ running │ LSM/4                                                                        │            4 │
│ dc01.shieldbase.com │ LanmanServer                                     │ running │ LanmanServer/4                                                               │            4 │
│ dc01.shieldbase.com │ LanmanWorkstation                                │ running │ LanmanWorkstation/4                                                          │            4 │
│ dc01.shieldbase.com │ Microsoft Account Sign-in Assistant              │ running │ wlidsvc/4                                                                    │           84 │
│ dc01.shieldbase.com │ Microsoft Account Sign-in Assistant              │ stopped │ wlidsvc/1                                                                    │           82 │
│ dc01.shieldbase.com │ Microsoft Defender Antivirus Service             │ stopped │ WinDefend/1                                                                  │            3 │
│ dc01.shieldbase.com │ Microsoft Edge Update Service (edgeupdate)       │ running │ edgeupdate/4                                                                 │           92 │
│ dc01.shieldbase.com │ Microsoft Edge Update Service (edgeupdate)       │ stopped │ edgeupdate/1                                                                 │           92 │
│ dc01.shieldbase.com │ Microsoft Software Shadow Copy Provider          │ running │ swprv/4                                                                      │           45 │
│ dc01.shieldbase.com │ Microsoft Software Shadow Copy Provider          │ stopped │ swprv/1                                                                      │           45 │
│ dc01.shieldbase.com │ NTDS                                             │ running │ NTDS/4                                                                       │            4 │
│ dc01.shieldbase.com │ NcaSvc                                           │ stopped │ NcaSvc/1                                                                     │            4 │
│ dc01.shieldbase.com │ NcbService                                       │ running │ NcbService/4                                                                 │            4 │
│ dc01.shieldbase.com │ Netlogon                                         │ running │ Netlogon/4                                                                   │            4 │
│ dc01.shieldbase.com │ Network Connectivity Assistant                   │ stopped │ NcaSvc/1                                                                     │            7 │
│ dc01.shieldbase.com │ Network Setup Service                            │ running │ NetSetupSvc/4                                                                │         3233 │
│ dc01.shieldbase.com │ Network Setup Service                            │ stopped │ NetSetupSvc/1                                                                │         3233 │
│ dc01.shieldbase.com │ NlaSvc                                           │ running │ NlaSvc/4                                                                     │            4 │
│ dc01.shieldbase.com │ Optimize drives                                  │ running │ defragsvc/4                                                                  │            4 │
│ dc01.shieldbase.com │ Optimize drives                                  │ stopped │ defragsvc/1                                                                  │            4 │
│ dc01.shieldbase.com │ Plug and Play                                    │ stopped │ PlugPlay/1                                                                   │            4 │
│ dc01.shieldbase.com │ PlugPlay                                         │ running │ PlugPlay/4                                                                   │            4 │
│ dc01.shieldbase.com │ PolicyAgent                                      │ running │ PolicyAgent/4                                                                │            4 │
│ dc01.shieldbase.com │ Portable Device Enumerator Service               │ running │ WPDBusEnum/4                                                                 │            8 │
│ dc01.shieldbase.com │ Portable Device Enumerator Service               │ stopped │ WPDBusEnum/1                                                                 │           12 │
│ dc01.shieldbase.com │ Power                                            │ running │ Power/4                                                                      │            4 │
│ dc01.shieldbase.com │ ProfSvc                                          │ running │ ProfSvc/4                                                                    │            4 │
│ dc01.shieldbase.com │ Program Compatibility Assistant Service          │ running │ PcaSvc/4                                                                     │            4 │
│ dc01.shieldbase.com │ Program Compatibility Assistant Service          │ stopped │ PcaSvc/1                                                                     │            4 │
│ dc01.shieldbase.com │ RasMan                                           │ running │ RasMan/4                                                                     │            1 │
│ dc01.shieldbase.com │ Remote Access Connection Manager                 │ running │ RasMan/4                                                                     │            1 │
│ dc01.shieldbase.com │ Remote Desktop Services UserMode Port Redirector │ stopped │ UmRdpService/1                                                               │            3 │
│ dc01.shieldbase.com │ Remote Registry                                  │ running │ RemoteRegistry/4                                                             │            2 │
│ dc01.shieldbase.com │ Remote Registry                                  │ stopped │ RemoteRegistry/1                                                             │            6 │
│ dc01.shieldbase.com │ RemoteRegistry                                   │ running │ RemoteRegistry/4                                                             │            4 │
│ dc01.shieldbase.com │ RpcEptMapper                                     │ running │ RpcEptMapper/4                                                               │            4 │
│ dc01.shieldbase.com │ RpcSs                                            │ running │ RpcSs/4                                                                      │            4 │
│ dc01.shieldbase.com │ SENS                                             │ running │ SENS/4                                                                       │            4 │
│ dc01.shieldbase.com │ SamSs                                            │ running │ SamSs/4                                                                      │            4 │
│ dc01.shieldbase.com │ Schedule                                         │ running │ Schedule/4                                                                   │            4 │
│ dc01.shieldbase.com │ Secure Socket Tunneling Protocol Service         │ running │ SstpSvc/4                                                                    │            1 │
│ dc01.shieldbase.com │ Secure Socket Tunneling Protocol Service         │ stopped │ SstpSvc/1                                                                    │            1 │
│ dc01.shieldbase.com │ SessionEnv                                       │ running │ SessionEnv/4                                                                 │            4 │
│ dc01.shieldbase.com │ ShellHWDetection                                 │ running │ ShellHWDetection/4                                                           │            4 │
│ dc01.shieldbase.com │ Software Protection                              │ running │ sppsvc/4                                                                     │            9 │
│ dc01.shieldbase.com │ Software Protection                              │ stopped │ sppsvc/1                                                                     │            8 │
│ dc01.shieldbase.com │ Spooler                                          │ running │ Spooler/4                                                                    │            4 │
│ dc01.shieldbase.com │ SstpSvc                                          │ running │ SstpSvc/4                                                                    │            1 │
│ dc01.shieldbase.com │ State Repository Service                         │ running │ StateRepository/4                                                            │            7 │
│ dc01.shieldbase.com │ State Repository Service                         │ stopped │ StateRepository/1                                                            │           11 │
│ dc01.shieldbase.com │ StateRepository                                  │ running │ StateRepository/4                                                            │            4 │
│ dc01.shieldbase.com │ Storage Service                                  │ running │ StorSvc/4                                                                    │            4 │
│ dc01.shieldbase.com │ SysMain                                          │ running │ SysMain/4                                                                    │            4 │
│ dc01.shieldbase.com │ SysMain                                          │ stopped │ SysMain/1                                                                    │            4 │
│ dc01.shieldbase.com │ SystemEventsBroker                               │ running │ SystemEventsBroker/4                                                         │            4 │
│ dc01.shieldbase.com │ Task Scheduler                                   │ stopped │ Schedule/1                                                                   │            3 │
│ dc01.shieldbase.com │ TermService                                      │ running │ TermService/4                                                                │            4 │
│ dc01.shieldbase.com │ Themes                                           │ running │ Themes/4                                                                     │            4 │
│ dc01.shieldbase.com │ TimeBrokerSvc                                    │ running │ TimeBrokerSvc/4                                                              │            4 │
│ dc01.shieldbase.com │ Touch Keyboard and Handwriting Panel Service     │ running │ TabletInputService/4                                                         │            3 │
│ dc01.shieldbase.com │ UmRdpService                                     │ running │ UmRdpService/4                                                               │            4 │
│ dc01.shieldbase.com │ Update Orchestrator Service                      │ running │ UsoSvc/4                                                                     │            4 │
│ dc01.shieldbase.com │ Update Orchestrator Service                      │ stopped │ UsoSvc/1                                                                     │            3 │
│ dc01.shieldbase.com │ User Access Logging Service                      │ running │ UALSVC/4                                                                     │            4 │
│ dc01.shieldbase.com │ User Access Logging Service                      │ stopped │ UALSVC/1                                                                     │            4 │
│ dc01.shieldbase.com │ User Profile Service                             │ stopped │ ProfSvc/1                                                                    │            4 │
│ dc01.shieldbase.com │ UserManager                                      │ running │ UserManager/4                                                                │            4 │
│ dc01.shieldbase.com │ Velociraptor Service                             │ running │ Velociraptor/4                                                               │            3 │
│ dc01.shieldbase.com │ Velociraptor Service                             │ stopped │ Velociraptor/1                                                               │            2 │
│ dc01.shieldbase.com │ Virtual Disk                                     │ stopped │ vds/1                                                                        │            3 │
│ dc01.shieldbase.com │ Volume Shadow Copy                               │ running │ VSS/4                                                                        │           46 │
│ dc01.shieldbase.com │ Volume Shadow Copy                               │ stopped │ VSS/1                                                                        │           46 │
│ dc01.shieldbase.com │ W32Time                                          │ running │ W32Time/4                                                                    │            1 │
│ dc01.shieldbase.com │ WPDBusEnum                                       │ running │ WPDBusEnum/4                                                                 │            4 │
│ dc01.shieldbase.com │ Wcmsvc                                           │ running │ Wcmsvc/4                                                                     │            4 │
│ dc01.shieldbase.com │ Web Account Manager                              │ running │ TokenBroker/4                                                                │            3 │
│ dc01.shieldbase.com │ WinDefend                                        │ running │ WinDefend/4                                                                  │            4 │
│ dc01.shieldbase.com │ WinHTTP Web Proxy Auto-Discovery Service         │ stopped │ WinHttpAutoProxySvc/1                                                        │            4 │
│ dc01.shieldbase.com │ WinHttpAutoProxySvc                              │ running │ WinHttpAutoProxySvc/4                                                        │            4 │
│ dc01.shieldbase.com │ WinRM                                            │ running │ WinRM/4                                                                      │            4 │
│ dc01.shieldbase.com │ Windows Connection Manager                       │ stopped │ Wcmsvc/1                                                                     │            4 │
│ dc01.shieldbase.com │ Windows Event Log                                │ stopped │ EventLog/1                                                                   │            3 │
│ dc01.shieldbase.com │ Windows Font Cache Service                       │ stopped │ FontCache/1                                                                  │            4 │
│ dc01.shieldbase.com │ Windows Installer                                │ running │ msiserver/4                                                                  │            5 │
│ dc01.shieldbase.com │ Windows Installer                                │ stopped │ msiserver/1                                                                  │            5 │
│ dc01.shieldbase.com │ Windows License Manager Service                  │ running │ LicenseManager/4                                                             │            2 │
│ dc01.shieldbase.com │ Windows Management Instrumentation               │ stopped │ Winmgmt/1                                                                    │            4 │
│ dc01.shieldbase.com │ Windows Modules Installer                        │ running │ TrustedInstaller/4                                                           │          149 │
│ dc01.shieldbase.com │ Windows Modules Installer                        │ stopped │ TrustedInstaller/1                                                           │          149 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_1d51b9   │ running │ WpnUserService_1d51b9/4                                                      │            1 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_1d51b9   │ stopped │ WpnUserService_1d51b9/1                                                      │            1 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_25a11f   │ running │ WpnUserService_25a11f/4                                                      │            1 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_25a11f   │ stopped │ WpnUserService_25a11f/1                                                      │            1 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_3da776c  │ running │ WpnUserService_3da776c/4                                                     │            1 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_3da776c  │ stopped │ WpnUserService_3da776c/1                                                     │            1 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_43970d4  │ running │ WpnUserService_43970d4/4                                                     │            1 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_43970d4  │ stopped │ WpnUserService_43970d4/1                                                     │            1 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_8623a    │ running │ WpnUserService_8623a/4                                                       │            1 │
│ dc01.shieldbase.com │ Windows Push Notifications User Service_8623a    │ stopped │ WpnUserService_8623a/1                                                       │            1 │
│ dc01.shieldbase.com │ Windows Remote Management (WS-Management)        │ stopped │ WinRM/1                                                                      │            3 │
│ dc01.shieldbase.com │ Windows Security Service                         │ running │ SecurityHealthService/4                                                      │            1 │
│ dc01.shieldbase.com │ Windows Security Service                         │ stopped │ SecurityHealthService/1                                                      │            1 │
│ dc01.shieldbase.com │ Windows Time                                     │ running │ W32Time/4                                                                    │            3 │
│ dc01.shieldbase.com │ Windows Time                                     │ stopped │ W32Time/1                                                                    │            4 │
│ dc01.shieldbase.com │ Windows Update                                   │ running │ wuauserv/4                                                                   │           90 │
│ dc01.shieldbase.com │ Windows Update                                   │ stopped │ wuauserv/1                                                                   │           90 │
│ dc01.shieldbase.com │ Windows Update Medic Service                     │ running │ WaaSMedicSvc/4                                                               │          163 │
│ dc01.shieldbase.com │ Windows Update Medic Service                     │ stopped │ WaaSMedicSvc/1                                                               │          163 │
│ dc01.shieldbase.com │ Winmgmt                                          │ running │ Winmgmt/4                                                                    │            4 │
│ dc01.shieldbase.com │ WpnService                                       │ running │ WpnService/4                                                                 │            4 │
│ dc01.shieldbase.com │ gpsvc                                            │ running │ gpsvc/4                                                                      │            4 │
│ dc01.shieldbase.com │ iphlpsvc                                         │ running │ iphlpsvc/4                                                                   │            4 │
│ dc01.shieldbase.com │ lmhosts                                          │ running │ lmhosts/4                                                                    │            4 │
│ dc01.shieldbase.com │ mpssvc                                           │ running │ mpssvc/4                                                                     │            4 │
│ dc01.shieldbase.com │ netprofm                                         │ running │ netprofm/4                                                                   │            4 │
│ dc01.shieldbase.com │ nsi                                              │ running │ nsi/4                                                                        │            4 │
│ dc01.shieldbase.com │ vds                                              │ running │ vds/4                                                                        │            4 │
│ dc01.shieldbase.com │ windows_exporter                                 │ running │ windows_exporter/4                                                           │            4 │
│ dc01.shieldbase.com │ windows_exporter                                 │ stopped │ windows_exporter/1                                                           │            3 │
│ dc01.shieldbase.com │ wmiApSrv                                         │ running │ wmiApSrv/4                                                                   │            4 │
└─────────────────────┴──────────────────────────────────────────────────┴─────────┴──────────────────────────────────────────────────────────────────────────────┴──────────────┘
  205 rows                                                                                                                                                             5 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-dc01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Service Control Manager') AND (e.json->>'$.Event.System.EventID."#text"' = '7036') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────┬──────────────┐
│         fullkey          │ count_star() │
│         varchar          │    int64     │
├──────────────────────────┼──────────────┤
│ $.Event.EventData.Binary │        43836 │
│ $.Event.EventData.param1 │        43836 │
│ $.Event.EventData.param2 │        43836 │
└──────────────────────────┴──────────────┘
```



## 7040 (Start type of a service has changed)
```sql
memory D SELECT Event.System.Computer, Event.EventData.param4, Event.EventData.param1, Event.EventData.param2, Event.EventData.param3, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Service Control Manager' AND (Event.System.EventID->>'$."#text"' = '7040')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────┬──────────────────────────────────────────────┬──────────────┬──────────────┬──────────────┐
│      Computer       │       param4       │                    param1                    │    param2    │    param3    │ count_star() │
│       varchar       │      varchar       │                   varchar                    │   varchar    │   varchar    │    int64     │
├─────────────────────┼────────────────────┼──────────────────────────────────────────────┼──────────────┼──────────────┼──────────────┤
│ rd01.shieldbase.com │ BITS               │ Background Intelligent Transfer Service      │ auto start   │ demand start │            1 │
│ rd01.shieldbase.com │ BITS               │ Background Intelligent Transfer Service      │ demand start │ auto start   │            1 │
│ rd01.shieldbase.com │ TabletInputService │ Touch Keyboard and Handwriting Panel Service │ auto start   │ demand start │            3 │
│ rd01.shieldbase.com │ TabletInputService │ Touch Keyboard and Handwriting Panel Service │ demand start │ auto start   │            3 │
└─────────────────────┴────────────────────┴──────────────────────────────────────────────┴──────────────┴──────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Service Control Manager') AND (e.json->>'$.Event.System.EventID."#text"' = '7040') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────┬──────────────┐
│         fullkey          │ count_star() │
│         varchar          │    int64     │
├──────────────────────────┼──────────────┤
│ $.Event.EventData.param1 │         2130 │
│ $.Event.EventData.param2 │         2130 │
│ $.Event.EventData.param3 │         2130 │
│ $.Event.EventData.param4 │         2130 │
└──────────────────────────┴──────────────┘
```



## 7045 (A new service was installed in the system)
```sql
memory D SELECT Event.System.Computer, Event.EventData.ServiceName, Event.EventData.ImagePath, Event.EventData.ServiceType, Event.EventData.StartType, Event.EventData.AccountName, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.Provider."#attributes".Name = 'Service Control Manager' AND (Event.System.EventID->>'$."#text"' = '7045')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬────────────────────┬──────────────┬─────────────┬──────────────┐
│      Computer       │        ServiceName         │                                                          ImagePath                                                          │    ServiceType     │  StartType   │ AccountName │ count_star() │
│       varchar       │          varchar           │                                                           varchar                                                           │      varchar       │   varchar    │   varchar   │    int64     │
├─────────────────────┼────────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼────────────────────┼──────────────┼─────────────┼──────────────┤
│ rd01.shieldbase.com │ Ec2Config                  │ "C:\Program Files\Amazon\Ec2ConfigService\Ec2Config.exe"                                                                    │ user mode service  │ auto start   │ LocalSystem │            1 │
│ rd01.shieldbase.com │ F-Response Subject Service │ "C:\windows\subject_srv.exe" -s "172.16.5.25:5682" -l 3262 -v "F-Response Subject Service" -k "155522845"                   │ user mode service  │ auto start   │ LocalSystem │            1 │
│ rd01.shieldbase.com │ MpKsl3f390b95              │ C:\Windows\system32\MpEngineStore\MpKslDrv.sys                                                                              │ kernel mode driver │ demand start │             │            2 │
│ rd01.shieldbase.com │ MpKsl80b1fd2a              │ C:\Windows\system32\MpEngineStore\MpKslDrv.sys                                                                              │ kernel mode driver │ demand start │             │            3 │
│ rd01.shieldbase.com │ MpKsld6867d33              │ C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{A0638AC4-7F42-4619-900A-6971E7AD116E}\MpKslDrv.sys            │ kernel mode driver │ demand start │             │            1 │
│ rd01.shieldbase.com │ MpKsle2439143              │ C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{1FEC7FC9-D47D-4480-85E5-AE4DD6CCA988}\MpKslDrv.sys            │ kernel mode driver │ demand start │             │            1 │
│ rd01.shieldbase.com │ Velociraptor Service       │ "C:\Program Files\Velociraptor\Velociraptor.exe"  --config "C:\Program Files\Velociraptor\/client.config.yaml" service run  │ user mode service  │ auto start   │ LocalSystem │            1 │
│ rd01.shieldbase.com │ mnemosyne                  │ C:\windows/Mnemosyne.sys                                                                                                    │ kernel mode driver │ demand start │             │            2 │
└─────────────────────┴────────────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴────────────────────┴──────────────┴─────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Service Control Manager') AND (e.json->>'$.Event.System.EventID."#text"' = '7045') AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────┬──────────────┐
│            fullkey            │ count_star() │
│            varchar            │    int64     │
├───────────────────────────────┼──────────────┤
│ $.Event.EventData.AccountName │          141 │
│ $.Event.EventData.ImagePath   │          141 │
│ $.Event.EventData.ServiceName │          141 │
│ $.Event.EventData.ServiceType │          141 │
│ $.Event.EventData.StartType   │          141 │
└───────────────────────────────┴──────────────┘
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



## 20001 (Device installation)
```sql
memory D SELECT Event.System.Computer, Event.UserData.InstallDeviceID.DriverProvider, Event.UserData.InstallDeviceID.DriverName, Event.UserData.InstallDeviceID.DriverVersion, Event.UserData.InstallDeviceID.DriverDescription, Event.UserData.InstallDeviceID.DeviceInstanceID, Event.UserData.InstallDeviceID.SetupClass, Event.UserData.InstallDeviceID.InstallStatus, Event.UserData.InstallDeviceID.IsDriverOEM, Event.UserData.InstallDeviceID.RebootOption, Event.UserData.InstallDeviceID.UpgradeDevice, COUNT(*)
         FROM read_json('.\system-sql01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.Provider."#attributes".Name = 'Microsoft-Windows-UserPnp' AND (Event.System.EventID->>'$' = '20001')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────┬────────────────┬─────────────────────────────────────┬───────────────┬─────────────────────────────────┬──────────────────────────────────────────────────────────────┬──────────────────────────────────────┬───────────────┬─────────────┬──────────────┬───────────────┬──────────────┐
│    Computer     │ DriverProvider │             DriverName              │ DriverVersion │        DriverDescription        │                       DeviceInstanceID                       │              SetupClass              │ InstallStatus │ IsDriverOEM │ RebootOption │ UpgradeDevice │ count_star() │
│     varchar     │    varchar     │               varchar               │    varchar    │             varchar             │                           varchar                            │                 uuid                 │    varchar    │   boolean   │   boolean    │    boolean    │    int64     │
├─────────────────┼────────────────┼─────────────────────────────────────┼───────────────┼─────────────────────────────────┼──────────────────────────────────────────────────────────────┼──────────────────────────────────────┼───────────────┼─────────────┼──────────────┼───────────────┼──────────────┤
│ EC2AMAZ-VPL09BT │ Intel          │ vxn68x64.inf_amd64_310813dc59a156c7 │ 2.1.185.0     │ Intel(R) 82599 Virtual Function │ PCI\VEN_8086&DEV_10ED&SUBSYS_00000000&REV_01\3&267A616A&1&18 │ 4d36e972-e325-11ce-bfc1-08002be10318 │ 0x0           │ true        │ false        │ false         │            1 │
└─────────────────┴────────────────┴─────────────────────────────────────┴───────────────┴─────────────────────────────────┴──────────────────────────────────────────────────────────────┴──────────────────────────────────────┴───────────────┴─────────────┴──────────────┴───────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-sql01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-UserPnp') AND (e.json->>'$.Event.System.EventID' = '20001') AND je.fullkey LIKE '$.Event.UserData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────────────────────┬──────────────┐
│                      fullkey                       │ count_star() │
│                      varchar                       │    int64     │
├────────────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.InstallDeviceID                   │            1 │
│ $.Event.UserData.InstallDeviceID.#attributes       │            1 │
│ $.Event.UserData.InstallDeviceID.#attributes.xmlns │            1 │
│ $.Event.UserData.InstallDeviceID.DeviceInstanceID  │            1 │
│ $.Event.UserData.InstallDeviceID.DriverDescription │            1 │
│ $.Event.UserData.InstallDeviceID.DriverName        │            1 │
│ $.Event.UserData.InstallDeviceID.DriverProvider    │            1 │
│ $.Event.UserData.InstallDeviceID.DriverVersion     │            1 │
│ $.Event.UserData.InstallDeviceID.InstallStatus     │            1 │
│ $.Event.UserData.InstallDeviceID.IsDriverOEM       │            1 │
│ $.Event.UserData.InstallDeviceID.RebootOption      │            1 │
│ $.Event.UserData.InstallDeviceID.SetupClass        │            1 │
│ $.Event.UserData.InstallDeviceID.UpgradeDevice     │            1 │
└────────────────────────────────────────────────────┴──────────────┘
  13 rows                                                 2 columns
```
InstallStatus
```
0x0: Installation Successful
0x00000002: File Not Found
0x80070002: File Not Found
0x80070003: Path Not Found
0x80070005: Access Denied
0x800F0233: Invalid Target
0x8028006E: Invalid Source Path
0x000005B3: Requires Interactive Workstation
0x000005B4: Timeout
0xE0000234: Driver Non-native
0xE0000246: Device Installer Not Ready
```



## 20003 (Service installation)
```sql
memory D SELECT Event.System.Computer, Event.UserData.AddServiceID.DriverFileName, Event.UserData.AddServiceID.DeviceInstanceID, Event.UserData.AddServiceID.ServiceName, Event.UserData.AddServiceID.PrimaryService, Event.UserData.AddServiceID.UpdateService, Event.UserData.AddServiceID.AddServiceStatus, COUNT(*)
         FROM read_json('.\system-rd01.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
         WHERE Event.System.Provider."#attributes".Name = 'Microsoft-Windows-UserPnp' AND (Event.System.EventID->>'$' = '20003')
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────┬────────────────────────────────────────────────┬─────────────────────────────────────────────────────────────┬───────────────┬────────────────┬───────────────┬──────────────────┬──────────────┐
│    Computer     │                 DriverFileName                 │                      DeviceInstanceID                       │  ServiceName  │ PrimaryService │ UpdateService │ AddServiceStatus │ count_star() │
│     varchar     │                    varchar                     │                           varchar                           │    varchar    │    boolean     │    boolean    │      int64       │    int64     │
├─────────────────┼────────────────────────────────────────────────┼─────────────────────────────────────────────────────────────┼───────────────┼────────────────┼───────────────┼──────────────────┼──────────────┤
│ OFFDEVS-TUHMGJE │ \SystemRoot\system32\DRIVERS\parport.sys       │ ACPI\PNP0400\5&2032E665&0                                   │ Parport       │ true           │ true          │                0 │            1 │
│ OFFDEVS-TUHMGJE │ \SystemRoot\system32\DRIVERS\serenum.sys       │ ACPI\PNP0501\1                                              │ Serenum       │ false          │ true          │                0 │            1 │
│ OFFDEVS-TUHMGJE │ \SystemRoot\system32\DRIVERS\serenum.sys       │ ACPI\PNP0501\2                                              │ Serenum       │ false          │ true          │                0 │            1 │
│ OFFDEVS-TUHMGJE │ \SystemRoot\system32\DRIVERS\serial.sys        │ ACPI\PNP0501\1                                              │ Serial        │ true           │ true          │                0 │            1 │
│ OFFDEVS-TUHMGJE │ \SystemRoot\system32\DRIVERS\serial.sys        │ ACPI\PNP0501\2                                              │ Serial        │ true           │ true          │                0 │            1 │
│ tpl-packer      │ %SystemRoot%\system32\vm3dservice.exe          │ PCI\VEN_15AD&DEV_0405&SUBSYS_040515AD&REV_00\3&61AAA01&0&78 │ VM3DService   │ false          │ false         │                0 │            1 │
│ tpl-packer      │ \SystemRoot\system32\DRIVERS\parport.sys       │ ACPI\PNP0400\4&2C5A7332&0                                   │ Parport       │ true           │ true          │                0 │            1 │
│ tpl-packer      │ \SystemRoot\system32\DRIVERS\vm3dmp-debug.sys  │ PCI\VEN_15AD&DEV_0405&SUBSYS_040515AD&REV_00\3&61AAA01&0&78 │ vm3dmp-debug  │ false          │ false         │                0 │            1 │
│ tpl-packer      │ \SystemRoot\system32\DRIVERS\vm3dmp-stats.sys  │ PCI\VEN_15AD&DEV_0405&SUBSYS_040515AD&REV_00\3&61AAA01&0&78 │ vm3dmp-stats  │ false          │ false         │                0 │            1 │
│ tpl-packer      │ \SystemRoot\system32\DRIVERS\vm3dmp.sys        │ PCI\VEN_15AD&DEV_0405&SUBSYS_040515AD&REV_00\3&61AAA01&0&78 │ vm3dmp        │ false          │ false         │                0 │            1 │
│ tpl-packer      │ \SystemRoot\system32\DRIVERS\vm3dmp_loader.sys │ PCI\VEN_15AD&DEV_0405&SUBSYS_040515AD&REV_00\3&61AAA01&0&78 │ vm3dmp_loader │ true           │ false         │                0 │            1 │
│ tpl-packer      │ system32\DRIVERS\xenvbd.sys                    │ XENBUS\VEN_XS0001&DEV_VBD&REV_00000001\_                    │ xenvbd        │ true           │ true          │                0 │            1 │
└─────────────────┴────────────────────────────────────────────────┴─────────────────────────────────────────────────────────────┴───────────────┴────────────────┴───────────────┴──────────────────┴──────────────┘
  12 rows                                                                                                                                                                                                 8 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\system-rd01.json') AS e, json_tree(e.json) AS je
         WHERE (e.json->>'$.Event.System.Provider."#attributes".Name' = 'Microsoft-Windows-UserPnp') AND (e.json->>'$.Event.System.EventID' = '20003') AND je.fullkey LIKE '$.Event.UserData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────────────────────┬──────────────┐
│                     fullkey                     │ count_star() │
│                     varchar                     │    int64     │
├─────────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.AddServiceID                   │           12 │
│ $.Event.UserData.AddServiceID.#attributes       │           12 │
│ $.Event.UserData.AddServiceID.#attributes.xmlns │           12 │
│ $.Event.UserData.AddServiceID.AddServiceStatus  │           12 │
│ $.Event.UserData.AddServiceID.DeviceInstanceID  │           12 │
│ $.Event.UserData.AddServiceID.DriverFileName    │           12 │
│ $.Event.UserData.AddServiceID.PrimaryService    │           12 │
│ $.Event.UserData.AddServiceID.ServiceName       │           12 │
│ $.Event.UserData.AddServiceID.UpdateService     │           12 │
└─────────────────────────────────────────────────┴──────────────┘
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
