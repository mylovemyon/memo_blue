```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f Storage-Storport%4Operational.json C:\Windows\System32\winevt\logs\Microsoft-Windows-Storage-Storport%4Operational.evtx
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
         FROM read_json('.\Storage-Storport%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│     504 │         4392 │
│     505 │        18562 │
│     524 │           93 │
│     549 │         4510 │
│     552 │          106 │
│     553 │          104 │
│     557 │           14 │
│     567 │           45 │
│     576 │           68 │
└─────────┴──────────────┘
```



## 504 (Error summary for Storport Device)
```sql
memory D SELECT Event.System.Computer, Event.EventData.LUN, Event.EventData.ProductId, Event.EventData.VendorId, Event.EventData.SerialNumber, Event.EventData.AdapterSerialNumber, Event.EventData.BusType, Event.EventData.PortNumber, Event.EventData.MiniportName,Event.EventData.LastError_Command, Event.EventData.LastError_SrbStatus, Event.EventData.LastError_ScsiStatus, Event.EventData.LastError_SenseKey, Event.EventData.LastError_AddSense, Event.EventData.LastError_AddSenseQ, COUNT(*)
         FROM read_json('.\Storage-Storport%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 504
         GROUP BY ALL
         ORDER BY Event.EventData.ProductId;
┌─────────────────────┬───────┬──────────────────┬──────────┬────────────────────────────────┬───────────────────────────┬─────────┬────────────┬──────────────┬───────────────────┬─────────────────────┬──────────────────────┬────────────────────┬────────────────────┬─────────────────────┬──────────────┐
│      Computer       │  LUN  │    ProductId     │ VendorId │          SerialNumber          │    AdapterSerialNumber    │ BusType │ PortNumber │ MiniportName │ LastError_Command │ LastError_SrbStatus │ LastError_ScsiStatus │ LastError_SenseKey │ LastError_AddSense │ LastError_AddSenseQ │ count_star() │
│       varchar       │ int64 │     varchar      │ varchar  │            varchar             │          varchar          │  int64  │   int64    │   varchar    │       int64       │        int64        │        int64         │       int64        │       int64        │        int64        │    int64     │
├─────────────────────┼───────┼──────────────────┼──────────┼────────────────────────────────┼───────────────────────────┼─────────┼────────────┼──────────────┼───────────────────┼─────────────────────┼──────────────────────┼────────────────────┼────────────────────┼─────────────────────┼──────────────┤
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │                42 │                   5 │                    0 │                  0 │                  0 │                   0 │            6 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │                40 │                   5 │                    0 │                  0 │                  0 │                   0 │          177 │
└─────────────────────┴───────┴──────────────────┴──────────┴────────────────────────────────┴───────────────────────────┴─────────┴────────────┴──────────────┴───────────────────┴─────────────────────┴──────────────────────┴────────────────────┴────────────────────┴─────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Storage-Storport%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '504' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────────────────────────┬──────────────┐
│                       fullkey                       │ count_star() │
│                       varchar                       │    int64     │
├─────────────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AdapterGuid                       │          757 │
│ $.Event.EventData.AdapterSerialNumber               │          757 │
│ $.Event.EventData.BusType                           │          757 │
│ $.Event.EventData.ClassDeviceGuid                   │          757 │
│ $.Event.EventData.HierarchicalResetSuccessCount     │          757 │
│ $.Event.EventData.LUN                               │          757 │
│ $.Event.EventData.LastError_AddSense                │          757 │
│ $.Event.EventData.LastError_AddSenseQ               │          757 │
│ $.Event.EventData.LastError_Command                 │          757 │
│ $.Event.EventData.LastError_IoSize                  │          757 │
│ $.Event.EventData.LastError_LBA                     │          757 │
│ $.Event.EventData.LastError_QueueDepth              │          757 │
│ $.Event.EventData.LastError_RequestDuration_ms      │          757 │
│ $.Event.EventData.LastError_ScsiStatus              │          757 │
│ $.Event.EventData.LastError_SenseKey                │          757 │
│ $.Event.EventData.LastError_SrbStatus               │          757 │
│ $.Event.EventData.LastError_WaitDuration_ms         │          757 │
│ $.Event.EventData.MiniportName                      │          757 │
│ $.Event.EventData.PathID                            │          757 │
│ $.Event.EventData.PortNumber                        │          757 │
│ $.Event.EventData.ProductId                         │          757 │
│ $.Event.EventData.SampledErrorLogArray              │          757 │
│ $.Event.EventData.SampledErrorLogArrayLength        │          757 │
│ $.Event.EventData.SerialNumber                      │          757 │
│ $.Event.EventData.SystemUptime_s                    │          757 │
│ $.Event.EventData.TargetID                          │          757 │
│ $.Event.EventData.TimeoutsInMiniport                │          757 │
│ $.Event.EventData.TotalDeviceFailureErrors          │          757 │
│ $.Event.EventData.TotalErrors                       │          757 │
│ $.Event.EventData.TotalImpendingDeviceFailureErrors │          757 │
│ $.Event.EventData.TotalReadWriteErrors              │          757 │
│ $.Event.EventData.UniqueErrorLogArray               │          757 │
│ $.Event.EventData.UniqueErrorLogArrayLength         │          757 │
│ $.Event.EventData.VendorId                          │          757 │
│ $.Event.EventData.Version                           │          757 │
└─────────────────────────────────────────────────────┴──────────────┘
  35 rows                                                  2 columns
```
BusType
```
0: The bus type is unknown.
1: SCSI
2: ATAPI
3: ATA
4: IEEE 1394
5: SSA
6: Fibre Channel
7: USB
8: RAID
9: iSCSI
10: Serial Attached SCSI (SAS)
11: Serial ATA (SATA)
12: Secure Digital (SD)
13: Multimedia Card (MMC)
14: This value is reserved for system use.
15: File-Backed Virtual
16: Storage Spaces
17: NVMe
18: This value is reserved for system use.
```
https://github.com/MicrosoftDocs/SupportArticles-docs/blob/main/support/windows-server/backup-and-storage/event-id-51-information.md#the-scsi-request-block-srb-parameters



