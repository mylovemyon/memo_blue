```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f StorageSpaces-Driver%4Operational.json C:\Windows\System32\winevt\logs\Microsoft-Windows-StorageSpaces-Driver%4Operational.evtx
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
         FROM read_json('.\StorageSpaces-Driver%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│     207 │          104 │
└─────────┴──────────────┘
```



## 207 (USB Connection)
```sql
memory D SELECT Event.System.Computer, Event.EventData.DeviceNumber, Event.EventData.DriveId, Event.EventData.DriveSerial, Event.EventData.DriveManufacturer, Event.EventData.DriveModel, Event.EventData.PoolId, COUNT(*)
         FROM read_json('.\StorageSpaces-Driver%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 207
         GROUP BY ALL
         ORDER BY Event.EventData.DriveModel;
┌─────────────────────┬──────────────┬──────────────────────────────────────┬────────────────────────────────┬───────────────────┬────────────────────────────┬──────────────────────────────────────┬──────────────┐
│      Computer       │ DeviceNumber │               DriveId                │          DriveSerial           │ DriveManufacturer │         DriveModel         │                PoolId                │ count_star() │
│       varchar       │    int64     │                 uuid                 │            varchar             │      varchar      │          varchar           │                 uuid                 │    int64     │
├─────────────────────┼──────────────┼──────────────────────────────────────┼────────────────────────────────┼───────────────────┼────────────────────────────┼──────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │            0 │ f6431577-1333-df61-b32e-220bf776984f │ vol03d970c2168bf631f_00000001. │ NULL              │ Amazon Elastic Block Store │ 00000000-0000-0000-0000-000000000000 │            4 │
└─────────────────────┴──────────────┴──────────────────────────────────────┴────────────────────────────────┴───────────────────┴────────────────────────────┴──────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\StorageSpaces-Driver%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '207' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────────┬──────────────┐
│               fullkey               │ count_star() │
│               varchar               │    int64     │
├─────────────────────────────────────┼──────────────┤
│ $.Event.EventData.DeviceNumber      │           26 │
│ $.Event.EventData.DriveId           │           26 │
│ $.Event.EventData.DriveManufacturer │           26 │
│ $.Event.EventData.DriveModel        │           26 │
│ $.Event.EventData.DriveSerial       │           26 │
│ $.Event.EventData.PoolId            │           26 │
└─────────────────────────────────────┴──────────────┘
```
