```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f "Winlogon%4Operational.json" "C:\Windows\System32\winevt\Logs\Microsoft-Windows-Winlogon%4Operational.evtx"
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
         FROM read_json('.\Winlogon%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│       1 │          213 │
│       2 │          213 │
│     505 │            1 │
│     811 │         4519 │
│     812 │         4519 │
└─────────┴──────────────┘
```



## 811 (User activity)
```sql
memory D SELECT Event.System.Computer, Event.EventData.SubscriberName, Event.EventData.Event,COUNT(*)
         FROM read_json('.\Winlogon%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 811
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────┬───────┬──────────────┐
│      Computer       │ SubscriberName │ Event │ count_star() │
│       varchar       │    varchar     │ int64 │    int64     │
├─────────────────────┼────────────────┼───────┼──────────────┤
│ rd01.shieldbase.com │ GPClient       │     0 │           40 │
│ rd01.shieldbase.com │ GPClient       │     2 │           19 │
│ rd01.shieldbase.com │ GPClient       │     3 │           18 │
│ rd01.shieldbase.com │ GPClient       │    12 │           19 │
│ rd01.shieldbase.com │ GPClient       │    13 │           34 │
│ rd01.shieldbase.com │ Profiles       │     2 │           19 │
│ rd01.shieldbase.com │ Profiles       │     3 │           18 │
│ rd01.shieldbase.com │ Sens           │     2 │           19 │
│ rd01.shieldbase.com │ Sens           │     3 │           18 │
│ rd01.shieldbase.com │ Sens           │     4 │           13 │
│ rd01.shieldbase.com │ Sens           │     5 │           12 │
│ rd01.shieldbase.com │ Sens           │     8 │           13 │
│ rd01.shieldbase.com │ Sens           │     9 │           12 │
│ rd01.shieldbase.com │ Sens           │    12 │           19 │
│ rd01.shieldbase.com │ SessionEnv     │     2 │           19 │
│ rd01.shieldbase.com │ SessionEnv     │     3 │           18 │
│ rd01.shieldbase.com │ SessionEnv     │     8 │           13 │
│ rd01.shieldbase.com │ SessionEnv     │     9 │           12 │
│ rd01.shieldbase.com │ SessionEnv     │    12 │           19 │
│ rd01.shieldbase.com │ TermSrv        │     0 │           40 │
│ rd01.shieldbase.com │ TermSrv        │     1 │           39 │
│ rd01.shieldbase.com │ TermSrv        │     2 │           19 │
│ rd01.shieldbase.com │ TermSrv        │     3 │           18 │
│ rd01.shieldbase.com │ TermSrv        │     4 │           13 │
│ rd01.shieldbase.com │ TermSrv        │     5 │           12 │
│ rd01.shieldbase.com │ TermSrv        │    12 │           19 │
│ rd01.shieldbase.com │ TermSrv        │    13 │           18 │
└─────────────────────┴────────────────┴───────┴──────────────┘
  27 rows                                           4 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Winlogon%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '811' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────────────┬──────────────┐
│             fullkey              │ count_star() │
│             varchar              │    int64     │
├──────────────────────────────────┼──────────────┤
│ $.Event.EventData.Event          │         1165 │
│ $.Event.EventData.SubscriberName │         1165 │
└──────────────────────────────────┴──────────────┘
```
Event
```
2: User logon
3: User logoff
4: System lock
5: System unlock
6: Screensaver start
7: Screensaver stop
8	セッション切断・休止状態への移行
9	切断されたセッションの再開
12	ログオン関連の通知
13	ログオフ関連の通知
```



## 812 (User activity)
```sql
memory D SELECT Event.System.Computer, Event.EventData.SubscriberName, Event.EventData.Event,COUNT(*)
         FROM read_json('.\Winlogon%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 812
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────┬───────┬──────────────┐
│      Computer       │ SubscriberName │ Event │ count_star() │
│       varchar       │    varchar     │ int64 │    int64     │
├─────────────────────┼────────────────┼───────┼──────────────┤
│ rd01.shieldbase.com │ GPClient       │     0 │           40 │
│ rd01.shieldbase.com │ GPClient       │     2 │           19 │
│ rd01.shieldbase.com │ GPClient       │     3 │           18 │
│ rd01.shieldbase.com │ GPClient       │    12 │           19 │
│ rd01.shieldbase.com │ GPClient       │    13 │           34 │
│ rd01.shieldbase.com │ Profiles       │     2 │           19 │
│ rd01.shieldbase.com │ Profiles       │     3 │           18 │
│ rd01.shieldbase.com │ Sens           │     2 │           19 │
│ rd01.shieldbase.com │ Sens           │     3 │           18 │
│ rd01.shieldbase.com │ Sens           │     4 │           13 │
│ rd01.shieldbase.com │ Sens           │     5 │           12 │
│ rd01.shieldbase.com │ Sens           │     8 │           13 │
│ rd01.shieldbase.com │ Sens           │     9 │           12 │
│ rd01.shieldbase.com │ Sens           │    12 │           19 │
│ rd01.shieldbase.com │ SessionEnv     │     2 │           19 │
│ rd01.shieldbase.com │ SessionEnv     │     3 │           18 │
│ rd01.shieldbase.com │ SessionEnv     │     8 │           13 │
│ rd01.shieldbase.com │ SessionEnv     │     9 │           12 │
│ rd01.shieldbase.com │ SessionEnv     │    12 │           19 │
│ rd01.shieldbase.com │ TermSrv        │     0 │           40 │
│ rd01.shieldbase.com │ TermSrv        │     1 │           39 │
│ rd01.shieldbase.com │ TermSrv        │     2 │           19 │
│ rd01.shieldbase.com │ TermSrv        │     3 │           18 │
│ rd01.shieldbase.com │ TermSrv        │     4 │           13 │
│ rd01.shieldbase.com │ TermSrv        │     5 │           12 │
│ rd01.shieldbase.com │ TermSrv        │    12 │           19 │
│ rd01.shieldbase.com │ TermSrv        │    13 │           18 │
└─────────────────────┴────────────────┴───────┴──────────────┘
  27 rows                                           4 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Winlogon%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '812' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────────────┬──────────────┐
│             fullkey              │ count_star() │
│             varchar              │    int64     │
├──────────────────────────────────┼──────────────┤
│ $.Event.EventData.Event          │         1166 │
│ $.Event.EventData.SubscriberName │         1166 │
└──────────────────────────────────┴──────────────┘
```
