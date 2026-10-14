```powershell
PS C:\Users\SANSDFIR>  .\evtx_dump-v0.12.3.exe -o jsonl -f TerminalServices-RemoteConnectionManager%4Operational.json C:\Windows\System32\winevt\logs\Microsoft-Windows-TerminalServices-RemoteConnectionManager%4Operational.evtx
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off" .\TerminalServices-RemoteConnectionManager%4Operational.json
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
TerminalServices-RemoteConnectionManager... D
```

## .databaess
```sql
TerminalServices-RemoteConnectionManager... D .databases
┌────────────────────────────────────────────────────────────────┐
│                           databases                            │
│                                                                │
│ TerminalServices-RemoteConnectionManager%4Operational (memory) │
└────────────────────────────────────────────────────────────────┘
```

## .tables
```sql
TerminalServices-RemoteConnectionManager... D .tables
 ────── TerminalServices-RemoteConnectionManager%4Operational ───────
 ─────────────────────────────── main ───────────────────────────────
┌───────────┐┌───────────────────────────────────────────────────────┐
│   file    ││ TerminalServices-RemoteConnectionManager%4Operational │
│           ││                                                       │
│ NULL null ││ NULL                                             null │
└───────────┘└───────────────────────────────────────────────────────┘
```

## DESCRIBE
```sql
TerminalServices-RemoteConnectionManager... D DESCRIBE;
┌───────────────────────────────────────────────────────┬─────────┬───────────────────────────────────────────────────────┬──────────────┬──────────────┬───────────┐
│                       database                        │ schema  │                         name                          │ column_names │ column_types │ temporary │
│                        varchar                        │ varchar │                        varchar                        │  varchar[]   │  varchar[]   │  boolean  │
├───────────────────────────────────────────────────────┼─────────┼───────────────────────────────────────────────────────┼──────────────┼──────────────┼───────────┤
│ TerminalServices-RemoteConnectionManager%4Operational │ main    │ TerminalServices-RemoteConnectionManager%4Operational │ [NULL]       │ [NULL]       │ false     │
│ TerminalServices-RemoteConnectionManager%4Operational │ main    │ file                                                  │ [NULL]       │ [NULL]       │ false     │
└───────────────────────────────────────────────────────┴─────────┴───────────────────────────────────────────────────────┴──────────────┴──────────────┴───────────┘
```

## EventID
```sql
TerminalServices-RemoteConnectionManager... D SELECT Event.System.Computer, Event.System.EventID, COUNT(*)
                                              FROM read_json('C:/Users/SANSDFIR/TerminalServices-RemoteConnectionManager%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                                              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                                              GROUP BY ALL
                                              ORDER BY ALL;
┌─────────────────────┬─────────┬──────────────┐
│      Computer       │ EventID │ count_star() │
│       varchar       │  int64  │    int64     │
├─────────────────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │     258 │            8 │
│ rd01.shieldbase.com │     261 │           84 │
│ rd01.shieldbase.com │     263 │           27 │
│ rd01.shieldbase.com │    1136 │            4 │
│ rd01.shieldbase.com │    1149 │           24 │
│ rd01.shieldbase.com │   20523 │            8 │
└─────────────────────┴─────────┴──────────────┘
```



## 1149 (RDP network connection established)
```sql
TerminalServices-RemoteConnectionManager... D SELECT Event.UserData.EventXML.Param2, Event.UserData.EventXML.Param1, Event.UserData.EventXML.Param3, COUNT(*)
                                              FROM read_json('C:/Users/SANSDFIR/TerminalServices-RemoteConnectionManager%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
                                              WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
                                              AND Event.System.EventID = 1149
                                              GROUP BY ALL
                                              ORDER BY ALL;
┌────────────┬─────────┬──────────────┬──────────────┐
│   Param2   │ Param1  │    Param3    │ count_star() │
│  varchar   │ varchar │   varchar    │    int64     │
├────────────┼─────────┼──────────────┼──────────────┤
│ shieldbase │ tdungan │ 172.16.30.20 │            1 │
│ NULL       │ NULL    │ 172.16.30.14 │            3 │
│ NULL       │ NULL    │ 172.16.30.20 │            6 │
│ NULL       │ NULL    │ 172.16.30.23 │            3 │
│ NULL       │ NULL    │ 172.16.30.3  │            4 │
│ NULL       │ NULL    │ 172.16.30.8  │            7 │
└────────────┴─────────┴──────────────┴──────────────┘
```
fullkey
```sql
TerminalServices-RemoteConnectionManager... D SELECT je.fullkey, COUNT(*)
                                              FROM read_json_objects('C:/Users/SANSDFIR/TerminalServices-RemoteConnectionManager%4Operational.json') AS e, json_tree(e.json) AS je
                                              WHERE e.json->>'$.Event.System.EventID' = '1149' AND je.fullkey LIKE '$.Event.UserData.EventXML.%'
                                              GROUP BY je.fullkey
                                              ORDER BY je.fullkey;
┌─────────────────────────────────────────────┬──────────────┐
│                   fullkey                   │ count_star() │
│                   varchar                   │    int64     │
├─────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.EventXML.#attributes       │           81 │
│ $.Event.UserData.EventXML.#attributes.xmlns │           81 │
│ $.Event.UserData.EventXML.Param1            │           81 │
│ $.Event.UserData.EventXML.Param2            │           81 │
│ $.Event.UserData.EventXML.Param3            │           81 │
└─────────────────────────────────────────────┴──────────────┘
```
