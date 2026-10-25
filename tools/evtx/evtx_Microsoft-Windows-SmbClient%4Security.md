```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f SmbClient%4Security.json C:\Windows\System32\winevt\logs\Microsoft-Windows-SmbClient%4Security.evtx
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
memory D SELECT Event.System.EventID, COUNT(*)
         FROM read_json('.\SmbClient%4Security.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│   31000 │            2 │
│   31001 │            1 │
│   31002 │            1 │
│   31010 │         4329 │
│   31013 │           20 │
│   31018 │          321 │
└─────────┴──────────────┘
```



## 31001 (SMB Client: DiagReasonISC)
```sql
memory D SELECT Event.System.Computer, Event.EventData.ServerName, Event.EventData.PrincipalName, Event.EventData.UserName, Event.EventData.Status, Event.EventData.SecurityStatus, Event.EventData.Reason, COUNT(*)
         FROM read_json('.\SmbClient%4Security-wkstn03.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 31001
         GROUP BY ALL
         ORDER BY Event.EventData.ServerName;
┌────────────────────────┬──────────────────────┬──────────────────────────┬──────────┬────────────┬────────────────┬────────┬──────────────┐
│        Computer        │      ServerName      │      PrincipalName       │ UserName │   Status   │ SecurityStatus │ Reason │ count_star() │
│        varchar         │       varchar        │         varchar          │ varchar  │   int64    │     int64      │ int64  │    int64     │
├────────────────────────┼──────────────────────┼──────────────────────────┼──────────┼────────────┼────────────────┼────────┼──────────────┤
│ wkstn03.shieldbase.com │ \dc01.shieldbase.com │ cifs/dc01.shieldbase.com │          │ 3221225626 │     3221225626 │     10 │            1 │
└────────────────────────┴──────────────────────┴──────────────────────────┴──────────┴────────────┴────────────────┴────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey
         FROM read_json_objects('.\SmbClient%4Security-wkstn03.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '31001' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────────────┐
│                fullkey                │
│                varchar                │
├───────────────────────────────────────┤
│ $.Event.EventData.LogonId             │
│ $.Event.EventData.PrincipalName       │
│ $.Event.EventData.PrincipalNameLength │
│ $.Event.EventData.Reason              │
│ $.Event.EventData.SecurityStatus      │
│ $.Event.EventData.ServerName          │
│ $.Event.EventData.ServerNameLength    │
│ $.Event.EventData.Status              │
│ $.Event.EventData.UserName            │
│ $.Event.EventData.UserNameLength      │
└───────────────────────────────────────┘
                 10 rows
```
Status
```
0xC000005E: there are currently no logon servers available to service the logon request
3221225566: there are currently no logon servers available to service the logon request
0xC0000064: user name does not exist
3221225572: user name does not exist
0xC000006A: user name is correct but the password is wrong
3221225578: user name is correct but the password is wrong
0xC000006D: the cause is either a bad username or authentication information
3221225581: the cause is either a bad username or authentication information
0xC000006E: some user account restriction has prevented successful authentication
3221225582: some user account restriction has prevented successful authentication
0xC000006F: user logon outside authorized hours
3221225583: user logon outside authorized hours
0xC0000070: user logon from unauthorized workstation
3221225584: user logon from unauthorized workstation
0xC0000071: user logon with expired password
3221225585: user logon with expired password
0xC0000072: user logon to account disabled by administrator
3221225586: user logon to account disabled by administrator
0xC00000DC: indicates the Sam Server was in the wrong state to perform the desired operation
3221225692: indicates the Sam Server was in the wrong state to perform the desired operation
0xC0000133: clocks between DC and other computer too far out of sync
3221225779: clocks between DC and other computer too far out of sync
0xC000015B: the user has not been granted the requested logon type at this machine
3221225819: the user has not been granted the requested logon type at this machine
0xC000018C: the trust relationship between the primary domain and the trusted domain failed
3221225868: the trust relationship between the primary domain and the trusted domain failed
0xC0000192: an attempt was made to logon, but the Netlogon service was not started.
3221225874: an attempt was made to logon, but the Netlogon service was not started.
0xC0000193: user logon with expired account
3221225875: user logon with expired account
0xC0000224: user is required to change password at next logon
3221226020: user is required to change password at next logon
0xC0000225: evidently a bug in Windows and not a risk
3221226021: evidently a bug in Windows and not a risk
0xC0000234: user is currently locked out
3221226036: user is currently locked out
0x0: Status OK
0: Status OK
```



## 31010 (The SMB client failed to connect to the share)
```sql
memory D SELECT Event.System.Computer, Event.EventData.ShareName, Event.EventData.ObjectName, Event.EventData.Status, Event.EventData.Reason, COUNT(*)
         FROM read_json('.\SmbClient%4Security-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 31010
         GROUP BY ALL
         ORDER BY Event.EventData.ShareName;
┌─────────────────────┬────────────────────────────┬────────────┬────────────┬────────┬──────────────┐
│      Computer       │         ShareName          │ ObjectName │   Status   │ Reason │ count_star() │
│       varchar       │          varchar           │  varchar   │   int64    │ int64  │    int64     │
├─────────────────────┼────────────────────────────┼────────────┼────────────┼────────┼──────────────┤
│ rd01.shieldbase.com │ \172.16.6.12\c$            │            │ 3221225506 │     12 │          210 │
│ rd01.shieldbase.com │ \172.16.6.13\c$            │            │ 3221225506 │     12 │          658 │
│ rd01.shieldbase.com │ \172.16.6.14\c$            │            │ 3221225506 │     12 │           27 │
│ rd01.shieldbase.com │ \172.16.6.19\c$            │            │ 3221225506 │     12 │          453 │
│ rd01.shieldbase.com │ \dev01.shieldbase.com\C$   │            │ 3221225506 │     12 │          911 │
│ rd01.shieldbase.com │ \dev01.shieldbase.com\c$   │            │ 3221225506 │     12 │           50 │
│ rd01.shieldbase.com │ \rd09.shieldbase.com\c$    │            │ 3221225506 │     12 │            5 │
│ rd01.shieldbase.com │ \wkstn01.shieldbase.com\c$ │            │ 3221225506 │     12 │            2 │
└─────────────────────┴────────────────────────────┴────────────┴────────────┴────────┴──────────────
```
fullkey
```sql
memory D SELECT je.fullkey
         FROM read_json_objects('.\SmbClient%4Security-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '31010' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┐
│              fullkey               │
│              varchar               │
├────────────────────────────────────┤
│ $.Event.EventData.ObjectName       │
│ $.Event.EventData.ObjectNameLength │
│ $.Event.EventData.Reason           │
│ $.Event.EventData.ShareName        │
│ $.Event.EventData.ShareNameLength  │
│ $.Event.EventData.Status           │
└────────────────────────────────────┘
```
Reason
```
12: Access Denied.
```
