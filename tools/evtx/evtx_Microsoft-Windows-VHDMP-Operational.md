```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f VHDMP-Operational.json C:\Windows\System32\winevt\Logs\Microsoft-Windows-VHDMP-Operational.evtx
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off"
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
memory D
```



## Event ID
```sql
memory D SELECT Event.System.EventID, COUNT(*)
         FROM read_json('.\VHDMP-Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│       1 │            2 │
│      12 │            2 │
│      14 │            2 │
│      15 │            2 │
│      21 │            2 │
│      22 │            6 │
│      23 │            6 │
│      25 │            2 │
│      30 │            2 │
└─────────┴──────────────┘
```
