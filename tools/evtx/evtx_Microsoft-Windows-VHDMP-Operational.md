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



## 1 (A VHD has been created)
```sql
memory D SELECT Event.System.Computer, Event.EventData.VhdFileName, Event.EventData.VirtualDisk, Event.EventData.VhdDiskNumber, COUNT(*)
         FROM read_json('.\VHDMP-Operational.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 1
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬───────────────────────────────────────────────────────────────┬────────────────────┬───────────────┬──────────────┐
│      Computer       │                          VhdFileName                          │    VirtualDisk     │ VhdDiskNumber │ count_star() │
│       varchar       │                            varchar                            │      varchar       │     int64     │    int64     │
├─────────────────────┼───────────────────────────────────────────────────────────────┼────────────────────┼───────────────┼──────────────┤
│ rd08.shieldbase.com │ C:\Users\slevine\Downloads\RSVP.iso                           │ 0xffffd60dc72a9480 │             0 │            1 │
│ rd09.shieldbase.com │ C:\Users\srogers\Downloads\2023-GDPR_Acknowledgement_Form.iso │ 0xffff8b86393ef040 │             0 │            1 │
└─────────────────────┴───────────────────────────────────────────────────────────────┴────────────────────┴───────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\VHDMP-Operational.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '1' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────┬──────────────┐
│             fullkey             │ count_star() │
│             varchar             │    int64     │
├─────────────────────────────────┼──────────────┤
│ $.Event.EventData.VhdDiskNumber │            3 │
│ $.Event.EventData.VhdFileName   │            3 │
│ $.Event.EventData.VirtualDisk   │            3 │
└─────────────────────────────────┴──────────────┘
```



## 2 (A VHD has been removed)
```sql
memory D SELECT Event.System.Computer, Event.EventData.VhdFileName, Event.EventData.VirtualDisk, Event.EventData.VhdDiskNumber, COUNT(*)
         FROM read_json('.\VHDMP-Operational.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.EventID = 2
         GROUP BY ALL
         ORDER BY ALL;
┌───────────────────────────┬─────────────────────────────────┬────────────────────┬───────────────┬──────────────┐
│         Computer          │           VhdFileName           │    VirtualDisk     │ VhdDiskNumber │ count_star() │
│          varchar          │             varchar             │      varchar       │     int64     │    int64     │
├───────────────────────────┼─────────────────────────────────┼────────────────────┼───────────────┼──────────────┤
│ exchange01.shieldbase.com │ C:\terraform\ExchangeServer.ISO │ 0xffffd106755ac4c0 │             0 │            1 │
└───────────────────────────┴─────────────────────────────────┴────────────────────┴───────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\VHDMP-Operational.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '2' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────┬──────────────┐
│             fullkey             │ count_star() │
│             varchar             │    int64     │
├─────────────────────────────────┼──────────────┤
│ $.Event.EventData.VhdDiskNumber │            1 │
│ $.Event.EventData.VhdFileName   │            1 │
│ $.Event.EventData.VirtualDisk   │            1 │
└─────────────────────────────────┴──────────────┘
```



## 50 (Performing Create VHD)



## 51 (Successfully performed Create VHD)
