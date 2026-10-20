```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f WinRM%4Operational.json C:\Windows\System32\winevt\Logs\Microsoft-Windows-WinRM%4Operational.evtx
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



## 6 (Creating WSMan Session)
```sql
memory D SELECT Event.EventData.connection, COUNT(*)
         FROM read_json('.\WinRM%4Operational-rd01.json', sample_size = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 6
         GROUP BY ALL
         ORDER BY ALL;
┌──────────────────────────────────────────────────────────┬──────────────┐
│                        connection                        │ count_star() │
│                         varchar                          │    int64     │
├──────────────────────────────────────────────────────────┼──────────────┤
│ dc01.shieldbase.com/wsman?PSVersion=5.1.22000.1335       │           16 │
│ dev01.shieldbase.com/wsman?PSVersion=5.1.22000.1335      │           16 │
│ exchange01.shieldbase.com/wsman?PSVersion=5.1.22000.1335 │           16 │
│ file01.shieldbase.com/wsman?PSVersion=5.1.22000.1335     │           16 │
│ proxy01.shieldbase.com/wsman?PSVersion=5.1.22000.1335    │           16 │
│ sql01.shieldbase.com/wsman?PSVersion=5.1.22000.1335      │           16 │
│ wac01.shieldbase.com/wsman?PSVersion=5.1.22000.1335      │           16 │
└──────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\WinRM%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '6' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────────┬──────────────┐
│           fullkey            │ count_star() │
│           varchar            │    int64     │
├──────────────────────────────┼──────────────┤
│ $.Event.EventData.connection │          112 │
└──────────────────────────────┴──────────────┘
```



## 91 (Creating WSMan shell on server with ResourceUri)
```sql
memory D SELECT Event.EventData.resourceUri, COUNT(*)
         FROM read_json('.\WinRM%4Operational-rd01.json', sample_size = -1, ignore_errors = true)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 91
         GROUP BY ALL
         ORDER BY ALL;
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│                                               resourceUri                                                │ count_star() │
│                                                 varchar                                                  │    int64     │
├──────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ http://schemas.microsoft.com/powershell/Microsoft.PowerShell (shieldbase\rsydow-a clientIP: 172.16.4.4)  │            5 │
│ http://schemas.microsoft.com/powershell/Microsoft.PowerShell (shieldbase\rsydow-a clientIP: 172.16.4.7)  │            8 │
│ http://schemas.microsoft.com/powershell/Microsoft.PowerShell (shieldbase\rsydow-a clientIP: 172.16.6.18) │           16 │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\WinRM%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '91' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────┬──────────────┐
│            fullkey            │ count_star() │
│            varchar            │    int64     │
├───────────────────────────────┼──────────────┤
│ $.Event.EventData.resourceUri │           29 │
└───────────────────────────────┴──────────────┘
```
