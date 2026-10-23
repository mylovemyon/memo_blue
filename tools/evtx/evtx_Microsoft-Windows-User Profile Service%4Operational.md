```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f "User Profile Service%4Operational.json" "C:\Windows\System32\winevt\Logs\Microsoft-Windows-User Profile Service%4Operational.evtx"
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off"
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
memory D
```



##  EventID
```sql
memory D SELECT Event.System.EventID, COUNT(*)
         FROM read_json('.\User Profile Service%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│       1 │          137 │
│       2 │          137 │
│       3 │          148 │
│       4 │          148 │
│       5 │         1994 │
│      59 │          322 │
│      67 │          997 │
└─────────┴──────────────┘
```



## 2 (An account was logged on)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Session, COUNT(*)
         FROM read_json('.\User Profile Service%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 2
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────┬──────────────┐
│      Computer       │ Session │ count_star() │
│       varchar       │  int64  │    int64     │
├─────────────────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │       2 │            4 │
│ rd01.shieldbase.com │       3 │            2 │
│ rd01.shieldbase.com │       4 │            2 │
│ rd01.shieldbase.com │       5 │            1 │
│ rd01.shieldbase.com │       6 │            1 │
│ rd01.shieldbase.com │       7 │            1 │
│ rd01.shieldbase.com │       8 │            1 │
│ rd01.shieldbase.com │       9 │            1 │
│ rd01.shieldbase.com │      10 │            1 │
│ rd01.shieldbase.com │      11 │            2 │
│ rd01.shieldbase.com │      12 │            1 │
│ rd01.shieldbase.com │      13 │            1 │
│ rd01.shieldbase.com │      14 │            1 │
└─────────────────────┴─────────┴──────────────┘
  13 rows                            3 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\User Profile Service%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '2' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────┬──────────────┐
│          fullkey          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ $.Event.EventData.Session │           46 │
└───────────────────────────┴──────────────┘
```



## 4 (An account was logged off)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Session, COUNT(*)
         FROM read_json('.\User Profile Service%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 4
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────┬──────────────┐
│      Computer       │ Session │ count_star() │
│       varchar       │  int64  │    int64     │
├─────────────────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │       2 │            3 │
│ rd01.shieldbase.com │       3 │            2 │
│ rd01.shieldbase.com │       4 │            2 │
│ rd01.shieldbase.com │       5 │            1 │
│ rd01.shieldbase.com │       6 │            1 │
│ rd01.shieldbase.com │       7 │            1 │
│ rd01.shieldbase.com │       8 │            1 │
│ rd01.shieldbase.com │       9 │            1 │
│ rd01.shieldbase.com │      10 │            1 │
│ rd01.shieldbase.com │      11 │            2 │
│ rd01.shieldbase.com │      12 │            1 │
│ rd01.shieldbase.com │      13 │            1 │
│ rd01.shieldbase.com │      14 │            1 │
└─────────────────────┴─────────┴──────────────┘
  13 rows                            3 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\User Profile Service%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '4' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────┬──────────────┐
│          fullkey          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ $.Event.EventData.Session │           45 │
└───────────────────────────┴──────────────┘
```



## 67 (Account Information)
```sql
memory D SELECT Event.System.Computer, Event.EventData.ProfileType, Event.EventData.LogonType, Event.EventData.LocalPath, COUNT(*)
         FROM read_json('.\User Profile Service%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 67
         GROUP BY ALL
         ORDER BY Event.EventData.LocalPath;
┌─────────────────────┬─────────────┬───────────┬───────────────────┬──────────────┐
│      Computer       │ ProfileType │ LogonType │     LocalPath     │ count_star() │
│       varchar       │   varchar   │  varchar  │      varchar      │    int64     │
├─────────────────────┼─────────────┼───────────┼───────────────────┼──────────────┤
│ rd01.shieldbase.com │ Regular     │ Regular   │ C:\Users\rsydow-a │           29 │
│ rd01.shieldbase.com │ Regular     │ Regular   │ C:\Users\tdungan  │           17 │
│ rd01.shieldbase.com │ Regular     │ Regular   │ C:\Users\wacsvc   │            2 │
└─────────────────────┴─────────────┴───────────┴───────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\User Profile Service%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '67' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────┬──────────────┐
│            fullkey            │ count_star() │
│            varchar            │    int64     │
├───────────────────────────────┼──────────────┤
│ $.Event.EventData.LocalPath   │          105 │
│ $.Event.EventData.LogonType   │          105 │
│ $.Event.EventData.ProfileType │          105 │
└───────────────────────────────┴──────────────┘
```
