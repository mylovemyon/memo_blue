```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f SmbClient%4Connectivity.json C:\Windows\System32\winevt\logs\Microsoft-Windows-SmbClient%4Connectivity.evtx
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
         FROM read_json('.\SmbClient%4Connectivity.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│   30800 │          182 │
│   30801 │            2 │
│   30803 │           43 │
│   30804 │           81 │
│   30805 │          103 │
│   30806 │           76 │
│   30807 │          118 │
│   30808 │          155 │
│   30810 │          220 │
│   30811 │           26 │
│   30812 │           99 │
│   30813 │            3 │
│   30822 │            9 │
└─────────┴──────────────┘
  13 rows      2 columns
```



## 30805 (SMB Client: the client lost its session to the server")
```sql
memory D SELECT Event.System.Computer, Event.EventData.ServerName, Event.EventData.Address, Event.EventData.TreeId, hex(Event.EventData.Status), COUNT(*)
         FROM read_json('.\SmbClient%4Connectivity-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 30805
         GROUP BY ALL
         ORDER BY Event.EventData.ServerName;
┌─────────────────────┬────────────┬─────────┬────────┬───────────────────────────────┬──────────────┐
│      Computer       │ ServerName │ Address │ TreeId │ hex("Event".EventData.Status) │ count_star() │
│       varchar       │  varchar   │ varchar │ int64  │            varchar            │    int64     │
├─────────────────────┼────────────┼─────────┼────────┼───────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ \file01    │         │      0 │ C000020C                      │            3 │
│ rd01.shieldbase.com │ \file01    │         │      0 │ C0000203                      │            6 │
└─────────────────────┴────────────┴─────────┴────────┴───────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey
         FROM read_json_objects('.\SmbClient%4Connectivity-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '30805' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┐
│              fullkey               │
│              varchar               │
├────────────────────────────────────┤
│ $.Event.EventData.Address          │
│ $.Event.EventData.AddressLength    │
│ $.Event.EventData.ServerName       │
│ $.Event.EventData.ServerNameLength │
│ $.Event.EventData.SessionId        │
│ $.Event.EventData.Status           │
│ $.Event.EventData.TreeId           │
└────────────────────────────────────┘
```



## 30806 (SMB Client: The client re-established its session to the server)
```sql
memory D SELECT Event.System.Computer, Event.EventData.ServerName,
             (SELECT string_agg(CAST(TRY_CAST('0x' || substr(ip_hex, i, 2) AS INTEGER) AS VARCHAR), '.' ORDER BY i)
              FROM (SELECT substr(Event.EventData.Address, 9, 8) AS ip_hex) AS a, range(1, 8, 2) AS r(i))
             || ':' ||
             CAST(TRY_CAST('0x' || substr(Event.EventData.Address, 5, 4) AS INTEGER) AS VARCHAR) AS IP_Port,
             Event.EventData.TreeId,
             hex(Event.EventData.Status),
             COUNT(*)
         FROM read_json('.\SmbClient%4Connectivity-rd01.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 30806
         GROUP BY ALL
         ORDER BY Event.EventData.ServerName;
┌─────────────────────┬────────────┬────────────────┬────────┬───────────────────────────────┬──────────────┐
│      Computer       │ ServerName │    IP_Port     │ TreeId │ hex("Event".EventData.Status) │ count_star() │
│       varchar       │  varchar   │    varchar     │ int64  │            varchar            │    int64     │
├─────────────────────┼────────────┼────────────────┼────────┼───────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ \file01    │ 172.16.4.5:445 │      0 │ 0                             │            7 │
└─────────────────────┴────────────┴────────────────┴────────┴───────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey
         FROM read_json_objects('.\SmbClient%4Connectivity-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '30806' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┐
│              fullkey               │
│              varchar               │
├────────────────────────────────────┤
│ $.Event.EventData.Address          │
│ $.Event.EventData.AddressLength    │
│ $.Event.EventData.ServerName       │
│ $.Event.EventData.ServerNameLength │
│ $.Event.EventData.SessionId        │
│ $.Event.EventData.Status           │
│ $.Event.EventData.TreeId           │
└────────────────────────────────────┘
```
