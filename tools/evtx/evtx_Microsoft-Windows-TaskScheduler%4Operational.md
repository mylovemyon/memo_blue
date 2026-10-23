```powershell
PS C:\Users\SANSDFIR>  .\evtx_dump-v0.12.3.exe -o jsonl -f TaskScheduler%4Operational.json C:\Windows\System32\winevt\logs\Microsoft-Windows-TaskScheduler%4Operational.evtx
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
         FROM read_json('.\TaskScheduler%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│     100 │       133516 │
│     101 │          536 │
│     102 │       122467 │
│     103 │         9783 │
│     106 │          288 │
│     107 │        79686 │
│     108 │         6278 │
│     109 │         4552 │
│     110 │          551 │
│     111 │          118 │
│     114 │          280 │
│     118 │         2802 │
│     119 │         8533 │
│     129 │       122721 │
│     140 │       132282 │
│     141 │          273 │
│     142 │           99 │
│     200 │       133516 │
│     201 │       122467 │
│     202 │         9783 │
│     203 │          506 │
│     322 │         3445 │
│     324 │          760 │
│     325 │         4818 │
│     328 │          298 │
│     329 │          120 │
│     332 │         3145 │
│     400 │           95 │
│     402 │           93 │
│     700 │           95 │
└─────────┴──────────────┘
  30 rows      2 columns
```



## 100 (Scheduled Task started)
```sql
memory D SELECT Event.System.Computer, Event.EventData.UserContext, Event.EventData.TaskName, COUNT(*)
         FROM read_json('.\TaskScheduler%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 100 AND Event.EventData.TaskName LIkE '\SRL%'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────────────────┬───────────────────────┬──────────────┐
│      Computer       │     UserContext     │       TaskName        │ count_star() │
│       varchar       │       varchar       │        varchar        │    int64     │
├─────────────────────┼─────────────────────┼───────────────────────┼──────────────┤
│ rd01.shieldbase.com │ NT AUTHORITY\SYSTEM │ \SRL Update Service   │            4 │
│ rd01.shieldbase.com │ shieldbase\tdungan  │ \SRL User Maintenance │            4 │
└─────────────────────┴─────────────────────┴───────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\TaskScheduler%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '100' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.#attributes      │        13485 │
│ $.Event.EventData.#attributes.Name │        13485 │
│ $.Event.EventData.InstanceId       │        13485 │
│ $.Event.EventData.TaskName         │        13485 │
│ $.Event.EventData.UserContext      │        13485 │
└────────────────────────────────────┴──────────────┘
```



## 102 (Scheduled Task completed)
```sql
memory D SELECT Event.System.Computer, Event.EventData.UserContext, Event.EventData.TaskName, COUNT(*)
         FROM read_json('.\TaskScheduler%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 102 AND Event.EventData.TaskName LIkE '\SRL%'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────────────────┬───────────────────────┬──────────────┐
│      Computer       │     UserContext     │       TaskName        │ count_star() │
│       varchar       │       varchar       │        varchar        │    int64     │
├─────────────────────┼─────────────────────┼───────────────────────┼──────────────┤
│ rd01.shieldbase.com │ NT AUTHORITY\SYSTEM │ \SRL Update Service   │            3 │
│ rd01.shieldbase.com │ shieldbase\tdungan  │ \SRL User Maintenance │            4 │
└─────────────────────┴─────────────────────┴───────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\TaskScheduler%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '102' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.#attributes      │        12893 │
│ $.Event.EventData.#attributes.Name │        12893 │
│ $.Event.EventData.InstanceId       │        12893 │
│ $.Event.EventData.TaskName         │        12893 │
│ $.Event.EventData.UserContext      │        12893 │
└────────────────────────────────────┴──────────────┘
```



## 106 (Scheduled Task created)
```sql
memory D SELECT Event.System.Computer, Event.EventData.UserContext, Event.EventData.TaskName, COUNT(*)
         FROM read_json('.\TaskScheduler%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 106 AND Event.EventData.TaskName LIkE '\SRL%'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────┬───────────────────────┬──────────────┐
│      Computer       │    UserContext     │       TaskName        │ count_star() │
│       varchar       │      varchar       │        varchar        │    int64     │
├─────────────────────┼────────────────────┼───────────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18           │ \SRL Update Service   │            1 │
│ rd01.shieldbase.com │ shieldbase\tdungan │ \SRL User Maintenance │            1 │
└─────────────────────┴────────────────────┴───────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\TaskScheduler%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '106' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.#attributes      │           37 │
│ $.Event.EventData.#attributes.Name │           37 │
│ $.Event.EventData.TaskName         │           37 │
│ $.Event.EventData.UserContext      │           37 │
└────────────────────────────────────┴──────────────┘
```



## 119 (Scheduled Task triggered on logon)
```sql
memory D SELECT Event.System.Computer, Event.EventData.UserName, Event.EventData.TaskName, COUNT(*)
         FROM read_json('.\TaskScheduler%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 119 AND Event.EventData.TaskName LIkE '\SRL%'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────┬───────────────────────┬──────────────┐
│      Computer       │      UserName      │       TaskName        │ count_star() │
│       varchar       │      varchar       │        varchar        │    int64     │
├─────────────────────┼────────────────────┼───────────────────────┼──────────────┤
│ rd01.shieldbase.com │ shieldbase\tdungan │ \SRL User Maintenance │            4 │
└─────────────────────┴────────────────────┴───────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\TaskScheduler%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '119' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.#attributes      │          917 │
│ $.Event.EventData.#attributes.Name │          917 │
│ $.Event.EventData.InstanceId       │          917 │
│ $.Event.EventData.TaskName         │          917 │
│ $.Event.EventData.UserName         │          917 │
└────────────────────────────────────┴──────────────┘
```



## 129 (Scheduled Task created)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Priority, Event.EventData.TaskName, Event.EventData.Path, COUNT(*)
         FROM read_json('.\TaskScheduler%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 129 AND Event.EventData.TaskName LIkE '\SRL%'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬──────────┬───────────────────────┬───────────────────────────────────┬──────────────┐
│      Computer       │ Priority │       TaskName        │               Path                │ count_star() │
│       varchar       │  int64   │        varchar        │              varchar              │    int64     │
├─────────────────────┼──────────┼───────────────────────┼───────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │    16384 │ \SRL Update Service   │ C:\Windows\System32\STUN.exe      │            4 │
│ rd01.shieldbase.com │    16384 │ \SRL User Maintenance │ C:\Windows\System32\SRLUpdate.exe │            4 │
└─────────────────────┴──────────┴───────────────────────┴───────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\TaskScheduler%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '129' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┬──────────────┐
│              fullkey               │ count_star() │
│              varchar               │    int64     │
├────────────────────────────────────┼──────────────┤
│ $.Event.EventData.#attributes      │        12691 │
│ $.Event.EventData.#attributes.Name │        12691 │
│ $.Event.EventData.Path             │        12691 │
│ $.Event.EventData.Priority         │        12691 │
│ $.Event.EventData.ProcessID        │        12691 │
│ $.Event.EventData.TaskName         │        12691 │
└────────────────────────────────────┴──────────────┘
```