## 505 (Performance summary for Storport Device)
```sql
memory D SELECT Event.System.Computer, Event.EventData.LUN, Event.EventData.ProductId, Event.EventData.VendorId, Event.EventData.SerialNumber, Event.EventData.AdapterSerialNumber,  Event.EventData.BusType, Event.EventData.PortNumber, Event.EventData.MiniportName,Event.EventData.BootDevice, Event.EventData.TotalReadBytes, Event.EventData.TotalWriteBytes, COUNT(*)
         FROM read_json('.\Storage-Storport%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 505
         GROUP BY ALL
         ORDER BY Event.EventData.ProductId
         LIMIT 10;
┌─────────────────────┬───────┬──────────────────┬──────────┬────────────────────────────────┬───────────────────────────┬─────────┬────────────┬──────────────┬────────────┬────────────────┬─────────────────┬──────────────┐
│      Computer       │  LUN  │    ProductId     │ VendorId │          SerialNumber          │    AdapterSerialNumber    │ BusType │ PortNumber │ MiniportName │ BootDevice │ TotalReadBytes │ TotalWriteBytes │ count_star() │
│       varchar       │ int64 │     varchar      │ varchar  │            varchar             │          varchar          │  int64  │   int64    │   varchar    │  boolean   │     int64      │      int64      │    int64     │
├─────────────────────┼───────┼──────────────────┼──────────┼────────────────────────────────┼───────────────────────────┼─────────┼────────────┼──────────────┼────────────┼────────────────┼─────────────────┼──────────────┤
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │        3112960 │        78878208 │            1 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │       12417536 │        50131456 │            1 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │        2824704 │        47547392 │            1 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │       12312576 │        51334144 │            1 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │     6641184768 │      1497723904 │            1 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │       18423296 │        68982784 │            1 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │        3386880 │        73186304 │            1 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │        6248448 │        46208512 │            1 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │         640000 │        57193472 │            1 │
│ rd01.shieldbase.com │     0 │ Amazon Elastic B │ NVMe     │ vol03d970c2168bf631f_00000001. │ vol03d970c2168bf631f_0000 │      17 │          0 │ stornvme     │ true       │       64496128 │        78620160 │            1 │
└─────────────────────┴───────┴──────────────────┴──────────┴────────────────────────────────┴───────────────────────────┴─────────┴────────────┴──────────────┴────────────┴────────────────┴─────────────────┴──────────────┘
  10 rows                                                                                                                                                                                                          13 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Storage-Storport%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '505' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────────────────────────────────┬──────────────┐
│                            fullkey                             │ count_star() │
│                            varchar                             │    int64     │
├────────────────────────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.AdapterGuid                                  │         2596 │
│ $.Event.EventData.AdapterSerialNumber                          │         2596 │
│ $.Event.EventData.BootDevice                                   │         2596 │
│ $.Event.EventData.BucketIoFailed1                              │         2596 │
│ $.Event.EventData.BucketIoFailed10                             │         2596 │
│ $.Event.EventData.BucketIoFailed11                             │         2596 │
│ $.Event.EventData.BucketIoFailed12                             │         2596 │
│ $.Event.EventData.BucketIoFailed2                              │         2596 │
│ $.Event.EventData.BucketIoFailed3                              │         2596 │
│ $.Event.EventData.BucketIoFailed4                              │         2596 │
│ $.Event.EventData.BucketIoFailed5                              │         2596 │
│ $.Event.EventData.BucketIoFailed6                              │         2596 │
│ $.Event.EventData.BucketIoFailed7                              │         2596 │
│ $.Event.EventData.BucketIoFailed8                              │         2596 │
│ $.Event.EventData.BucketIoFailed9                              │         2596 │
│ $.Event.EventData.BucketIoLatency10_100ns                      │         2596 │
│ $.Event.EventData.BucketIoLatency11_100ns                      │         2596 │
│ $.Event.EventData.BucketIoLatency12_100ns                      │         2596 │
│ $.Event.EventData.BucketIoLatency1_100ns                       │         2596 │
│ $.Event.EventData.BucketIoLatency2_100ns                       │         2596 │
│ $.Event.EventData.BucketIoLatency3_100ns                       │         2596 │
│ $.Event.EventData.BucketIoLatency4_100ns                       │         2596 │
│ $.Event.EventData.BucketIoLatency5_100ns                       │         2596 │
│ $.Event.EventData.BucketIoLatency6_100ns                       │         2596 │
│ $.Event.EventData.BucketIoLatency7_100ns                       │         2596 │
│ $.Event.EventData.BucketIoLatency8_100ns                       │         2596 │
│ $.Event.EventData.BucketIoLatency9_100ns                       │         2596 │
│ $.Event.EventData.BucketIoSuccess1                             │         2596 │
│ $.Event.EventData.BucketIoSuccess10                            │         2596 │
│ $.Event.EventData.BucketIoSuccess11                            │         2596 │
│ $.Event.EventData.BucketIoSuccess12                            │         2596 │
│ $.Event.EventData.BucketIoSuccess2                             │         2596 │
│ $.Event.EventData.BucketIoSuccess3                             │         2596 │
│ $.Event.EventData.BucketIoSuccess4                             │         2596 │
│ $.Event.EventData.BucketIoSuccess5                             │         2596 │
│ $.Event.EventData.BucketIoSuccess6                             │         2596 │
│ $.Event.EventData.BucketIoSuccess7                             │         2596 │
│ $.Event.EventData.BucketIoSuccess8                             │         2596 │
│ $.Event.EventData.BucketIoSuccess9                             │         2596 │
│ $.Event.EventData.BusType                                      │         2596 │
│ $.Event.EventData.ClassDeviceGuid                              │         2596 │
│ $.Event.EventData.DeviceQueueIoBusyCount                       │         2596 │
│ $.Event.EventData.DeviceQueueIoPausedCount                     │         2596 │
│ $.Event.EventData.DeviceQueueIoPausedForUntaggedCount          │         2596 │
│ $.Event.EventData.DeviceQueueIoUntaggedCommandOutstandingCount │         2596 │
│ $.Event.EventData.DeviceQueueIoWaitExceededTimeoutCount        │         2596 │
│ $.Event.EventData.FirmwareRevision                             │         2596 │
│ $.Event.EventData.HighLatencyIoCount                           │         2596 │
│ $.Event.EventData.IoLatencyBuckets                             │         2596 │
│ $.Event.EventData.IoTimeout_s                                  │         2596 │
│ $.Event.EventData.LUN                                          │         2596 │
│ $.Event.EventData.MaxDeviceQueueCount                          │         2596 │
│ $.Event.EventData.MaxDeviceQueueIoWaitDuration_100ns           │         2596 │
│ $.Event.EventData.MaxFlushLatency_100ns                        │         2596 │
│ $.Event.EventData.MaxOutstandingCount                          │         2596 │
│ $.Event.EventData.MaxReadWriteLatency_100ns                    │         2596 │
│ $.Event.EventData.MaxUnmapLatency_100ns                        │         2596 │
│ $.Event.EventData.MiniportName                                 │         2596 │
│ $.Event.EventData.PathID                                       │         2596 │
│ $.Event.EventData.PortNumber                                   │         2596 │
│ $.Event.EventData.ProductId                                    │         2596 │
│ $.Event.EventData.SerialNumber                                 │         2596 │
│ $.Event.EventData.SystemUptime_s                               │         2596 │
│ $.Event.EventData.TargetID                                     │         2596 │
│ $.Event.EventData.TotalDeviceQueueIoCount                      │         2596 │
│ $.Event.EventData.TotalDeviceQueueIoWaitDuration_100ns         │         2596 │
│ $.Event.EventData.TotalIoCount                                 │         2596 │
│ $.Event.EventData.TotalReadBytes                               │         2596 │
│ $.Event.EventData.TotalWriteBytes                              │         2596 │
│ $.Event.EventData.VendorId                                     │         2596 │
│ $.Event.EventData.Version                                      │         2596 │
└────────────────────────────────────────────────────────────────┴──────────────┘
  71 rows                                                             2 columns
```
