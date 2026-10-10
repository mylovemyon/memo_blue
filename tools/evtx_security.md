https://github.com/omerbenamram/evtx

# command
## --help
```
Utility to parse EVTX files

Usage: evtx_dump-v0.12.3.exe [OPTIONS] [INPUT] [COMMAND]

Commands:
  extract-wevt-templates   Build a WEVT template cache from PE files (EXE/DLL)
  dump-template-instances  Dump BinXML TemplateInstance substitution arrays from EVTX records (JSONL)
  apply-wevt-cache         Render a WEVT template using an offline cache + substitution values
  help                     Print this message or the help of the given subcommand(s)

Arguments:
  [INPUT]
          Input EVTX file path, or '-' to read from stdin. Required unless using a subcommand.

Options:
  -t, --threads <num-threads>
          Sets the number of worker threads, defaults to number of CPU cores.

          [default: 0]

  -o, --format <output-format>
          Sets the output format:
          "xml"   - prints XML output.
          "json"  - prints JSON output.
          "jsonl" - (jsonlines) same as json with --no-indent --dont-show-record-number


          [default: xml]
          [possible values: json, xml, jsonl]

  -f, --output <output-target>
          Writes output to the file specified instead of stdout, errors will still be printed to stderr.
          Will ask for confirmation before overwriting files, to allow overwriting, pass `--no-confirm-overwrite`
          Will create parent directories if needed.

      --no-confirm-overwrite
          When set, will not ask for confirmation before overwriting files, useful for automation

      --events <event-ranges>
          When set, only the specified events (offseted reltaive to file) will be outputted.
          For example:
              --events=1 will output the first event.
              --events=0-10,20-30 will output events 0-10 and 20-30.


      --validate-checksums
          When set, chunks with invalid checksums will not be parsed. Usually dirty files have bad checksums, so using this flag will result in fewer records.

      --no-indent
          When set, output will not be indented.

      --separate-json-attributes
          If outputting JSON, XML Element's attributes will be stored in a separate object named '<ELEMENTNAME>_attributes', with <ELEMENTNAME> containing the value of the node.

      --dont-show-record-number
          When set, `Record <id>` will not be printed.

      --ansi-codec <ansi-codec>
          When set, controls the codec of ansi encoded strings the file.

          [default: windows-1252]
          [possible values: ascii, ibm866, iso-8859-1, iso-8859-2, iso-8859-3, iso-8859-4, iso-8859-5, iso-8859-6, iso-8859-7, iso-8859-8, iso-8859-10, iso-8859-13, iso-8859-14, iso-8859-15, iso-8859-16, koi8-r, koi8-u, mac-roman, windows-874, windows-1250, windows-1251, windows-1252, windows-1253, windows-1254, windows-1255, windows-1256, windows-1257, windows-1258, mac-cyrillic, utf-8, windows-949, euc-jp, windows-31j, gbk, gb18030, hz, big5-2003, pua-mapped-binary, iso-8859-8-i]

      --wevt-cache <WEVTCACHE>
          Path to a WEVT template cache file (`.wevtcache`). When set, evtx_dump will try to render records using this cache if the embedded EVTX template expansion fails.

      --stop-after-one-error
          When set, will exit after any failure of reading a record. Useful for debugging.

  -v...
          Sets debug prints level for the application:
              -v   - info
              -vv  - debug
              -vvv - trace
          NOTE: trace output is only available in debug builds, as it is extremely verbose.

  -h, --help
          Print help (see a summary with '-h')

  -V, --version
          Print version
```


## -o
```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f security.json E:\Windows\System32\winevt\Logs\Security.evtx
PS C:\Users\SANSDFIR>
```



# duckdb
## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off" .\security.json
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
security D
```

## .databases
```sql
security D .databases
┌───────────────────┐
│     databases     │
│                   │
│ security (memory) │
└───────────────────┘
```

## .tables
```sql
security D .tables
 ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── security ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── main ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                           file                                                                                                                           │
