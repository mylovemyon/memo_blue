```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f WinINet-Config%4ProxyConfigChanged.json C:\Windows\System32\winevt\Logs\Microsoft-Windows-WinINet-Config%4ProxyConfigChanged.evtx
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
         FROM read_json('.\WinINet-Config%4ProxyConfigChanged.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│    5600 │           13 │
└─────────┴──────────────┘
```



## 5600 (Proxy configuration changed)
```sql
memory D SELECT Event.EventData.pwszProxy, Event.EventData.pwszProxyBypass, Event.EventData.fAutoDetect, Event.EventData.pwszAutoConfigUrl, COUNT(*)
         FROM read_json('.\WinINet-Config%4ProxyConfigChanged-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 5600
         GROUP BY ALL
         ORDER BY ALL;
┌──────────────────┬───────────────────────────┬─────────────┬───────────────────┬──────────────┐
│    pwszProxy     │      pwszProxyBypass      │ fAutoDetect │ pwszAutoConfigUrl │ count_star() │
│     varchar      │          varchar          │   boolean   │      varchar      │    int64     │
├──────────────────┼───────────────────────────┼─────────────┼───────────────────┼──────────────┤
│ 172.16.4.10:8080 │ 172.16.*;*.shieldbase.com │ false       │                   │            1 │
└──────────────────┴───────────────────────────┴─────────────┴───────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\WinINet-Config%4ProxyConfigChanged-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '5600' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.fAutoDetect       │            8 │
│ $.Event.EventData.pwszAutoConfigUrl │            8 │
│ $.Event.EventData.pwszProxy         │            8 │
│ $.Event.EventData.pwszProxyBypass   │            8 │
└─────────────────────────────────────┴──────────────┘
```
