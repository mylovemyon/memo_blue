```powershell
PS C:\Users\SANSDFIR>  .\evtx_dump-v0.12.3.exe -o jsonl -f Windows_Defender%4Operational.json "E:\C\Windows\System32\winevt\logs\Microsoft-Windows-Windows Defender%4Operational.evtx"
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off" .\Windows_Defender%4Operational.json
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
Windows_Defender%4Operational D
```

## .databases
```sql
Windows_Defender%4Operational D
.databases
┌────────────────────────────────────────┐
│               databases                │
│                                        │
│ Windows_Defender%4Operational (memory) │
└────────────────────────────────────────┘
```

## .table
```sql
Windows_Defender%4Operational D
.tables
 ────── Windows_Defender%4Operational ───────
 ─────────────────── main ───────────────────
┌───────────────────────────────┐┌───────────┐
│ Windows_Defender%4Operational ││   file    │
│                               ││           │
│ NULL                     null ││ NULL null │
└───────────────────────────────┘└───────────┘
```

## DESCRIBE
```sql
Windows_Defender%4Operational D
DESCRIBE;
┌───────────────────────────────┬─────────┬───────────────────────────────┬──────────────┬──────────────┬───────────┐
│           database            │ schema  │             name              │ column_names │ column_types │ temporary │
│            varchar            │ varchar │            varchar            │  varchar[]   │  varchar[]   │  boolean  │
├───────────────────────────────┼─────────┼───────────────────────────────┼──────────────┼──────────────┼───────────┤
│ Windows_Defender%4Operational │ main    │ Windows_Defender%4Operational │ [NULL]       │ [NULL]       │ false     │
│ Windows_Defender%4Operational │ main    │ file                          │ [NULL]       │ [NULL]       │ false     │
└───────────────────────────────┴─────────┴───────────────────────────────┴──────────────┴──────────────┴───────────┘
```


## EventID
```sql
Windows_Defender%4Operational D
SELECT Event.System.Computer, Event.System.EventID, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/Windows_Defender%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
GROUP BY ALL
ORDER BY ALL;
┌─────────────────────┬─────────┬──────────────┐
│      Computer       │ EventID │ count_star() │
│       varchar       │  int64  │    int64     │
├─────────────────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │    1000 │           20 │
│ rd01.shieldbase.com │    1001 │           19 │
│ rd01.shieldbase.com │    1002 │            1 │
│ rd01.shieldbase.com │    1013 │           17 │
│ rd01.shieldbase.com │    1116 │           15 │
│ rd01.shieldbase.com │    1117 │            8 │
│ rd01.shieldbase.com │    1150 │            1 │
│ rd01.shieldbase.com │    1151 │          466 │
│ rd01.shieldbase.com │    2001 │          188 │
│ rd01.shieldbase.com │    5000 │            3 │
│ rd01.shieldbase.com │    5007 │           15 │
│ rd01.shieldbase.com │    5013 │            2 │
└─────────────────────┴─────────┴──────────────┘
  12 rows                            3 columns
```



## 1000
```sql
Windows_Defender%4Operational D
SELECT Event.System.Computer, Event.EventData.SID, Event.EventData.Domain, Event.EventData.User, Event.EventData."Product Name", Event.EventData."Product Version", Event.EventData."Scan ID", Event.EventData."Scan Type", Event.EventData."Scan Parameters", COUNT(*)
FROM read_json('C:/Users/SANSDFIR/Windows_Defender%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 1000
GROUP BY ALL
ORDER BY ALL;
┌─────────────────────┬──────────┬──────────────┬─────────┬──────────────────────────────┬─────────────────┬────────────────────────────────────────┬─────────────┬─────────────────┬──────────────┐
│      Computer       │   SID    │    Domain    │  User   │         Product Name         │ Product Version │                Scan ID                 │  Scan Type  │ Scan Parameters │ count_star() │
│       varchar       │ varchar  │   varchar    │ varchar │           varchar            │     varchar     │                varchar                 │   varchar   │     varchar     │    int64     │
├─────────────────────┼──────────┼──────────────┼─────────┼──────────────────────────────┼─────────────────┼────────────────────────────────────────┼─────────────┼─────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {0838659A-223E-45A2-A021-6DC749148C9F} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {097F0F26-6AC5-476E-8229-6C4DCBA6B5F5} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {1C33F0DF-0FA4-4E44-88A6-CB534B821311} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {3F39B53A-586A-4454-81B1-0D18DD9BE4B7} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {52F63D00-7FD4-44B0-B809-DADBC308D0B0} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {655519B9-80AF-4BF9-AEDD-1695ECB86395} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {68824E31-8887-4CC2-89E4-ABCDAC492380} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {7038655C-54BC-4B9D-9A02-D4F495BEA3EC} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {76DC8A73-6F2F-4B4E-86D0-AF009AAFDC31} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {783676CE-B429-4B4D-9F0D-CB06EF6E3287} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {824BC624-2C80-48A6-9A40-AE15DBB3A8A1} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {9FCBAD54-7389-4DDA-96BF-F1068D16B6A5} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {C06166F4-1A57-4203-91DF-094B3B58B81A} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {C4125B09-111A-4D4C-9E15-FA408A92ED15} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {C70EBB08-255E-4ECC-BED4-032E52CB7CB6} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {CE5B8F58-F89E-41B9-8548-7CF7C30CF940} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {F1737A87-66DB-4A40-B238-18771E7E0A0A} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {F8BEFB91-A3D4-4BE4-8E1B-56F6AD17DFEF} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {FE02A5CB-2350-40FA-80A4-2DE4F19C037B} │ Antimalware │ Quick Scan      │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {FF2C952D-3405-42F3-82C0-724950952719} │ Antimalware │ Quick Scan      │            1 │
└─────────────────────┴──────────┴──────────────┴─────────┴──────────────────────────────┴─────────────────┴────────────────────────────────────────┴─────────────┴─────────────────┴──────────────┘
  20 rows                                                                                                                                                                               10 columns
```
fullkey
```sql
Windows_Defender%4Operational D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/Windows_Defender%4Operational.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '1000' AND je.fullkey LIKE '$.Event.EventData.%'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌─────────────────────────────────────────┬──────────────┐
│                 fullkey                 │ count_star() │
│                 varchar                 │    int64     │
├─────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.Domain                │           20 │
│ $.Event.EventData.Product Name          │           20 │
│ $.Event.EventData.Product Version       │           20 │
│ $.Event.EventData.SID                   │           20 │
│ $.Event.EventData.Scan ID               │           20 │
│ $.Event.EventData.Scan Parameters       │           20 │
│ $.Event.EventData.Scan Parameters Index │           20 │
│ $.Event.EventData.Scan Resources        │           20 │
│ $.Event.EventData.Scan Type             │           20 │
│ $.Event.EventData.Scan Type Index       │           20 │
│ $.Event.EventData.User                  │           20 │
└─────────────────────────────────────────┴──────────────┘
  11 rows                                      2 columns
  ```