│                                                                                                                                                                                                                                                          │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid varchar)), eventid bigint, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated stru… │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                                                                                         security                                                                                                                         │
│                                                                                                                                                                                                                                                          │
│ Event struct("#attributes" struct(xmlns varchar), "system" struct(provider struct("#attributes" struct("name" varchar, guid varchar)), eventid bigint, "version" bigint, "level" bigint, task bigint, opcode bigint, keywords varchar, timecreated stru… │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

## DESCRIBE
```sql
security D DESCRIBE;
┌──────────┬─────────┬──────────┬──────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────┐
│ database │ schema  │   name   │ column_names │                                                                                         column_types                                                                                          │ temporary │
│ varchar  │ varchar │ varchar  │  varchar[]   │                                                                                           varchar[]                                                                                           │  boolean  │
├──────────┼─────────┼──────────┼──────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────┤
│ security │ main    │ file     │ [Event]      │ ['STRUCT("#attributes" STRUCT(xmlns VARCHAR), "System" STRUCT(Provider STRUCT("#attributes" STRUCT("Name" VARCHAR, Guid VARCHAR)), EventID BIGINT, "Version" BIGINT, "Level" BIGINT, Task BIG │ false     │
│          │         │          │              │ INT, Opcode BIGINT, Keywords VARCHAR, TimeCreated STRUCT("#attributes" STRUCT(SystemTime TIMESTAMP)), EventRecordID BIGINT, Correlation STRUCT("#attributes" STRUCT(ActivityID UUID)), Execut │           │
│          │         │          │              │ ion STRUCT("#attributes" STRUCT(ProcessID BIGINT, ThreadID BIGINT)), Channel VARCHAR, Computer VARCHAR, "Security" JSON), EventData MAP(VARCHAR, JSON), UserData STRUCT(ServiceShutdown STRUC │           │
│          │         │          │              │ T("#attributes" STRUCT(xmlns VARCHAR))))']                                                                                                                                                    │           │
├──────────┼─────────┼──────────┼──────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────┤
│ security │ main    │ security │ [Event]      │ ['STRUCT("#attributes" STRUCT(xmlns VARCHAR), "System" STRUCT(Provider STRUCT("#attributes" STRUCT("Name" VARCHAR, Guid VARCHAR)), EventID BIGINT, "Version" BIGINT, "Level" BIGINT, Task BIG │ false     │
│          │         │          │              │ INT, Opcode BIGINT, Keywords VARCHAR, TimeCreated STRUCT("#attributes" STRUCT(SystemTime TIMESTAMP)), EventRecordID BIGINT, Correlation STRUCT("#attributes" STRUCT(ActivityID UUID)), Execut │           │
│          │         │          │              │ ion STRUCT("#attributes" STRUCT(ProcessID BIGINT, ThreadID BIGINT)), Channel VARCHAR, Computer VARCHAR, "Security" JSON), EventData MAP(VARCHAR, JSON), UserData STRUCT(ServiceShutdown STRUC │           │
│          │         │          │              │ T("#attributes" STRUCT(xmlns VARCHAR))))']                                                                                                                                                    │           │
└──────────┴─────────┴──────────┴──────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────┘
```

## a
```sql
memory D
SELECT
Event.System.EventID AS event_id,
COUNT(*) AS count
FROM read_json_auto('C:/Users/SANSDFIR/security.json')
GROUP BY event_id
ORDER BY count DESC;
┌──────────┬────────┐
│ event_id │ count  │
│  int64   │ int64  │
├──────────┼────────┤
│     5061 │ 150931 │
│     4624 │  69121 │
│     4672 │  52079 │
│     4799 │  32503 │
│     4634 │  30884 │
│     5140 │  17009 │
│     4945 │   4410 │
│     4625 │   3707 │
│     4648 │   2617 │
│     4948 │    978 │
│     4946 │    977 │
│     4798 │    968 │
│     4611 │    188 │
│     4956 │    135 │
│     4688 │    120 │
│     4622 │    120 │
│     4697 │    109 │
│     4905 │     96 │
│     4904 │     96 │
│     4616 │     92 │
│     5142 │     74 │
│     4947 │     67 │
│     5144 │     38 │
│     4800 │     34 │
│     4801 │     32 │
│     4614 │     12 │
│     5478 │     12 │
│     4797 │     12 │
│     4608 │     12 │
│     4610 │     12 │
│     4826 │     12 │
│     4902 │     12 │
│     4944 │     12 │
│     1100 │      9 │
│     4647 │      9 │
│     4954 │      5 │
│     4692 │      4 │
│     4693 │      3 │
│     1101 │      3 │
│     4732 │      1 │
└──────────┴────────┘
       40 rows
```


## 4624
```sql
security D SELECT je.fullkey, COUNT(*)
           FROM read_json_objects('C:/Users/SANSDFIR/security.json') AS e, json_tree(e.json) AS je
           WHERE e.json->>'$.Event.System.EventID' = '4624'
           GROUP BY je.fullkey
           ORDER BY je.fullkey;
┌───────────────────────────────────────────────────┬──────────────┐
│                      fullkey                      │ count_star() │
│                      varchar                      │    int64     │
├───────────────────────────────────────────────────┼──────────────┤
│ $                                                 │        17544 │
│ $.Event                                           │        17544 │
│ $.Event.#attributes                               │        17544 │
│ $.Event.#attributes.xmlns                         │        17544 │
│ $.Event.EventData                                 │        17544 │
│ $.Event.EventData.AuthenticationPackageName       │        17544 │
│ $.Event.EventData.ElevatedToken                   │        17544 │
│ $.Event.EventData.ImpersonationLevel              │        17544 │
│ $.Event.EventData.IpAddress                       │        17544 │
│ $.Event.EventData.IpPort                          │        17544 │
│ $.Event.EventData.KeyLength                       │        17544 │
│ $.Event.EventData.LmPackageName                   │        17544 │
│ $.Event.EventData.LogonGuid                       │        17544 │
│ $.Event.EventData.LogonProcessName                │        17544 │
│ $.Event.EventData.LogonType                       │        17544 │
│ $.Event.EventData.ProcessId                       │        17544 │
│ $.Event.EventData.ProcessName                     │        17544 │
│ $.Event.EventData.RestrictedAdminMode             │        17544 │
│ $.Event.EventData.SubjectDomainName               │        17544 │
│ $.Event.EventData.SubjectLogonId                  │        17544 │
│ $.Event.EventData.SubjectUserName                 │        17544 │
│ $.Event.EventData.SubjectUserSid                  │        17544 │
│ $.Event.EventData.TargetDomainName                │        17544 │
│ $.Event.EventData.TargetLinkedLogonId             │        17544 │
│ $.Event.EventData.TargetLogonId                   │        17544 │
│ $.Event.EventData.TargetOutboundDomainName        │        17544 │
│ $.Event.EventData.TargetOutboundUserName          │        17544 │
│ $.Event.EventData.TargetUserName                  │        17544 │
│ $.Event.EventData.TargetUserSid                   │        17544 │
│ $.Event.EventData.TransmittedServices             │        17544 │
│ $.Event.EventData.VirtualAccount                  │        17544 │
│ $.Event.EventData.WorkstationName                 │        17544 │
│ $.Event.System                                    │        17544 │
│ $.Event.System.Channel                            │        17544 │
│ $.Event.System.Computer                           │        17544 │
│ $.Event.System.Correlation                        │        17544 │
│ $.Event.System.Correlation.#attributes            │        17521 │
│ $.Event.System.Correlation.#attributes.ActivityID │        17521 │
│ $.Event.System.EventID                            │        17544 │
│ $.Event.System.EventRecordID                      │        17544 │
│ $.Event.System.Execution                          │        17544 │
│ $.Event.System.Execution.#attributes              │        17544 │
│ $.Event.System.Execution.#attributes.ProcessID    │        17544 │
│ $.Event.System.Execution.#attributes.ThreadID     │        17544 │
│ $.Event.System.Keywords                           │        17544 │
│ $.Event.System.Level                              │        17544 │
│ $.Event.System.Opcode                             │        17544 │
│ $.Event.System.Provider                           │        17544 │
│ $.Event.System.Provider.#attributes               │        17544 │
│ $.Event.System.Provider.#attributes.Guid          │        17544 │
│ $.Event.System.Provider.#attributes.Name          │        17544 │
│ $.Event.System.Security                           │        17544 │
│ $.Event.System.Task                               │        17544 │
│ $.Event.System.TimeCreated                        │        17544 │
│ $.Event.System.TimeCreated.#attributes            │        17544 │
│ $.Event.System.TimeCreated.#attributes.SystemTime │        17544 │
│ $.Event.System.Version                            │        17544 │
└───────────────────────────────────────────────────┴──────────────┘
  57 rows                                                2 columns
```

```sql
security D 
SELECT Event.System.EventID, Event.EventData.LogonType, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.WorkstationName, Event.EventData.ProcessName, Event.EventData.IpAddress, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.EventID = 4624
GROUP BY ALL
ORDER BY ALL;
┌─────────┬───────────┬───────────────────┬─────────────────┬──────────────────┬─────────────────┬─────────────────┬──────────────────────────────────────────────────────────────┬───────────────────────────┬──────────────┐
│ EventID │ LogonType │ SubjectDomainName │ SubjectUserName │ TargetDomainName │ TargetUserName  │ WorkstationName │                         ProcessName                          │         IpAddress         │ count_star() │
│  int64  │   int64   │      varchar      │     varchar     │     varchar      │     varchar     │     varchar     │                           varchar                            │          varchar          │    int64     │
├─────────┼───────────┼───────────────────┼─────────────────┼──────────────────┼─────────────────┼─────────────────┼──────────────────────────────────────────────────────────────┼───────────────────────────┼──────────────┤
│    4624 │         0 │ -                 │ -               │ NT AUTHORITY     │ SYSTEM          │ -               │                                                              │ -                         │           23 │
│    4624 │         2 │                   │ MINWINPC$       │ Font Driver Host │ UMFD-0          │ -               │ C:\Windows\System32\wininit.exe                              │ -                         │            1 │
│    4624 │         2 │                   │ MINWINPC$       │ Font Driver Host │ UMFD-1          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            1 │
│    4624 │         2 │                   │ MINWINPC$       │ Window Manager   │ DWM-1           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            1 │
│    4624 │         2 │ WORKGROUP         │ RD01$           │ Font Driver Host │ UMFD-0          │ -               │ C:\Windows\System32\wininit.exe                              │ -                         │            1 │
│    4624 │         2 │ WORKGROUP         │ RD01$           │ Font Driver Host │ UMFD-1          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            1 │
│    4624 │         2 │ WORKGROUP         │ RD01$           │ Window Manager   │ DWM-1           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            2 │
│    4624 │         2 │ WORKGROUP         │ TPL-PACKER$     │ Font Driver Host │ UMFD-0          │ -               │ C:\Windows\System32\wininit.exe                              │ -                         │            6 │
│    4624 │         2 │ WORKGROUP         │ TPL-PACKER$     │ Font Driver Host │ UMFD-1          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            6 │
│    4624 │         2 │ WORKGROUP         │ TPL-PACKER$     │ Font Driver Host │ UMFD-2          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            1 │
│    4624 │         2 │ WORKGROUP         │ TPL-PACKER$     │ TPL-PACKER       │ Administrator   │ TPL-PACKER      │ C:\Windows\System32\svchost.exe                              │ 127.0.0.1                 │            3 │
│    4624 │         2 │ WORKGROUP         │ TPL-PACKER$     │ TPL-PACKER       │ defaultuser0    │ TPL-PACKER      │ C:\Windows\System32\oobe\msoobe.exe                          │ -                         │            1 │
│    4624 │         2 │ WORKGROUP         │ TPL-PACKER$     │ TPL-PACKER       │ defaultuser0    │ TPL-PACKER      │ C:\Windows\System32\svchost.exe                              │ 127.0.0.1                 │            1 │
│    4624 │         2 │ WORKGROUP         │ TPL-PACKER$     │ Window Manager   │ DWM-1           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            9 │
│    4624 │         2 │ WORKGROUP         │ TPL-PACKER$     │ Window Manager   │ DWM-2           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            1 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-0          │ -               │ C:\Windows\System32\wininit.exe                              │ -                         │           15 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-1          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           15 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-10         │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            2 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-11         │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            4 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-12         │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            1 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-13         │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            1 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-14         │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           10 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-15         │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            5 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-2          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           12 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-3          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           21 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-4          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            4 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-5          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            5 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-6          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            7 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-7          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            6 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-8          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            8 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Font Driver Host │ UMFD-9          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            4 │
│    4624 │         2 │ shieldbase        │ RD01$           │ RD01             │ SRLAdmin        │ RD01            │ C:\Windows\System32\consent.exe                              │ ::1                       │            2 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-1           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           30 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-10          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            4 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-11          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            8 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-12          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            2 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-13          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            2 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-14          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           20 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-15          │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           10 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-2           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           24 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-3           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           42 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-4           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            8 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-5           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           10 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-6           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           14 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-7           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           12 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-8           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │           16 │
│    4624 │         2 │ shieldbase        │ RD01$           │ Window Manager   │ DWM-9           │ -               │ C:\Windows\System32\winlogon.exe                             │ -                         │            8 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ DEV01$          │ -               │ -                                                            │ 172.16.4.9                │            3 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ HUNT01$         │ -               │ -                                                            │ 172.16.5.25               │            2 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ RD01$           │ -               │ -                                                            │ -                         │          333 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ RD01$           │ -               │ -                                                            │ ::1                       │         2794 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ cbarton-a       │ -               │ -                                                            │ 172.16.5.25               │           26 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ rsydow-a        │ -               │ -                                                            │ 172.16.4.4                │          677 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ rsydow-a        │ -               │ -                                                            │ 172.16.4.7                │           48 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ rsydow-a        │ -               │ -                                                            │ 172.16.6.18               │           82 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ slevine         │ -               │ -                                                            │ 172.16.6.18               │            1 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ wacsvc          │ -               │ -                                                            │ 172.16.6.18               │            1 │
│    4624 │         3 │ -                 │ -               │ SHIELDBASE.COM   │ wacsvc          │ -               │ -                                                            │ fe80::7e6b:763c:b405:22b4 │            1 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ DEV01$          │ DEV01           │ -                                                            │ 172.16.4.9                │            1 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ rsydow-a        │ DC01            │ -                                                            │ 172.16.4.4                │            4 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ rsydow-a        │ RD08            │ -                                                            │ 172.16.6.18               │            3 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ rsydow-a        │ WAC01           │ -                                                            │ 172.16.4.7                │            4 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ tdungan         │ DUNGANATOR      │ -                                                            │ 172.16.30.14              │            6 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ tdungan         │ DUNGANATOR      │ -                                                            │ 172.16.30.20              │           14 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ tdungan         │ DUNGANATOR      │ -                                                            │ 172.16.30.23              │            5 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ tdungan         │ DUNGANATOR      │ -                                                            │ 172.16.30.3               │           33 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ tdungan         │ DUNGANATOR      │ -                                                            │ 172.16.30.4               │           48 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ tdungan         │ DUNGANATOR      │ -                                                            │ 172.16.30.6               │           12 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ tdungan         │ DUNGANATOR      │ -                                                            │ 172.16.30.7               │            2 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ tdungan         │ DUNGANATOR      │ -                                                            │ 172.16.30.8               │           42 │
│    4624 │         3 │ -                 │ -               │ shieldbase       │ tdungan         │ DUNGANATOR      │ -                                                            │ 172.16.30.9               │            6 │
│    4624 │         3 │ WORKGROUP         │ TPL-PACKER$     │ TPL-PACKER       │ Administrator   │ TPL-PACKER      │ C:\Windows\System32\svchost.exe                              │ -                         │          505 │
│    4624 │         3 │ WORKGROUP         │ TPL-PACKER$     │ TPL-PACKER       │ Administrator   │ TPL-PACKER      │ C:\Windows\System32\svchost.exe                              │ ::1                       │          117 │
│    4624 │         5 │                   │ MINWINPC$       │ NT AUTHORITY     │ LOCAL SERVICE   │ -               │ C:\Windows\System32\services.exe                             │ -                         │            1 │
│    4624 │         5 │                   │ MINWINPC$       │ NT AUTHORITY     │ NETWORK SERVICE │ -               │ C:\Windows\System32\services.exe                             │ -                         │            1 │
│    4624 │         5 │                   │ MINWINPC$       │ NT AUTHORITY     │ SYSTEM          │ -               │ C:\Windows\System32\services.exe                             │ -                         │           24 │
│    4624 │         5 │ WORKGROUP         │ RD01$           │ NT AUTHORITY     │ LOCAL SERVICE   │ -               │ C:\Windows\System32\services.exe                             │ -                         │            1 │
│    4624 │         5 │ WORKGROUP         │ RD01$           │ NT AUTHORITY     │ NETWORK SERVICE │ -               │ C:\Windows\System32\services.exe                             │ -                         │            1 │
│    4624 │         5 │ WORKGROUP         │ RD01$           │ NT AUTHORITY     │ SYSTEM          │ -               │ C:\Windows\System32\services.exe                             │ -                         │           49 │
│    4624 │         5 │ WORKGROUP         │ TPL-PACKER$     │ NT AUTHORITY     │ LOCAL SERVICE   │ -               │ C:\Windows\System32\services.exe                             │ -                         │            6 │
│    4624 │         5 │ WORKGROUP         │ TPL-PACKER$     │ NT AUTHORITY     │ NETWORK SERVICE │ -               │ C:\Windows\System32\services.exe                             │ -                         │            6 │
│    4624 │         5 │ WORKGROUP         │ TPL-PACKER$     │ NT AUTHORITY     │ SYSTEM          │ -               │ C:\Windows\System32\services.exe                             │ -                         │          261 │
│    4624 │         5 │ shieldbase        │ RD01$           │ NT AUTHORITY     │ LOCAL SERVICE   │ -               │ C:\Windows\System32\services.exe                             │ -                         │           15 │
│    4624 │         5 │ shieldbase        │ RD01$           │ NT AUTHORITY     │ LOCAL SERVICE   │ -               │ C:\Windows\System32\svchost.exe                              │ -                         │            1 │
│    4624 │         5 │ shieldbase        │ RD01$           │ NT AUTHORITY     │ NETWORK SERVICE │ -               │ C:\Windows\System32\services.exe                             │ -                         │           15 │
│    4624 │         5 │ shieldbase        │ RD01$           │ NT AUTHORITY     │ NETWORK SERVICE │ -               │ C:\Windows\System32\svchost.exe                              │ -                         │            1 │
│    4624 │         5 │ shieldbase        │ RD01$           │ NT AUTHORITY     │ SYSTEM          │ -               │ C:\Windows\System32\services.exe                             │ -                         │        11900 │
│    4624 │         9 │ shieldbase        │ tdungan         │ shieldbase       │ tdungan         │ -               │ C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe │ -                         │            1 │
│    4624 │         9 │ shieldbase        │ tdungan         │ shieldbase       │ tdungan         │ -               │ C:\Windows\System32\svchost.exe                              │ ::1                       │           17 │
│    4624 │         9 │ shieldbase        │ tdungan         │ shieldbase       │ tdungan         │ -               │ C:\Windows\explorer.exe                                      │ -                         │            1 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 0.0.0.0                   │            1 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.30.14              │            3 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.30.20              │            5 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.30.23              │            2 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.30.3               │           11 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.30.4               │           19 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.30.6               │            6 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.30.7               │            1 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.30.8               │           19 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ wacsvc          │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.4.9                │            2 │
│    4624 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ wacsvc          │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.6.18               │           12 │
│    4624 │        11 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\consent.exe                              │ ::1                       │            1 │
│    4624 │        12 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan         │ RD01            │ C:\Windows\System32\svchost.exe                              │ 172.16.30.20              │            1 │
└─────────┴───────────┴───────────────────┴─────────────────┴──────────────────┴─────────────────┴─────────────────┴──────────────────────────────────────────────────────────────┴───────────────────────────┴──────────────┘
  103 rows                                                                                                                                                                                                        10 columns
```

## 4625
```sql
security D 
SELECT Event.System.EventID, Event.EventData.LogonType, Event.EventData.SubjectDomainName, Event.EventData.SubjectUserName, Event.EventData.TargetDomainName, Event.EventData.TargetUserName, Event.EventData.WorkstationName, Event.EventData.ProcessName, Event.EventData.IpAddress, COUNT(*)
FROM read_json('C:/Users/SANSDFIR/security.json', map_inference_threshold = -1)
WHERE Event.System.EventID = 4625
GROUP BY ALL
ORDER BY ALL;
┌─────────┬───────────┬───────────────────┬─────────────────┬──────────────────┬────────────────┬─────────────────┬─────────────────────────────────────────────────────────────────────────────────────────┬──────────────┬──────────────┐
│ EventID │ LogonType │ SubjectDomainName │ SubjectUserName │ TargetDomainName │ TargetUserName │ WorkstationName │                                       ProcessName                                       │  IpAddress   │ count_star() │
│  int64  │   int64   │      varchar      │     varchar     │     varchar      │    varchar     │     varchar     │                                         varchar                                         │   varchar    │    int64     │
├─────────┼───────────┼───────────────────┼─────────────────┼──────────────────┼────────────────┼─────────────────┼─────────────────────────────────────────────────────────────────────────────────────────┼──────────────┼──────────────┤
│    4625 │         2 │ TPL-PACKER        │ Administrator   │ TPL-PACKER       │ Administrator  │ TPL-PACKER      │ C:\Program Files (x86)\Microsoft\EdgeWebView\Application\90.0.818.66\msedgewebview2.exe │ -            │            2 │
│    4625 │         2 │ shieldbase        │ RD01$           │ RD01             │ srladmin       │ RD01            │ C:\Windows\System32\consent.exe                                                         │ ::1          │            1 │
│    4625 │         3 │ -                 │ -               │ shieldbase       │ tdungan        │ DUNGANATOR      │ -                                                                                       │ 172.16.30.20 │            4 │
│    4625 │         3 │ shieldbase        │ RD01$           │                  │ sprx           │ RD01            │ C:\Windows\System32\svchost.exe                                                         │ -            │           14 │
│    4625 │         3 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ RD01            │ C:\Windows\System32\svchost.exe                                                         │ -            │            1 │
│    4625 │        10 │ shieldbase        │ RD01$           │ shieldbase       │ tdungan        │ RD01            │ C:\Windows\System32\svchost.exe                                                         │ 172.16.30.20 │            1 │
│    4625 │        11 │ shieldbase        │ RD01$           │ RD01             │ srladmin       │ RD01            │ C:\Windows\System32\consent.exe                                                         │ ::1          │            2 │
└─────────┴───────────┴───────────────────┴─────────────────┴──────────────────┴────────────────┴─────────────────┴─────────────────────────────────────────────────────────────────────────────────────────┴──────────────┴──────────────┘
```
