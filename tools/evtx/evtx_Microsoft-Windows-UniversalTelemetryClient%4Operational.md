```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f UniversalTelemetryClient%4Operational.json C:\Windows\System32\winevt\Logs\Microsoft-Windows-UniversalTelemetryClient%4Operational.evtx
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
         FROM read_json('.\UniversalTelemetryClient%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│       1 │         8774 │
│      29 │        27363 │
│      50 │           76 │
│      55 │           99 │
│      56 │          134 │
│      61 │          344 │
│      62 │           76 │
└─────────┴──────────────┘
```



## 55 (Internet connectivity status)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Environment, Event.EventData.State, COUNT(*)
         FROM read_json('.\UniversalTelemetryClient%4Operational.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 55
         GROUP BY ALL
         ORDER BY ALL;
┌────────────────────────┬─────────────┬─────────┬──────────────┐
│        Computer        │ Environment │  State  │ count_star() │
│        varchar         │   varchar   │ boolean │    int64     │
├────────────────────────┼─────────────┼─────────┼──────────────┤
│ admin01.shieldbase.com │ ServiceHost │ true    │            1 │
│ hunt01.shieldbase.com  │ ServiceHost │ true    │            3 │
│ rd01.shieldbase.com    │ ServiceHost │ true    │            4 │
│ rd02.shieldbase.com    │ ServiceHost │ true    │            3 │
│ rd03.shieldbase.com    │ ServiceHost │ true    │            3 │
│ rd04.shieldbase.com    │ ServiceHost │ true    │            2 │
│ rd05.shieldbase.com    │ ServiceHost │ true    │            4 │
│ rd06.shieldbase.com    │ ServiceHost │ true    │            3 │
│ rd07.shieldbase.com    │ ServiceHost │ true    │            2 │
│ rd08.shieldbase.com    │ ServiceHost │ false   │            1 │
│ rd08.shieldbase.com    │ ServiceHost │ true    │            2 │
│ rd09.shieldbase.com    │ ServiceHost │ false   │            1 │
│ rd09.shieldbase.com    │ ServiceHost │ true    │           15 │
│ rd10.shieldbase.com    │ ServiceHost │ false   │            1 │
│ rd10.shieldbase.com    │ ServiceHost │ true    │            4 │
│ wkstn01.shieldbase.com │ ServiceHost │ true    │            2 │
│ wkstn02.shieldbase.com │ ServiceHost │ true    │            3 │
│ wkstn03.shieldbase.com │ ServiceHost │ false   │            3 │
│ wkstn03.shieldbase.com │ ServiceHost │ true    │            6 │
│ wkstn04.shieldbase.com │ ServiceHost │ true    │            3 │
│ wkstn05.shieldbase.com │ ServiceHost │ true    │            3 │
│ wkstn06.shieldbase.com │ ServiceHost │ true    │            2 │
│ wkstn07.shieldbase.com │ ServiceHost │ true    │            2 │
│ wkstn08.shieldbase.com │ ServiceHost │ false   │            2 │
│ wkstn08.shieldbase.com │ ServiceHost │ true    │            6 │
│ wkstn09.shieldbase.com │ ServiceHost │ false   │            1 │
│ wkstn09.shieldbase.com │ ServiceHost │ true    │            5 │
│ wkstn10.shieldbase.com │ ServiceHost │ false   │            3 │
│ wkstn10.shieldbase.com │ ServiceHost │ true    │            9 │
└────────────────────────┴─────────────┴─────────┴──────────────┘
  29 rows                                             4 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\UniversalTelemetryClient%4Operational.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '55' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────┬──────────────┐
│            fullkey            │ count_star() │
│            varchar            │    int64     │
├───────────────────────────────┼──────────────┤
│ $.Event.EventData.Environment │          104 │
│ $.Event.EventData.State       │          104 │
└───────────────────────────────┴──────────────┘
```
