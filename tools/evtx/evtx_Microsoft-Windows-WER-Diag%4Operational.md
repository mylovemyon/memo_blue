```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f WER-Diag%4Operational.json C:\Windows\System32\winevt\Logs\Microsoft-Windows-WER-Diag%4Operational.evtx
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
         FROM read_json('.\WER-Diag%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│       2 │         1195 │
│       3 │            1 │
│       4 │            4 │
│       5 │            1 │
└─────────┴──────────────┘
```



## 4 (Application Crash on Launch)
```sql
memory D SELECT Event.System.Computer, Event.EventData.ModuleName, to_timestamp(CAST(Event.EventData.StartTime AS DOUBLE) / 10000000　- 11644473600) AS StartTime, Event.EventData.CrashTimeFromStart, COUNT(*)
         FROM read_json('.\WER-Diag%4Operational.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 4
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬───────────────────────────────────┬───────────────────────────────┬────────────────────┬──────────────┐
│      Computer       │            ModuleName             │           StartTime           │ CrashTimeFromStart │ count_star() │
│       varchar       │              varchar              │   timestamp with time zone    │       int64        │    int64     │
├─────────────────────┼───────────────────────────────────┼───────────────────────────────┼────────────────────┼──────────────┤
│ rd05.shieldbase.com │ C:\Windows\System32\InputHost.dll │ 2023-01-27 21:45:13.044224+00 │           22260856 │            1 │
│ rd05.shieldbase.com │ C:\Windows\System32\ucrtbase.dll  │ 2023-01-26 22:03:30.948227+00 │           14672823 │            1 │
│ rd05.shieldbase.com │ c:\windows\system32\diagtrack.dll │ 2023-01-28 00:39:16.594019+00 │           39630541 │            1 │
│ rd05.shieldbase.com │ c:\windows\system32\diagtrack.dll │ 2023-01-28 00:40:00.469761+00 │           39824192 │            1 │
└─────────────────────┴───────────────────────────────────┴───────────────────────────────┴────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\WER-Diag%4Operational.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '4' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────────────────┬──────────────┐
│               fullkey                │ count_star() │
│               varchar                │    int64     │
├──────────────────────────────────────┼──────────────┤
│ $.Event.EventData.#attributes        │            4 │
│ $.Event.EventData.#attributes.Name   │            4 │
│ $.Event.EventData.CrashTimeFromStart │            4 │
│ $.Event.EventData.ModuleName         │            4 │
│ $.Event.EventData.ProcessId          │            4 │
│ $.Event.EventData.StartTime          │            4 │
└──────────────────────────────────────┴──────────────┘
```
