```powershell
PS C:\Users\SANSDFIR>  .\evtx_dump-v0.12.3.exe -o jsonl -f Windows_Defender%4Operational.json "C:\Windows\System32\winevt\logs\Microsoft-Windows-Windows Defender%4Operational.evtx"
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



## 1000 (An antimalware scan started)
```sql
Windows_Defender%4Operational D
SELECT Event.System.Computer, Event.EventData.SID, Event.EventData.Domain, Event.EventData.User, Event.EventData."Product Name", Event.EventData."Product Version", Event.EventData."Scan ID", Event.EventData."Scan Type", Event.EventData."Scan Parameters", COUNT(*)
FROM read_json('C:/Users/SANSDFIR/Windows_Defender%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 1000
GROUP BY ALL
ORDER BY Event.EventData.SID, Event.EventData."Scan ID";
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



## 1001 (An antimalware scan finished)
```sql
Windows_Defender%4Operational D
SELECT Event.System.Computer, Event.EventData.SID, Event.EventData.Domain, Event.EventData.User, Event.EventData."Product Name", Event.EventData."Product Version", Event.EventData."Scan ID", Event.EventData."Scan Type", Event.EventData."Scan Parameters", Event.EventData."Scan Time Hours", Event.EventData."Scan Time Minutes", Event.EventData."Scan Time Seconds", COUNT(*)
FROM read_json('C:/Users/SANSDFIR/Windows_Defender%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 1001
GROUP BY ALL
ORDER BY Event.EventData.SID, Event.EventData."Scan ID";
┌─────────────────────┬──────────┬──────────────┬─────────┬──────────────────────────────┬─────────────────┬────────────────────────────────────────┬─────────────┬─────────────────┬─────────────────┬───────────────────┬───────────────────┬──────────────┐
│      Computer       │   SID    │    Domain    │  User   │         Product Name         │ Product Version │                Scan ID                 │  Scan Type  │ Scan Parameters │ Scan Time Hours │ Scan Time Minutes │ Scan Time Seconds │ count_star() │
│       varchar       │ varchar  │   varchar    │ varchar │           varchar            │     varchar     │                varchar                 │   varchar   │     varchar     │     varchar     │      varchar      │      varchar      │    int64     │
├─────────────────────┼──────────┼──────────────┼─────────┼──────────────────────────────┼─────────────────┼────────────────────────────────────────┼─────────────┼─────────────────┼─────────────────┼───────────────────┼───────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {0838659A-223E-45A2-A021-6DC749148C9F} │ Antimalware │ Quick Scan      │ 0               │ 12                │ 24                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {097F0F26-6AC5-476E-8229-6C4DCBA6B5F5} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 44                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {1C33F0DF-0FA4-4E44-88A6-CB534B821311} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 13                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {3F39B53A-586A-4454-81B1-0D18DD9BE4B7} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 19                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {52F63D00-7FD4-44B0-B809-DADBC308D0B0} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 44                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {655519B9-80AF-4BF9-AEDD-1695ECB86395} │ Antimalware │ Quick Scan      │ 0               │ 11                │ 34                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {68824E31-8887-4CC2-89E4-ABCDAC492380} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 53                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {7038655C-54BC-4B9D-9A02-D4F495BEA3EC} │ Antimalware │ Quick Scan      │ 0               │ 11                │ 27                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {76DC8A73-6F2F-4B4E-86D0-AF009AAFDC31} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 51                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {783676CE-B429-4B4D-9F0D-CB06EF6E3287} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 52                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {824BC624-2C80-48A6-9A40-AE15DBB3A8A1} │ Antimalware │ Quick Scan      │ 0               │ 12                │ 04                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {C06166F4-1A57-4203-91DF-094B3B58B81A} │ Antimalware │ Quick Scan      │ 0               │ 11                │ 07                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {C4125B09-111A-4D4C-9E15-FA408A92ED15} │ Antimalware │ Quick Scan      │ 0               │ 11                │ 51                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {C70EBB08-255E-4ECC-BED4-032E52CB7CB6} │ Antimalware │ Quick Scan      │ 0               │ 11                │ 16                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {CE5B8F58-F89E-41B9-8548-7CF7C30CF940} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 46                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {F1737A87-66DB-4A40-B238-18771E7E0A0A} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 44                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {F8BEFB91-A3D4-4BE4-8E1B-56F6AD17DFEF} │ Antimalware │ Quick Scan      │ 0               │ 11                │ 32                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {FE02A5CB-2350-40FA-80A4-2DE4F19C037B} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 33                │            1 │
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {FF2C952D-3405-42F3-82C0-724950952719} │ Antimalware │ Quick Scan      │ 0               │ 10                │ 50                │            1 │
└─────────────────────┴──────────┴──────────────┴─────────┴──────────────────────────────┴─────────────────┴────────────────────────────────────────┴─────────────┴─────────────────┴─────────────────┴───────────────────┴───────────────────┴──────────────┘
  19 rows                                                                                                                                                                                                                                         13 columns
```
fullkey
```
Windows_Defender%4Operational D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/Windows_Defender%4Operational.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '1001' AND je.fullkey LIKE '$.Event.EventData.%'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌─────────────────────────────────────────┬──────────────┐
│                 fullkey                 │ count_star() │
│                 varchar                 │    int64     │
├─────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.Domain                │           19 │
│ $.Event.EventData.Product Name          │           19 │
│ $.Event.EventData.Product Version       │           19 │
│ $.Event.EventData.SID                   │           19 │
│ $.Event.EventData.Scan ID               │           19 │
│ $.Event.EventData.Scan Parameters       │           19 │
│ $.Event.EventData.Scan Parameters Index │           19 │
│ $.Event.EventData.Scan Time Hours       │           19 │
│ $.Event.EventData.Scan Time Minutes     │           19 │
│ $.Event.EventData.Scan Time Seconds     │           19 │
│ $.Event.EventData.Scan Type             │           19 │
│ $.Event.EventData.Scan Type Index       │           19 │
│ $.Event.EventData.User                  │           19 │
└─────────────────────────────────────────┴──────────────┘
  13 rows                                      2 columns
```



## 1002 (Warning - An antimalware scan was stopped before it finished)
```sql
Windows_Defender%4Operational D
SELECT Event.System.Computer, Event.EventData.SID, Event.EventData.Domain, Event.EventData.User, Event.EventData."Product Name", Event.EventData."Product Version", Event.EventData."Scan ID", Event.EventData."Scan Type", Event.EventData."Scan Parameters", COUNT(*)
FROM read_json('C:/Users/SANSDFIR/Windows_Defender%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 1002
GROUP BY ALL
ORDER BY Event.EventData.SID, Event.EventData."Scan ID";
┌─────────────────────┬──────────┬──────────────┬─────────┬──────────────────────────────┬─────────────────┬────────────────────────────────────────┬─────────────┬─────────────────┬──────────────┐
│      Computer       │   SID    │    Domain    │  User   │         Product Name         │ Product Version │                Scan ID                 │  Scan Type  │ Scan Parameters │ count_star() │
│       varchar       │ varchar  │   varchar    │ varchar │           varchar            │     varchar     │                varchar                 │   varchar   │     varchar     │    int64     │
├─────────────────────┼──────────┼──────────────┼─────────┼──────────────────────────────┼─────────────────┼────────────────────────────────────────┼─────────────┼─────────────────┼──────────────┤
│ rd01.shieldbase.com │ S-1-5-18 │ NT AUTHORITY │ SYSTEM  │ Microsoft Defender Antivirus │ 4.18.2211.5     │ {9FCBAD54-7389-4DDA-96BF-F1068D16B6A5} │ Antimalware │ Quick Scan      │            1 │
└─────────────────────┴──────────┴──────────────┴─────────┴──────────────────────────────┴─────────────────┴────────────────────────────────────────┴─────────────┴─────────────────┴──────────────┘
```
fullkey
```sql
Windows_Defender%4Operational D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/Windows_Defender%4Operational.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '1002' AND je.fullkey LIKE '$.Event.EventData.%'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌─────────────────────────────────────────┬──────────────┐
│                 fullkey                 │ count_star() │
│                 varchar                 │    int64     │
├─────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.Domain                │            1 │
│ $.Event.EventData.Product Name          │            1 │
│ $.Event.EventData.Product Version       │            1 │
│ $.Event.EventData.SID                   │            1 │
│ $.Event.EventData.Scan ID               │            1 │
│ $.Event.EventData.Scan Parameters       │            1 │
│ $.Event.EventData.Scan Parameters Index │            1 │
│ $.Event.EventData.Scan Type             │            1 │
│ $.Event.EventData.Scan Type Index       │            1 │
│ $.Event.EventData.User                  │            1 │
└─────────────────────────────────────────┴──────────────┘
  10 rows                                      2 columns
```



## 1116 (Detection - The antimalware platform detected malware or other potentially unwanted software)
```sql
Windows_Defender%4Operational D
SELECT Event.System.Computer, Event.EventData."Product Name", Event.EventData."Product Version", Event.EventData."Detection User", Event.EventData."Process Name", Event.EventData.Path, Event.EventData."Threat Name", Event.EventData.FWLink, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/Windows_Defender%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 1116
GROUP BY ALL
ORDER BY Event.EventData."Process Name";
┌─────────────────────┬──────────────────────────────┬─────────────────┬─────────────────────┬──────────────────────────────────────────────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────────────────────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │         Product Name         │ Product Version │   Detection User    │                         Process Name                         │                                                                                                                                                                                                                                            Path                                                                                                                                                                                                                                             │            Threat Name            │                                                        FWLink                                                         │ count_star() │
│       varchar       │           varchar            │     varchar     │       varchar       │                           varchar                            │                                                                                                                                                                                                                                           varchar                                                                                                                                                                                                                                           │              varchar              │                                                        varchar                                                        │    int64     │
├─────────────────────┼──────────────────────────────┼─────────────────┼─────────────────────┼──────────────────────────────────────────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ shieldbase\tdungan  │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ amsi:_\Device\HarddiskVolume1\Program Files (x86)\Microsoft\Edge\Application\msedge.exe                                                                                                                                                                                                                                                                                                                                                                                                     │ Trojan:Win32/PowerRunner.A        │ https://go.microsoft.com/fwlink/?linkid=37020&name=Trojan:Win32/PowerRunner.A&threatid=2147808488&enterprise=0        │            3 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ shieldbase\tdungan  │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ amsi:_\Device\HarddiskVolume1\Program Files (x86)\Microsoft\Edge\Application\msedge.exe                                                                                                                                                                                                                                                                                                                                                                                                     │ VirTool:Win32/Kekeo.B             │ https://go.microsoft.com/fwlink/?linkid=37020&name=VirTool:Win32/Kekeo.B&threatid=2147805713&enterprise=0             │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ NT AUTHORITY\SYSTEM │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ amsi:_\Device\HarddiskVolume1\Program Files (x86)\Microsoft\Edge\Application\msedge.exe                                                                                                                                                                                                                                                                                                                                                                                                     │ VirTool:Win32/Kekeo.B             │ https://go.microsoft.com/fwlink/?linkid=37020&name=VirTool:Win32/Kekeo.B&threatid=2147805713&enterprise=0             │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │                     │ C:\Windows\System32\SRLUpdate.exe                            │ behavior:_process: C:\Windows\System32\SRLUpdate.exe, pid:6976:111820579542652; process:_pid:6976,ProcessStart:133190040928055801; process:_pid:8544,ProcessStart:133190040934814107                                                                                                                                                                                                                                                                                                        │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │                     │ C:\Windows\System32\svchost.exe                              │ behavior:_process: C:\Windows\System32\svchost.exe, pid:2376:111820579542652; process:_pid:2376,ProcessStart:133189590785418864                                                                                                                                                                                                                                                                                                                                                             │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │                     │ C:\Windows\explorer.exe                                      │ behavior:_process: C:\Windows\explorer.exe, pid:8544:111820579542652; process:_pid:10912,ProcessStart:133190439088345003; process:_pid:8544,ProcessStart:133190040934814107                                                                                                                                                                                                                                                                                                                 │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ NT AUTHORITY\SYSTEM │ Unknown                                                      │ behavior:_process: C:\Windows\System32\SRLUpdate.exe, pid:6976:111820579542652; file:_C:\Windows\System32\SRLUpdate.exe; file:_C:\Windows\System32\Tasks\SRL User Maintenance->(UTF-16LE); regkey:_HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks\{F0E32B12-4364-4ED6-8BCC-ED667179F8E2}; regkey:_HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\SRL User Maintenance; taskscheduler:_C:\Windows\System32\Tasks\SRL User Maintenance │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │                     │ Unknown                                                      │ behavior:_process: C:\Windows\System32\svchost.exe, pid:2376:111820579542652; process:_pid:2376,ProcessStart:133189590785418864                                                                                                                                                                                                                                                                                                                                                             │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │                     │ Unknown                                                      │ behavior:_process: C:\Windows\explorer.exe, pid:8544:111820579542652; process:_pid:10912,ProcessStart:133190439088345003; process:_pid:8544,ProcessStart:133190040934814107                                                                                                                                                                                                                                                                                                                 │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ NT AUTHORITY\SYSTEM │ Unknown                                                      │ behavior:_process: C:\Windows\System32\SRLUpdate.exe, pid:6976:111820579542652; file:_C:\Windows\System32\SRLUpdate.exe                                                                                                                                                                                                                                                                                                                                                                     │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │            2 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │                     │ Unknown                                                      │ behavior:_process: C:\Windows\System32\SRLUpdate.exe, pid:6976:111820579542652; process:_pid:6976,ProcessStart:133190040928055801; process:_pid:8544,ProcessStart:133190040934814107                                                                                                                                                                                                                                                                                                        │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │            2 │
└─────────────────────┴──────────────────────────────┴─────────────────┴─────────────────────┴──────────────────────────────────────────────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────────────────────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
Windows_Defender%4Operational D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/Windows_Defender%4Operational.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '1116' AND je.fullkey LIKE '$.Event.EventData.%'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌─────────────────────────────────────────────────┬──────────────┐
│                     fullkey                     │ count_star() │
│                     varchar                     │    int64     │
├─────────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.Action ID                     │           15 │
│ $.Event.EventData.Action Name                   │           15 │
│ $.Event.EventData.Additional Actions ID         │           15 │
│ $.Event.EventData.Additional Actions String     │           15 │
│ $.Event.EventData.Category ID                   │           15 │
│ $.Event.EventData.Category Name                 │           15 │
│ $.Event.EventData.Detection ID                  │           15 │
│ $.Event.EventData.Detection Time                │           15 │
│ $.Event.EventData.Detection User                │           15 │
│ $.Event.EventData.Engine Version                │           15 │
│ $.Event.EventData.Error Code                    │           15 │
│ $.Event.EventData.Error Description             │           15 │
│ $.Event.EventData.Execution ID                  │           15 │
│ $.Event.EventData.Execution Name                │           15 │
│ $.Event.EventData.FWLink                        │           15 │
│ $.Event.EventData.Origin ID                     │           15 │
│ $.Event.EventData.Origin Name                   │           15 │
│ $.Event.EventData.Path                          │           15 │
│ $.Event.EventData.Post Clean Status             │           15 │
│ $.Event.EventData.Pre Execution Status          │           15 │
│ $.Event.EventData.Process Name                  │           15 │
│ $.Event.EventData.Product Name                  │           15 │
│ $.Event.EventData.Product Version               │           15 │
│ $.Event.EventData.Remediation User              │           15 │
│ $.Event.EventData.Security intelligence Version │           15 │
│ $.Event.EventData.Severity ID                   │           15 │
│ $.Event.EventData.Severity Name                 │           15 │
│ $.Event.EventData.Source ID                     │           15 │
│ $.Event.EventData.Source Name                   │           15 │
│ $.Event.EventData.State                         │           15 │
│ $.Event.EventData.Status Code                   │           15 │
│ $.Event.EventData.Status Description            │           15 │
│ $.Event.EventData.Threat ID                     │           15 │
│ $.Event.EventData.Threat Name                   │           15 │
│ $.Event.EventData.Type ID                       │           15 │
│ $.Event.EventData.Type Name                     │           15 │
│ $.Event.EventData.Unused                        │           15 │
│ $.Event.EventData.Unused2                       │           15 │
│ $.Event.EventData.Unused3                       │           15 │
│ $.Event.EventData.Unused4                       │           15 │
│ $.Event.EventData.Unused5                       │           15 │
│ $.Event.EventData.Unused6                       │           15 │
└─────────────────────────────────────────────────┴──────────────┘
  42 rows                                              2 columns
```



## 1117 (Detection - The antimalware platform performed an action to protect your system from malware or other potentially unwanted software)
```sql
Windows_Defender%4Operational D
SELECT Event.System.Computer, Event.EventData."Product Name", Event.EventData."Product Version", Event.EventData."Detection User", Event.EventData."Process Name", Event.EventData.Path, Event.EventData."Threat Name", Event.EventData.FWLink, Event.EventData."Remediation User", Event.EventData."Action Name", COUNT(*)
FROM read_json('C:/Users/SANSDFIR/Windows_Defender%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 1117
GROUP BY ALL
ORDER BY Event.EventData."Process Name";
┌─────────────────────┬──────────────────────────────┬─────────────────┬─────────────────────┬──────────────────────────────────────────────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────────────────────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬─────────────────────┬─────────────┬──────────────┐
│      Computer       │         Product Name         │ Product Version │   Detection User    │                         Process Name                         │                                                                                                                                                                                                                                            Path                                                                                                                                                                                                                                             │            Threat Name            │                                                        FWLink                                                         │  Remediation User   │ Action Name │ count_star() │
│       varchar       │           varchar            │     varchar     │       varchar       │                           varchar                            │                                                                                                                                                                                                                                           varchar                                                                                                                                                                                                                                           │              varchar              │                                                        varchar                                                        │       varchar       │   varchar   │    int64     │
├─────────────────────┼──────────────────────────────┼─────────────────┼─────────────────────┼──────────────────────────────────────────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼─────────────────────┼─────────────┼──────────────┤
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ shieldbase\tdungan  │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ amsi:_\Device\HarddiskVolume1\Program Files (x86)\Microsoft\Edge\Application\msedge.exe                                                                                                                                                                                                                                                                                                                                                                                                     │ VirTool:Win32/Kekeo.B             │ https://go.microsoft.com/fwlink/?linkid=37020&name=VirTool:Win32/Kekeo.B&threatid=2147805713&enterprise=0             │ NT AUTHORITY\SYSTEM │ Quarantine  │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ shieldbase\tdungan  │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ amsi:_\Device\HarddiskVolume1\Program Files (x86)\Microsoft\Edge\Application\msedge.exe                                                                                                                                                                                                                                                                                                                                                                                                     │ Trojan:Win32/PowerRunner.A        │ https://go.microsoft.com/fwlink/?linkid=37020&name=Trojan:Win32/PowerRunner.A&threatid=2147808488&enterprise=0        │ NT AUTHORITY\SYSTEM │ Quarantine  │            2 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ NT AUTHORITY\SYSTEM │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ amsi:_\Device\HarddiskVolume1\Program Files (x86)\Microsoft\Edge\Application\msedge.exe                                                                                                                                                                                                                                                                                                                                                                                                     │ VirTool:Win32/Kekeo.B             │ https://go.microsoft.com/fwlink/?linkid=37020&name=VirTool:Win32/Kekeo.B&threatid=2147805713&enterprise=0             │ NT AUTHORITY\SYSTEM │ Quarantine  │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │                     │ C:\Windows\System32\SRLUpdate.exe                            │ behavior:_process: C:\Windows\System32\SRLUpdate.exe, pid:6976:111820579542652; process:_pid:6976,ProcessStart:133190040928055801; process:_pid:8544,ProcessStart:133190040934814107                                                                                                                                                                                                                                                                                                        │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │ NT AUTHORITY\SYSTEM │ Remove      │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │                     │ C:\Windows\System32\svchost.exe                              │ behavior:_process: C:\Windows\System32\svchost.exe, pid:2376:111820579542652; process:_pid:2376,ProcessStart:133189590785418864                                                                                                                                                                                                                                                                                                                                                             │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │ NT AUTHORITY\SYSTEM │ Remove      │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │                     │ C:\Windows\explorer.exe                                      │ behavior:_process: C:\Windows\explorer.exe, pid:8544:111820579542652; process:_pid:10912,ProcessStart:133190439088345003; process:_pid:8544,ProcessStart:133190040934814107                                                                                                                                                                                                                                                                                                                 │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │ NT AUTHORITY\SYSTEM │ Remove      │            1 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ NT AUTHORITY\SYSTEM │ Unknown                                                      │ behavior:_process: C:\Windows\System32\SRLUpdate.exe, pid:6976:111820579542652; file:_C:\Windows\System32\SRLUpdate.exe; file:_C:\Windows\System32\Tasks\SRL User Maintenance->(UTF-16LE); regkey:_HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks\{F0E32B12-4364-4ED6-8BCC-ED667179F8E2}; regkey:_HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\SRL User Maintenance; taskscheduler:_C:\Windows\System32\Tasks\SRL User Maintenance │ Behavior:Win32/CobaltStrike.E!sms │ https://go.microsoft.com/fwlink/?linkid=37020&name=Behavior:Win32/CobaltStrike.E!sms&threatid=2147776941&enterprise=0 │ NT AUTHORITY\SYSTEM │ Quarantine  │            1 │
└─────────────────────┴──────────────────────────────┴─────────────────┴─────────────────────┴──────────────────────────────────────────────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────────────────────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴─────────────────────┴─────────────┴──────────────┘
```
fullkey
```sql
Windows_Defender%4Operational D
SELECT je.fullkey, COUNT(*)
FROM read_json_objects('C:/Users/SANSDFIR/Windows_Defender%4Operational.json') AS e, json_tree(e.json) AS je
WHERE e.json->>'$.Event.System.EventID' = '1117' AND je.fullkey LIKE '$.Event.EventData.%'
GROUP BY je.fullkey
ORDER BY je.fullkey;
┌─────────────────────────────────────────────────┬──────────────┐
│                     fullkey                     │ count_star() │
│                     varchar                     │    int64     │
├─────────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.Action ID                     │            8 │
│ $.Event.EventData.Action Name                   │            8 │
│ $.Event.EventData.Additional Actions ID         │            8 │
│ $.Event.EventData.Additional Actions String     │            8 │
│ $.Event.EventData.Category ID                   │            8 │
│ $.Event.EventData.Category Name                 │            8 │
│ $.Event.EventData.Detection ID                  │            8 │
│ $.Event.EventData.Detection Time                │            8 │
│ $.Event.EventData.Detection User                │            8 │
│ $.Event.EventData.Engine Version                │            8 │
│ $.Event.EventData.Error Code                    │            8 │
│ $.Event.EventData.Error Description             │            8 │
│ $.Event.EventData.Execution ID                  │            8 │
│ $.Event.EventData.Execution Name                │            8 │
│ $.Event.EventData.FWLink                        │            8 │
│ $.Event.EventData.Origin ID                     │            8 │
│ $.Event.EventData.Origin Name                   │            8 │
│ $.Event.EventData.Path                          │            8 │
│ $.Event.EventData.Post Clean Status             │            8 │
│ $.Event.EventData.Pre Execution Status          │            8 │
│ $.Event.EventData.Process Name                  │            8 │
│ $.Event.EventData.Product Name                  │            8 │
│ $.Event.EventData.Product Version               │            8 │
│ $.Event.EventData.Remediation User              │            8 │
│ $.Event.EventData.Security intelligence Version │            8 │
│ $.Event.EventData.Severity ID                   │            8 │
│ $.Event.EventData.Severity Name                 │            8 │
│ $.Event.EventData.Source ID                     │            8 │
│ $.Event.EventData.Source Name                   │            8 │
│ $.Event.EventData.State                         │            8 │
│ $.Event.EventData.Status Code                   │            8 │
│ $.Event.EventData.Status Description            │            8 │
│ $.Event.EventData.Threat ID                     │            8 │
│ $.Event.EventData.Threat Name                   │            8 │
│ $.Event.EventData.Type ID                       │            8 │
│ $.Event.EventData.Type Name                     │            8 │
│ $.Event.EventData.Unused                        │            8 │
│ $.Event.EventData.Unused2                       │            8 │
│ $.Event.EventData.Unused3                       │            8 │
│ $.Event.EventData.Unused4                       │            8 │
│ $.Event.EventData.Unused5                       │            8 │
│ $.Event.EventData.Unused6                       │            8 │
└─────────────────────────────────────────────────┴──────────────┘
  42 rows                                              2 columns
```



## 5007 (Real-time Protection was disabled)
```sql
Windows_Defender%4Operational D
SELECT Event.System.Computer, Event.EventData."Product Name", Event.EventData."Product Version", Event.EventData."Old Value", Event.EventData."New Value", COUNT(*)
FROM read_json('C:/Users/SANSDFIR/Windows_Defender%4Operational.json', sample_size = -1, map_inference_threshold = -1, ignore_errors = true)
WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime <  TIMESTAMP '2023-02-01 00:00:00'
AND Event.System.EventID = 5007
GROUP BY ALL
ORDER BY ALL;
┌─────────────────────┬──────────────────────────────┬─────────────────┬──────────────────────────────────────────────────────────────────────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │         Product Name         │ Product Version │                                      Old Value                                       │                                                New Value                                                │ count_star() │
│       varchar       │           varchar            │     varchar     │                                       varchar                                        │                                                 varchar                                                 │    int64     │
├─────────────────────┼──────────────────────────────┼─────────────────┼──────────────────────────────────────────────────────────────────────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ Default\IsServiceRunning = 0x0                                                       │ HKLM\SOFTWARE\Microsoft\Windows Defender\IsServiceRunning = 0x1                                         │            3 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ Default\ProductAppDataPath = C:\ProgramData\Microsoft\Windows Defender               │ HKLM\SOFTWARE\Microsoft\Windows Defender\ProductAppDataPath = C:\ProgramData\Microsoft\Windows Defender │            3 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ Default\ServiceStartStates = 0x0                                                     │ HKLM\SOFTWARE\Microsoft\Windows Defender\ServiceStartStates = 0x1                                       │            3 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ HKLM\SOFTWARE\Microsoft\Windows Defender\ServiceStartStates = 0x1                    │ Default\ServiceStartStates = 0x0                                                                        │            3 │
│ rd01.shieldbase.com │ Microsoft Defender Antivirus │ 4.18.2211.5     │ HKLM\SOFTWARE\Policies\Microsoft\Windows Defender\DisableRoutinelyTakingAction = 0x1 │ Default\DisableRoutinelyTakingAction = 0x0                                                              │            3 │
└─────────────────────┴──────────────────────────────┴─────────────────┴──────────────────────────────────────────────────────────────────────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
Windows_Defender%4Operational D SELECT je.fullkey, COUNT(*)
                                FROM read_json_objects('C:/Users/SANSDFIR/Windows_Defender%4Operational.json') AS e, json_tree(e.json) AS je
                                WHERE e.json->>'$.Event.System.EventID' = '5007' AND je.fullkey LIKE '$.Event.EventData.%'
                                GROUP BY je.fullkey
                                ORDER BY je.fullkey;
┌───────────────────────────────────┬──────────────┐
│              fullkey              │ count_star() │
│              varchar              │    int64     │
├───────────────────────────────────┼──────────────┤
│ $.Event.EventData.New Value       │           15 │
│ $.Event.EventData.Old Value       │           15 │
│ $.Event.EventData.Product Name    │           15 │
│ $.Event.EventData.Product Version │           15 │
└───────────────────────────────────┴──────────────┘
```
