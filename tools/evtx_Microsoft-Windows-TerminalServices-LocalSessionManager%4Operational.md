```powershell
PS C:\Users\SANSDFIR>  .\evtx_dump-v0.12.3.exe -o jsonl -f TerminalServices-LocalSessionManager%4Operational.json C:\C\Windows\System32\winevt\logs\Microsoft-Windows-TerminalServices-LocalSessionManager%4Operational.evtx
PS C:\Users\SANSDFIR>
```

## .cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off" .\TerminalServices-LocalSessionManager%4Operational.json
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
TerminalServices-LocalSessionManager%4Op... D
```

## .databases
```sql
TerminalServices-LocalSessionManager%4Op... D　.databases
┌────────────────────────────────────────────────────────────┐
│                         databases                          │
│                                                            │
│ TerminalServices-LocalSessionManager%4Operational (memory) │
└────────────────────────────────────────────────────────────┘
```

## .tables
```sql
TerminalServices-LocalSessionManager%4Op... D .tables
 ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── TerminalServices-LocalSessionManager%4Operational ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── main ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                                                                                                                                                                                                                                                                                                                                                              file                                                                                                                                                                                                                                                                                                                                                                                                                                                              │
│                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid uuid)), eventid bigint, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated struct("#attributes" struct(systemtime timestamp)), eventrecordid bigint, correlation struct("#attributes" struct(activityid uuid)), execution struct("#attributes" struct(processid bigint, threadid bigint)), channel varchar, computer varchar, "security" struct("#attributes" struct(userid varchar))), userdata struct(eventxml struct("#attributes" struct(xmlns varchar), messagename varchar, "user" varchar, sessionid bigint, address varchar, targetsession bigint, "source" bigint, "session" bigint, reason bigint, param1 varchar)), eventdata struct(sessionid bigint, state bigint, statename varchar, "event" bigint, eventname varchar, errorcode varchar)) │
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                                                                                                                                                                                                                                                                                                                                       TerminalServices-LocalSessionManager%4Operational                                                                                                                                                                                                                                                                                                                                                                                                                                        │
│                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid uuid)), eventid bigint, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated struct("#attributes" struct(systemtime timestamp)), eventrecordid bigint, correlation struct("#attributes" struct(activityid uuid)), execution struct("#attributes" struct(processid bigint, threadid bigint)), channel varchar, computer varchar, "security" struct("#attributes" struct(userid varchar))), userdata struct(eventxml struct("#attributes" struct(xmlns varchar), messagename varchar, "user" varchar, sessionid bigint, address varchar, targetsession bigint, "source" bigint, "session" bigint, reason bigint, param1 varchar)), eventdata struct(sessionid bigint, state bigint, statename varchar, "event" bigint, eventname varchar, errorcode varchar)) │
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## DESCRIBE
```Sql
TerminalServices-LocalSessionManager%4Op... D DESCRIBE;
┌───────────────────────────────────────────────────┬─────────┬───────────────────────────────────────────────────┬──────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────┐
│                     database                      │ schema  │                       name                        │ column_names │                                                                                                                                                                                                                                                                                                                                                                                                                                                         column_types                                                                                                                                                                                                                                                                                                                                                                                                                                                         │ temporary │
│                      varchar                      │ varchar │                      varchar                      │  varchar[]   │                                                                                                                                                                                                                                                                                                                                                                                                                                                          varchar[]                                                                                                                                                                                                                                                                                                                                                                                                                                                           │  boolean  │
├───────────────────────────────────────────────────┼─────────┼───────────────────────────────────────────────────┼──────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────┤
│ TerminalServices-LocalSessionManager%4Operational │ main    │ TerminalServices-LocalSessionManager%4Operational │ [Event]      │ ['STRUCT("#attributes" STRUCT(xmlns VARCHAR), "System" STRUCT(Provider STRUCT("#attributes" STRUCT("Name" VARCHAR, Guid UUID)), EventID BIGINT, "Version" BIGINT, "Level" BIGINT, Task BIGINT, Opcode BIGINT, Keywords VARCHAR, TimeCreated STRUCT("#attributes" STRUCT(SystemTime TIMESTAMP)), EventRecordID BIGINT, Correlation STRUCT("#attributes" STRUCT(ActivityID UUID)), Execution STRUCT("#attributes" STRUCT(ProcessID BIGINT, ThreadID BIGINT)), Channel VARCHAR, Computer VARCHAR, "Security" STRUCT("#attributes" STRUCT(UserID VARCHAR))), UserData STRUCT(EventXML STRUCT("#attributes" STRUCT(xmlns VARCHAR), messageName VARCHAR, "User" VARCHAR, SessionID BIGINT, Address VARCHAR, TargetSession BIGINT, "Source" BIGINT, "Session" BIGINT, Reason BIGINT, Param1 VARCHAR)), EventData STRUCT(SessionId BIGINT, State BIGINT, StateName VARCHAR, "Event" BIGINT, EventName VARCHAR, ErrorCode VARCHAR))'] │ false     │
│ TerminalServices-LocalSessionManager%4Operational │ main    │ file                                              │ [Event]      │ ['STRUCT("#attributes" STRUCT(xmlns VARCHAR), "System" STRUCT(Provider STRUCT("#attributes" STRUCT("Name" VARCHAR, Guid UUID)), EventID BIGINT, "Version" BIGINT, "Level" BIGINT, Task BIGINT, Opcode BIGINT, Keywords VARCHAR, TimeCreated STRUCT("#attributes" STRUCT(SystemTime TIMESTAMP)), EventRecordID BIGINT, Correlation STRUCT("#attributes" STRUCT(ActivityID UUID)), Execution STRUCT("#attributes" STRUCT(ProcessID BIGINT, ThreadID BIGINT)), Channel VARCHAR, Computer VARCHAR, "Security" STRUCT("#attributes" STRUCT(UserID VARCHAR))), UserData STRUCT(EventXML STRUCT("#attributes" STRUCT(xmlns VARCHAR), messageName VARCHAR, "User" VARCHAR, SessionID BIGINT, Address VARCHAR, TargetSession BIGINT, "Source" BIGINT, "Session" BIGINT, Reason BIGINT, Param1 VARCHAR)), EventData STRUCT(SessionId BIGINT, State BIGINT, StateName VARCHAR, "Event" BIGINT, EventName VARCHAR, ErrorCode VARCHAR))'] │ false     │
└───────────────────────────────────────────────────┴─────────┴───────────────────────────────────────────────────┴──────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────┘
```

