```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f system.json E:\Windows\System32\winevt\Logs\System.evtx
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off" .\system.json
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
system_db D
```

## .databases
```sql
system_db D .databases
┌────────────────────┐
│     databases      │
│                    │
│ system_db (memory) │
└────────────────────┘
```

## .tables
```sql
system_db D .tables
 ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── system_db ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── main ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               file                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               │
│                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid varchar, eventsourcename varchar)), eventid json, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated struct("#attributes" struct(systemtime timestamp)), eventrecordid bigint, correlation struct("#attributes" struct(activityid uuid)), execution struct("#attributes" struct(processid bigint, threadid bigint)), channel varchar, computer varchar, "security" struct("#attributes" struct(userid varchar))), eventdata map(varchar, json), userdata struct(addserviceid struct("#attributes" struct(xmlns varchar), servicename varchar, driverfilename varchar, deviceinstanceid varchar, primaryservice boolean, updateservice boolean, addservicestatus bigint), logfilecleared struct("#attributes" struct(xmlns varchar), subjectusername varchar, subjectdomainname varchar, channel varchar, backuppath json))) │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            system_db                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             │
│                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid varchar, eventsourcename varchar)), eventid json, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated struct("#attributes" struct(systemtime timestamp)), eventrecordid bigint, correlation struct("#attributes" struct(activityid uuid)), execution struct("#attributes" struct(processid bigint, threadid bigint)), channel varchar, computer varchar, "security" struct("#attributes" struct(userid varchar))), eventdata map(varchar, json), userdata struct(addserviceid struct("#attributes" struct(xmlns varchar), servicename varchar, driverfilename varchar, deviceinstanceid varchar, primaryservice boolean, updateservice boolean, addservicestatus bigint), logfilecleared struct("#attributes" struct(xmlns varchar), subjectusername varchar, subjectdomainname varchar, channel varchar, backuppath json))) │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## DESCRIBE
```sql
system_db D DESCRIBE;
┌───────────┬─────────┬───────────┬──────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────┐
│ database  │ schema  │   name    │ column_names │                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          column_types                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          │ temporary │
│  varchar  │ varchar │  varchar  │  varchar[]   │                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           varchar[]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            │  boolean  │
├───────────┼─────────┼───────────┼──────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────┤
│ system_db │ main    │ file      │ [Event]      │ ['STRUCT("#attributes" STRUCT(xmlns VARCHAR), "System" STRUCT(Provider STRUCT("#attributes" STRUCT("Name" VARCHAR, Guid VARCHAR, EventSourceName VARCHAR)), EventID JSON, "Version" BIGINT, "Level" BIGINT, Task BIGINT, Opcode BIGINT, Keywords VARCHAR, TimeCreated STRUCT("#attributes" STRUCT(SystemTime TIMESTAMP)), EventRecordID BIGINT, Correlation STRUCT("#attributes" STRUCT(ActivityID UUID)), Execution STRUCT("#attributes" STRUCT(ProcessID BIGINT, ThreadID BIGINT)), Channel VARCHAR, Computer VARCHAR, "Security" STRUCT("#attributes" STRUCT(UserID VARCHAR))), EventData MAP(VARCHAR, JSON), UserData STRUCT(AddServiceID STRUCT("#attributes" STRUCT(xmlns VARCHAR), ServiceName VARCHAR, DriverFileName VARCHAR, DeviceInstanceID VARCHAR, PrimaryService BOOLEAN, UpdateService BOOLEAN, AddServiceStatus BIGINT), LogFileCleared STRUCT("#attributes" STRUCT(xmlns VARCHAR), SubjectUserName VARCHAR, SubjectDomainName VARCHAR, Channel VARCHAR, BackupPath JSON)))'] │ false     │
│ system_db │ main    │ system_db │ [Event]      │ ['STRUCT("#attributes" STRUCT(xmlns VARCHAR), "System" STRUCT(Provider STRUCT("#attributes" STRUCT("Name" VARCHAR, Guid VARCHAR, EventSourceName VARCHAR)), EventID JSON, "Version" BIGINT, "Level" BIGINT, Task BIGINT, Opcode BIGINT, Keywords VARCHAR, TimeCreated STRUCT("#attributes" STRUCT(SystemTime TIMESTAMP)), EventRecordID BIGINT, Correlation STRUCT("#attributes" STRUCT(ActivityID UUID)), Execution STRUCT("#attributes" STRUCT(ProcessID BIGINT, ThreadID BIGINT)), Channel VARCHAR, Computer VARCHAR, "Security" STRUCT("#attributes" STRUCT(UserID VARCHAR))), EventData MAP(VARCHAR, JSON), UserData STRUCT(AddServiceID STRUCT("#attributes" STRUCT(xmlns VARCHAR), ServiceName VARCHAR, DriverFileName VARCHAR, DeviceInstanceID VARCHAR, PrimaryService BOOLEAN, UpdateService BOOLEAN, AddServiceStatus BIGINT), LogFileCleared STRUCT("#attributes" STRUCT(xmlns VARCHAR), SubjectUserName VARCHAR, SubjectDomainName VARCHAR, Channel VARCHAR, BackupPath JSON)))'] │ false     │
└───────────┴─────────┴───────────┴──────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────┘
```

## EventID
```sql
system_db D WITH evtx AS (
                SELECT Event.System.Computer AS Computer, Event.System.Provider."#attributes".Name, Event.System.TimeCreated."#attributes".SystemTime,
                    TRY_CAST(
                        CASE
                            WHEN json_type(Event.System.EventID) = 'OBJECT'
                            THEN json_extract_string(Event.System.EventID, '$."#text"')
                            ELSE json_extract_string(Event.System.EventID, '$')
                        END AS BIGINT
                    ) AS EventID
                FROM read_json('C:/Users/SANSDFIR/system.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
            )
            SELECT Name, EventID, COUNT(*)
            FROM evtx
            WHERE SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
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
system-rd05 D SELECT Event.EventData.Caption, Event.EventData.Message, COUNT(*)
              FROM read_json('C:/Users/SANSDFIR/system-rd05.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
              AND json_extract_string(Event.System.EventID, '$') = '26'
              GROUP BY ALL
              ORDER BY ALL;
┌──────────────────────────────────────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│                   Caption                    │                                                                              Message                                                                               │ count_star() │
│                   varchar                    │                                                                              varchar                                                                               │    int64     │
├──────────────────────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ CompatTelRunner.exe - Fatal Application Exit │ Exception Processing Message 0x40000015 - Unexpected parameters                                                                                                    │            1 │
│ PhoneExperienceHost.exe - Application Error  │ The exception unknown software exception (0xe0434352) occurred in the application at location 0x00007FFC6495428C.\r\n                                              │            1 │
│ Windows - Out of Virtual Memory              │ Your system is low on virtual memory. To ensure that Windows runs properly, increase the size of your virtual memory paging file. For more information, see Help.  │            1 │
└──────────────────────────────────────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
system-rd05 D SELECT je.fullkey, COUNT(*)
              FROM read_json_objects('C:/Users/SANSDFIR/system-rd05.json') AS e, json_tree(e.json) AS je
              WHERE e.json->>'$.Event.System.EventID' = '26' AND je.fullkey LIKE '$.Event.EventData.%'
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



## 6005 (The Event log service was started)
```sql
system-rd01 D SELECT Event.System.TimeCreated."#attributes".SystemTime , COUNT(*)
              FROM read_json('C:/Users/SANSDFIR/system-rd05.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
              AND json_extract_string(Event.System.EventID, '$."#text"') = '6005'
              GROUP BY ALL
              ORDER BY ALL;
┌────────────────────────────┬──────────────┐
│         SystemTime         │ count_star() │
│         timestamp          │    int64     │
├────────────────────────────┼──────────────┤
│ 2023-01-02 12:38:08.386482 │            1 │
│ 2023-01-28 01:10:33.741012 │            1 │
└────────────────────────────┴──────────────┘
```
fullkey
```sql
system-rd01 D SELECT je.fullkey, COUNT(*)
              FROM read_json_objects('C:/Users/SANSDFIR/system-rd01.json') AS e, json_tree(e.json) AS je
              WHERE e.json->>'$.Event.System.EventID."#text"' = '6005' AND je.fullkey LIKE '$.Event.EventData.%'
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
