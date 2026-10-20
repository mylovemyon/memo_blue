```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f WMI-Activity%4Operational.json C:\Windows\System32\winevt\Logs\Microsoft-Windows-WMI-Activity%4Operational.evtx
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
         FROM read_json('.\WMI-Activity%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│    5857 │        80435 │
│    5858 │        48269 │
│    5859 │           96 │
│    5860 │          529 │
│    5861 │           99 │
└─────────┴──────────────┘
```



## 5857 (WMI wmiprvse execution)
```sql
memory D SELECT Event.System.Computer, Event.UserData.Operation_StartedOperational.HostProcess, Event.UserData.Operation_StartedOperational.ProviderName, Event.UserData.Operation_StartedOperational.ProviderPath, Event.UserData.Operation_StartedOperational.Code, COUNT(*)
         FROM read_json('.\WMI-Activity%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 5857
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬──────────────┬───────────────────────────────────┬─────────────────────────────────────────────┬─────────┬──────────────┐
│      Computer       │ HostProcess  │           ProviderName            │                ProviderPath                 │  Code   │ count_star() │
│       varchar       │   varchar    │              varchar              │                   varchar                   │ varchar │    int64     │
├─────────────────────┼──────────────┼───────────────────────────────────┼─────────────────────────────────────────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │ wmiprvse.exe │ CIMWin32                          │ %systemroot%\system32\wbem\cimwin32.dll     │ 0x0     │            4 │
│ rd01.shieldbase.com │ wmiprvse.exe │ CIMWin32a                         │ %systemroot%\system32\wbem\wmipcima.dll     │ 0x0     │          225 │
│ rd01.shieldbase.com │ wmiprvse.exe │ DeliveryOptimizationMIProv        │ %SystemRoot%\system32\domgmt.dll            │ 0x0     │           25 │
│ rd01.shieldbase.com │ wmiprvse.exe │ MSVDS__PROVIDER                   │ %systemroot%\system32\wbem\vdswmi.dll       │ 0x0     │            1 │
│ rd01.shieldbase.com │ wmiprvse.exe │ MSVSS__PROVIDER                   │ %systemroot%\system32\wbem\vsswmi.dll       │ 0x0     │           10 │
│ rd01.shieldbase.com │ wmiprvse.exe │ MS_NT_EVENTLOG_PROVIDER           │ %systemroot%\system32\wbem\ntevt.dll        │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ PowerWmiProvider                  │ %systemroot%\system32\umpowmi.dll           │ 0x0     │            1 │
│ rd01.shieldbase.com │ wmiprvse.exe │ RegProv                           │ %systemroot%\system32\wbem\stdprov.dll      │ 0x0     │            1 │
│ rd01.shieldbase.com │ wmiprvse.exe │ RouteProvider                     │ %systemroot%\system32\wbem\wmipiprt.dll     │ 0x0     │            4 │
│ rd01.shieldbase.com │ wmiprvse.exe │ StorageWMI                        │ %SystemRoot%\System32\storagewmi.dll        │ 0x0     │            1 │
│ rd01.shieldbase.com │ wmiprvse.exe │ UserProfileProvider               │ %systemroot%\system32\profprov.dll          │ 0x0     │           25 │
│ rd01.shieldbase.com │ wmiprvse.exe │ WMI Kernel Trace Event Provider   │ C:\Windows\System32\wbem\krnlprov.dll       │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ WMIProv                           │ %systemroot%\system32\wbem\wmiprov.dll      │ 0x0     │           30 │
│ rd01.shieldbase.com │ wmiprvse.exe │ WhqlProvider                      │ %systemroot%\system32\signdrv.dll           │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ Win32_DeviceGuard                 │ %SystemRoot%\System32\Win32_DeviceGuard.dll │ 0x0     │           25 │
│ rd01.shieldbase.com │ wmiprvse.exe │ Win32_TpmProvider                 │ C:\Windows\System32\wbem\Win32_TPM.dll      │ 0x0     │           25 │
│ rd01.shieldbase.com │ wmiprvse.exe │ Win32_WIN32_TERMINALSERVICE_Prov  │ %SystemRoot%\system32\tscfgwmi.dll          │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ Win32_WIN32_TSGENERALSETTING_Prov │ %SystemRoot%\system32\tscfgwmi.dll          │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ WmiPerfClass                      │ C:\Windows\System32\wbem\WmiPerfClass.dll   │ 0x0     │           29 │
│ rd01.shieldbase.com │ wmiprvse.exe │ smbwmiv2                          │ %SystemRoot%\System32\smbwmiv2.dll          │ 0x0     │            1 │
└─────────────────────┴──────────────┴───────────────────────────────────┴─────────────────────────────────────────────┴─────────┴──────────────┘
  20 rows                                                                                                                             6 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\WMI-Activity%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '5857' AND je.fullkey LIKE '$.Event.UserData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────────────────────────────────────┬──────────────┐
│                             fullkey                             │ count_star() │
│                             varchar                             │    int64     │
├─────────────────────────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.Operation_StartedOperational                   │         2718 │
│ $.Event.UserData.Operation_StartedOperational.#attributes       │         2718 │
│ $.Event.UserData.Operation_StartedOperational.#attributes.xmlns │         2718 │
│ $.Event.UserData.Operation_StartedOperational.Code              │         2718 │
│ $.Event.UserData.Operation_StartedOperational.HostProcess       │         2718 │
│ $.Event.UserData.Operation_StartedOperational.ProcessID         │         2718 │
│ $.Event.UserData.Operation_StartedOperational.ProviderName      │         2718 │
│ $.Event.UserData.Operation_StartedOperational.ProviderPath      │         2718 │
└─────────────────────────────────────────────────────────────────┴──────────────┘
```