## EventID
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT Event.System.Computer, Event.System.EventID, COUNT(*)
                                              FROM read_json('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                                              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                                              GROUP BY ALL
                                              ORDER BY ALL;
┌─────────────────────┬─────────┬──────────────┐
│      Computer       │ EventID │ count_star() │
│       varchar       │  int64  │    int64     │
├─────────────────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │      21 │           19 │
│ rd01.shieldbase.com │      22 │           19 │
│ rd01.shieldbase.com │      23 │           18 │
│ rd01.shieldbase.com │      24 │           30 │
│ rd01.shieldbase.com │      25 │           12 │
│ rd01.shieldbase.com │      32 │            4 │
│ rd01.shieldbase.com │      40 │           62 │
│ rd01.shieldbase.com │      41 │           29 │
│ rd01.shieldbase.com │      42 │           29 │
│ rd01.shieldbase.com │      54 │            4 │
└─────────────────────┴─────────┴──────────────┘
  10 rows                            3 columns
```



## 21 (Remote Desktop Services: Session logon succeeded)
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT Event.System.Computer, Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User, Event.UserData.EventXML.Address, COUNT(*)
                                              FROM read_json('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                                              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                                              AND Event.System.EventID = 21
                                              GROUP BY ALL
                                              ORDER BY Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User, Event.UserData.EventXML.Address;
┌─────────────────────┬───────────┬────────────────────┬──────────────┬──────────────┐
│      Computer       │ SessionID │        User        │   Address    │ count_star() │
│       varchar       │   int64   │      varchar       │   varchar    │    int64     │
├─────────────────────┼───────────┼────────────────────┼──────────────┼──────────────┤
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │ 172.16.30.14 │            1 │
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │ 172.16.30.23 │            2 │
│ rd01.shieldbase.com │         3 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         3 │ shieldbase\wacsvc  │ 172.16.4.9   │            1 │
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │         5 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         6 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         7 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │         8 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │         9 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        10 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        11 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │        11 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        12 │ shieldbase\wacsvc  │ 172.16.6.18  │            1 │
│ rd01.shieldbase.com │        13 │ shieldbase\tdungan │ 172.16.30.3  │            1 │
│ rd01.shieldbase.com │        14 │ shieldbase\tdungan │ 172.16.30.3  │            1 │
└─────────────────────┴───────────┴────────────────────┴──────────────┴──────────────┘
  18 rows                                                                  5 columns
```
fullkey
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT je.fullkey, COUNT(*)
                                              FROM read_json_objects('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json') AS e, json_tree(e.json) AS je
                                              WHERE e.json->>'$.Event.System.EventID' = '21' AND je.fullkey LIKE '$.Event.UserData.EventXML.%'
                                              GROUP BY je.fullkey
                                              ORDER BY je.fullkey;
┌─────────────────────────────────────────────┬──────────────┐
│                   fullkey                   │ count_star() │
│                   varchar                   │    int64     │
├─────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.EventXML.#attributes       │           46 │
│ $.Event.UserData.EventXML.#attributes.xmlns │           46 │
│ $.Event.UserData.EventXML.Address           │           46 │
│ $.Event.UserData.EventXML.SessionID         │           46 │
│ $.Event.UserData.EventXML.User              │           46 │
└─────────────────────────────────────────────┴──────────────┘
```



## 22 (Remote Desktop Services: Shell start notification received)
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT Event.System.Computer, Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User, Event.UserData.EventXML.Address, COUNT(*)
                                              FROM read_json('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                                              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                                              AND Event.System.EventID = 22
                                              GROUP BY ALL
                                              ORDER BY Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User, Event.UserData.EventXML.Address;
┌─────────────────────┬───────────┬────────────────────┬──────────────┬──────────────┐
│      Computer       │ SessionID │        User        │   Address    │ count_star() │
│       varchar       │   int64   │      varchar       │   varchar    │    int64     │
├─────────────────────┼───────────┼────────────────────┼──────────────┼──────────────┤
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │ 172.16.30.14 │            1 │
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │ 172.16.30.23 │            2 │
│ rd01.shieldbase.com │         3 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         3 │ shieldbase\wacsvc  │ 172.16.4.9   │            1 │
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │         5 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         6 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         7 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │         8 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │         9 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        10 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        11 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │        11 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        12 │ shieldbase\wacsvc  │ 172.16.6.18  │            1 │
│ rd01.shieldbase.com │        13 │ shieldbase\tdungan │ 172.16.30.3  │            1 │
│ rd01.shieldbase.com │        14 │ shieldbase\tdungan │ 172.16.30.3  │            1 │
└─────────────────────┴───────────┴────────────────────┴──────────────┴──────────────┘
  18 rows                                                                  5 columns
```
fullkey
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT je.fullkey, COUNT(*)
                                              FROM read_json_objects('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json') AS e, json_tree(e.json) AS je
                                              WHERE e.json->>'$.Event.System.EventID' = '22' AND je.fullkey LIKE '$.Event.UserData.EventXML.%'
                                              GROUP BY je.fullkey
                                              ORDER BY je.fullkey;
┌─────────────────────────────────────────────┬──────────────┐
│                   fullkey                   │ count_star() │
│                   varchar                   │    int64     │
├─────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.EventXML.#attributes       │           45 │
│ $.Event.UserData.EventXML.#attributes.xmlns │           45 │
│ $.Event.UserData.EventXML.Address           │           45 │
│ $.Event.UserData.EventXML.SessionID         │           45 │
│ $.Event.UserData.EventXML.User              │           45 │
└─────────────────────────────────────────────┴──────────────┘
```



## 23 (Remote Desktop Services: Session logoff succeeded)
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT Event.System.Computer, Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User, COUNT(*)
                                              FROM read_json('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                                              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                                              AND Event.System.EventID = 23
                                              GROUP BY ALL
                                              ORDER BY Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User;
┌─────────────────────┬───────────┬────────────────────┬──────────────┐
│      Computer       │ SessionID │        User        │ count_star() │
│       varchar       │   int64   │      varchar       │    int64     │
├─────────────────────┼───────────┼────────────────────┼──────────────┤
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │            3 │
│ rd01.shieldbase.com │         3 │ shieldbase\tdungan │            1 │
│ rd01.shieldbase.com │         3 │ shieldbase\wacsvc  │            1 │
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │            2 │
│ rd01.shieldbase.com │         5 │ shieldbase\tdungan │            1 │
│ rd01.shieldbase.com │         6 │ shieldbase\tdungan │            1 │
│ rd01.shieldbase.com │         7 │ shieldbase\tdungan │            1 │
│ rd01.shieldbase.com │         8 │ shieldbase\tdungan │            1 │
│ rd01.shieldbase.com │         9 │ shieldbase\tdungan │            1 │
│ rd01.shieldbase.com │        10 │ shieldbase\tdungan │            1 │
│ rd01.shieldbase.com │        11 │ shieldbase\tdungan │            2 │
│ rd01.shieldbase.com │        12 │ shieldbase\wacsvc  │            1 │
│ rd01.shieldbase.com │        13 │ shieldbase\tdungan │            1 │
│ rd01.shieldbase.com │        14 │ shieldbase\tdungan │            1 │
└─────────────────────┴───────────┴────────────────────┴──────────────┘
  14 rows                                                   4 columns
```
fullkey
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT je.fullkey, COUNT(*)
                                              FROM read_json_objects('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json') AS e, json_tree(e.json) AS je
                                              WHERE e.json->>'$.Event.System.EventID' = '23' AND je.fullkey LIKE '$.Event.UserData.EventXML.%'
                                              GROUP BY je.fullkey
                                              ORDER BY je.fullkey;
┌─────────────────────────────────────────────┬──────────────┐
│                   fullkey                   │ count_star() │
│                   varchar                   │    int64     │
├─────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.EventXML.#attributes       │           45 │
│ $.Event.UserData.EventXML.#attributes.xmlns │           45 │
│ $.Event.UserData.EventXML.SessionID         │           45 │
│ $.Event.UserData.EventXML.User              │           45 │
└─────────────────────────────────────────────┴──────────────┘
```



## 24 (Remote Desktop Services: Session has been disconnected)
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT Event.System.Computer, Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User, Event.UserData.EventXML.Address, COUNT(*)
                                              FROM read_json('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                                              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                                              AND Event.System.EventID = 24
                                              GROUP BY ALL
                                              ORDER BY Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User, Event.UserData.EventXML.Address;
┌─────────────────────┬───────────┬────────────────────┬──────────────┬──────────────┐
│      Computer       │ SessionID │        User        │   Address    │ count_star() │
│       varchar       │   int64   │      varchar       │   varchar    │    int64     │
├─────────────────────┼───────────┼────────────────────┼──────────────┼──────────────┤
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │ 172.16.30.14 │            1 │
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         2 │ shieldbase\tdungan │ 172.16.30.23 │            1 │
│ rd01.shieldbase.com │         3 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         3 │ shieldbase\wacsvc  │ 172.16.4.9   │            1 │
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │ 172.16.30.23 │            1 │
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │ 172.16.30.8  │            2 │
│ rd01.shieldbase.com │         5 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         6 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │         7 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │         8 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │         9 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        10 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        11 │ shieldbase\tdungan │ 172.16.30.20 │            1 │
│ rd01.shieldbase.com │        11 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        12 │ shieldbase\wacsvc  │ 172.16.6.18  │            8 │
│ rd01.shieldbase.com │        13 │ shieldbase\tdungan │ 172.16.30.3  │            2 │
│ rd01.shieldbase.com │        14 │ shieldbase\tdungan │ 172.16.30.14 │            2 │
│ rd01.shieldbase.com │        14 │ shieldbase\tdungan │ 172.16.30.3  │            1 │
└─────────────────────┴───────────┴────────────────────┴──────────────┴──────────────┘
  20 rows                                                                  5 columns
```
fullkey
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT je.fullkey, COUNT(*)
                                              FROM read_json_objects('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json') AS e, json_tree(e.json) AS je
                                              WHERE e.json->>'$.Event.System.EventID' = '24' AND je.fullkey LIKE '$.Event.UserData.EventXML.%'
                                              GROUP BY je.fullkey
                                              ORDER BY je.fullkey;
┌─────────────────────────────────────────────┬──────────────┐
│                   fullkey                   │ count_star() │
│                   varchar                   │    int64     │
├─────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.EventXML.#attributes       │           78 │
│ $.Event.UserData.EventXML.#attributes.xmlns │           78 │
│ $.Event.UserData.EventXML.Address           │           78 │
│ $.Event.UserData.EventXML.SessionID         │           78 │
│ $.Event.UserData.EventXML.User              │           78 │
└─────────────────────────────────────────────┴──────────────┘
```



## 25 (Remote Desktop Services: Session reconnection succeeded)
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT Event.System.Computer, Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User, Event.UserData.EventXML.Address, COUNT(*)
                                              FROM read_json('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                                              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                                              AND Event.System.EventID = 25
                                              GROUP BY ALL
                                              ORDER BY Event.UserData.EventXML.SessionID, Event.UserData.EventXML.User, Event.UserData.EventXML.Address;
┌─────────────────────┬───────────┬────────────────────┬──────────────┬──────────────┐
│      Computer       │ SessionID │        User        │   Address    │ count_star() │
│       varchar       │   int64   │      varchar       │   varchar    │    int64     │
├─────────────────────┼───────────┼────────────────────┼──────────────┼──────────────┤
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │ 172.16.30.23 │            1 │
│ rd01.shieldbase.com │         4 │ shieldbase\tdungan │ 172.16.30.8  │            1 │
│ rd01.shieldbase.com │        12 │ shieldbase\wacsvc  │ 172.16.6.18  │            7 │
│ rd01.shieldbase.com │        13 │ shieldbase\tdungan │ 172.16.30.3  │            1 │
│ rd01.shieldbase.com │        14 │ shieldbase\tdungan │ 172.16.30.14 │            2 │
└─────────────────────┴───────────┴────────────────────┴──────────────┴──────────────┘
```
fullkey
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT je.fullkey, COUNT(*)
                                              FROM read_json_objects('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json') AS e, json_tree(e.json) AS je
                                              WHERE e.json->>'$.Event.System.EventID' = '25' AND je.fullkey LIKE '$.Event.UserData.EventXML.%'
                                              GROUP BY je.fullkey
                                              ORDER BY je.fullkey;
┌─────────────────────────────────────────────┬──────────────┐
│                   fullkey                   │ count_star() │
│                   varchar                   │    int64     │
├─────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.EventXML.#attributes       │           36 │
│ $.Event.UserData.EventXML.#attributes.xmlns │           36 │
│ $.Event.UserData.EventXML.Address           │           36 │
│ $.Event.UserData.EventXML.SessionID         │           36 │
│ $.Event.UserData.EventXML.User              │           36 │
└─────────────────────────────────────────────┴──────────────┘
```



## 40 (Session (PayloadData1) has been disconnected, reason code (PayloadData2))
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT Event.System.Computer, Event.UserData.EventXML.Session, Event.UserData.EventXML.Reason, COUNT(*)
                                              FROM read_json('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                                              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                                              AND Event.System.EventID = 40
                                              GROUP BY ALL
                                              ORDER BY Event.UserData.EventXML.Session, Event.UserData.EventXML.Reason;
┌─────────────────────┬─────────┬────────────┬──────────────┐
│      Computer       │ Session │   Reason   │ count_star() │
│       varchar       │  int64  │   int64    │    int64     │
├─────────────────────┼─────────┼────────────┼──────────────┤
│ rd01.shieldbase.com │       2 │         12 │            2 │
│ rd01.shieldbase.com │       2 │         25 │            1 │
│ rd01.shieldbase.com │       3 │         12 │            2 │
│ rd01.shieldbase.com │       4 │          0 │            2 │
│ rd01.shieldbase.com │       4 │          5 │            2 │
│ rd01.shieldbase.com │       4 │         12 │            1 │
│ rd01.shieldbase.com │       4 │         26 │            1 │
│ rd01.shieldbase.com │       4 │ 2147942464 │            1 │
│ rd01.shieldbase.com │       5 │          0 │            1 │
│ rd01.shieldbase.com │       5 │         12 │            1 │
│ rd01.shieldbase.com │       6 │         12 │            1 │
│ rd01.shieldbase.com │       7 │         12 │            1 │
│ rd01.shieldbase.com │       8 │         12 │            1 │
│ rd01.shieldbase.com │       9 │         12 │            1 │
│ rd01.shieldbase.com │      10 │         12 │            1 │
│ rd01.shieldbase.com │      11 │         12 │            1 │
│ rd01.shieldbase.com │      11 │         26 │            1 │
│ rd01.shieldbase.com │      11 │ 2147942464 │            2 │
│ rd01.shieldbase.com │      12 │          0 │            2 │
│ rd01.shieldbase.com │      12 │          5 │            7 │
│ rd01.shieldbase.com │      12 │ 2147949465 │            8 │
│ rd01.shieldbase.com │      13 │          0 │            1 │
│ rd01.shieldbase.com │      13 │          5 │            1 │
│ rd01.shieldbase.com │      13 │         12 │            1 │
│ rd01.shieldbase.com │      14 │          0 │            8 │
│ rd01.shieldbase.com │      14 │          5 │            2 │
│ rd01.shieldbase.com │      14 │         12 │            2 │
│ rd01.shieldbase.com │      14 │         26 │            1 │
│ rd01.shieldbase.com │      14 │ 2147949465 │            1 │
│ rd01.shieldbase.com │      15 │          0 │            2 │
│ rd01.shieldbase.com │      15 │         12 │            1 │
│ rd01.shieldbase.com │      15 │ 2147949465 │            2 │
└─────────────────────┴─────────┴────────────┴──────────────┘
  32 rows                                         4 columns
```
fullkey
```sql
TerminalServices-LocalSessionManager%4Op... D SELECT je.fullkey, COUNT(*)
                                              FROM read_json_objects('C:/Users/SANSDFIR/TerminalServices-LocalSessionManager%4Operational.json') AS e, json_tree(e.json) AS je
                                              WHERE e.json->>'$.Event.System.EventID' = '40' AND je.fullkey LIKE '$.Event.UserData.EventXML.%'
                                              GROUP BY je.fullkey
                                              ORDER BY je.fullkey;
┌─────────────────────────────────────────────┬──────────────┐
│                   fullkey                   │ count_star() │
│                   varchar                   │    int64     │
├─────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.EventXML.#attributes       │          174 │
│ $.Event.UserData.EventXML.#attributes.xmlns │          174 │
│ $.Event.UserData.EventXML.Reason            │          174 │
│ $.Event.UserData.EventXML.Session           │          174 │
└─────────────────────────────────────────────┴──────────────┘
```
Reason
```
0: No additional information is available (i.e. the user has closed RDP window)
1: The disconnection was initiated by an administrative tool on the server in another session
2: The disconnection was due to a forced logoff initiated by an administrative tool on the server in another session
3: The idle session limit timer on the server has elapsed
4: The active session limit timer on the server has elapsed
5: The client's connection was replaced by another connection (i.e. a user reconnected to a previous RDP session)
6: The server ran out of available memory resources
7: The server denied the connection
9: The user cannot connect to the server due to insufficient access privileges
10: The server does not accept saved user credentials and requires that the user enter their credentials for each connection
11: The disconnection was initiated by the user disconnecting his or her session on the server or by an administrative tool on the server
12: The disconnection was initiated by the user logging off his or her session on the server
```
