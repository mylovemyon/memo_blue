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
  12 rows      2 columns
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
