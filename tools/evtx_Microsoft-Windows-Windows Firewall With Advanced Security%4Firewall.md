```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f Windows Firewall With Advanced Security%4Firewall.json C:\Windows\System32\winevt\Logs\Microsoft-Windows-Windows Firewall With Advanced Security%4Firewall.evtx
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
         FROM read_json('.\Windows Firewall With Advanced Security%4Firewall.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│    2002 │           98 │
│    2004 │         4638 │
│    2005 │          824 │
│    2006 │         1786 │
│    2008 │          320 │
│    2010 │          110 │
│    2011 │            1 │
│    2033 │          356 │
│    2047 │           35 │
│    2051 │          356 │
└─────────┴──────────────┘
  10 rows      2 columns
```



## 2003 (The firewall has enabled/disabled)
```sql
memory D SELECT Event.System.Computer, Event.EventData.ModifyingUser, Event.EventData.ModifyingApplication, Event.EventData.Profiles, Event.EventData.Origin, Event.EventData.SettingType, Event.EventData.SettingValue, Event.EventData.SettingValueString, Event.EventData.SettingValueSize, COUNT(*)
         FROM read_json('.\Windows Firewall With Advanced Security%4Firewall-rd06.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.EventID = 2003
         GROUP BY ALL
         ORDER BY ALL;
┌──────────┬───────────────┬───────────────────────────────────────┬──────────┬────────┬─────────────┬──────────────┬────────────────────┬──────────────────┬──────────────┐
│ Computer │ ModifyingUser │         ModifyingApplication          │ Profiles │ Origin │ SettingType │ SettingValue │ SettingValueString │ SettingValueSize │ count_star() │
│ varchar  │    varchar    │                varchar                │  int64   │ int64  │    int64    │   varchar    │      varchar       │      int64       │    int64     │
├──────────┼───────────────┼───────────────────────────────────────┼──────────┼────────┼─────────────┼──────────────┼────────────────────┼──────────────────┼──────────────┤
│ rd06     │ S-1-5-18      │ C:\Windows\System32\wbem\WmiPrvSE.exe │        1 │      1 │           1 │ 00000000     │ No                 │                4 │            1 │
└──────────┴───────────────┴───────────────────────────────────────┴──────────┴────────┴─────────────┴──────────────┴────────────────────┴──────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Windows Firewall With Advanced Security%4Firewall-rd06.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '2003' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────────┬──────────────┐
│                fullkey                 │ count_star() │
│                varchar                 │    int64     │
├────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.ModifyingApplication │            1 │
│ $.Event.EventData.ModifyingUser        │            1 │
│ $.Event.EventData.Origin               │            1 │
│ $.Event.EventData.Profiles             │            1 │
│ $.Event.EventData.SettingType          │            1 │
│ $.Event.EventData.SettingValue         │            1 │
│ $.Event.EventData.SettingValueSize     │            1 │
│ $.Event.EventData.SettingValueString   │            1 │
└────────────────────────────────────────┴──────────────┘
```



## 20004 (A rule has been added to the Windows Defender Firewall exception list)
```sql

```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Windows Firewall With Advanced Security%4Firewall-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '2004' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────────────────────────────┬──────────────┐
│                     fullkey                      │ count_star() │
│                     varchar                      │    int64     │
├──────────────────────────────────────────────────┼──────────────┤
│ $.Event.EventData.Action                         │          622 │
│ $.Event.EventData.Active                         │          622 │
│ $.Event.EventData.ApplicationPath                │          622 │
│ $.Event.EventData.Direction                      │          622 │
│ $.Event.EventData.EdgeTraversal                  │          622 │
│ $.Event.EventData.EmbeddedContext                │          622 │
│ $.Event.EventData.Flags                          │          622 │
│ $.Event.EventData.LocalAddresses                 │          622 │
│ $.Event.EventData.LocalOnlyMapped                │          622 │
│ $.Event.EventData.LocalPorts                     │          622 │
│ $.Event.EventData.LooseSourceMapped              │          622 │
│ $.Event.EventData.ModifyingApplication           │          622 │
│ $.Event.EventData.ModifyingUser                  │          622 │
│ $.Event.EventData.Origin                         │          622 │
│ $.Event.EventData.Profiles                       │          622 │
│ $.Event.EventData.Protocol                       │          622 │
│ $.Event.EventData.RemoteAddresses                │          622 │
│ $.Event.EventData.RemoteMachineAuthorizationList │          622 │
│ $.Event.EventData.RemotePorts                    │          622 │
│ $.Event.EventData.RemoteUserAuthorizationList    │          622 │
│ $.Event.EventData.RuleId                         │          622 │
│ $.Event.EventData.RuleName                       │          622 │
│ $.Event.EventData.RuleStatus                     │          622 │
│ $.Event.EventData.SchemaVersion                  │          622 │
│ $.Event.EventData.SecurityOptions                │          622 │
│ $.Event.EventData.ServiceName                    │          622 │
└──────────────────────────────────────────────────┴──────────────┘
  26 rows                                               2 columns
```
