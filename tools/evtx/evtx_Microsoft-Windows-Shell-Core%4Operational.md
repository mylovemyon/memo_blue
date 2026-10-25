```poewrshell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f Shell-Core%4Operational.json C:\Windows\System32\winevt\logs\Microsoft-Windows-Shell-Core%4Operational.evtx
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
         FROM read_json('.\Shell-Core%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│    9648 │          827 │
│    9649 │          828 │
│    9652 │          497 │
│    9653 │          498 │
│    9703 │            9 │
│    9704 │            9 │
│    9705 │          363 │
│    9706 │          363 │
│    9707 │          591 │
│    9708 │          592 │
│   22082 │            5 │
│   22083 │            5 │
│   28017 │          636 │
│   28018 │          637 │
│   28019 │          628 │
│   28032 │          588 │
│   28115 │          733 │
│   28117 │          489 │
│   28125 │          259 │
│   62144 │         1536 │
│   62164 │           16 │
│   62170 │         6731 │
│   62171 │         6857 │
│   62408 │           31 │
│   62409 │           31 │
└─────────┴──────────────┘
  25 rows      2 columns
```



## 9701 (RunOnceEx commands started)



## 9702 (RunOnceEx commands finished)



## 9703 (RunOnce commands started)
```sql
memory D SELECT Event.System.Computer, Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
         FROM read_json('.\Shell-Core%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 9703
         GROUP BY ALL
         ORDER BY Event.System.TimeCreated."#attributes".SystemTime;
┌─────────────────────┬────────────────────────────┬──────────────┐
│      Computer       │         SystemTime         │ count_star() │
│       varchar       │         timestamp          │    int64     │
├─────────────────────┼────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-17 14:43:27.78511  │            1 │
│ rd01.shieldbase.com │ 2023-01-23 20:53:10.939479 │            1 │
└─────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Shell-Core%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '9703' AND je.fullkey LIKE '$.Event.EventData%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────┬──────────────┐
│      fullkey      │ count_star() │
│      varchar      │    int64     │
├───────────────────┼──────────────┤
│ $.Event.EventData │            2 │
└───────────────────┴──────────────┘
```



## 9704 (RunOnce commands finished)
```sql
memory D SELECT Event.System.Computer, Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
         FROM read_json('.\Shell-Core%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 9704
         GROUP BY ALL
         ORDER BY Event.System.TimeCreated."#attributes".SystemTime;
┌─────────────────────┬────────────────────────────┬──────────────┐
│      Computer       │         SystemTime         │ count_star() │
│       varchar       │         timestamp          │    int64     │
├─────────────────────┼────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-17 14:43:30.560519 │            1 │
│ rd01.shieldbase.com │ 2023-01-23 20:53:12.239097 │            1 │
└─────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Shell-Core%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '9704' AND je.fullkey LIKE '$.Event.EventData%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────┬──────────────┐
│      fullkey      │ count_star() │
│      varchar      │    int64     │
├───────────────────┼──────────────┤
│ $.Event.EventData │            2 │
└───────────────────┴──────────────┘
```



## 9705 (Started enumeration of commands for registry key)
```sql
memory D SELECT Event.System.Computer, Event.EventData.KeyName, COUNT(*)
         FROM read_json('.\Shell-Core%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 9705
         GROUP BY ALL
         ORDER BY Event.EventData.KeyName;
┌─────────────────────┬───────────────────────────────────────────────────┬──────────────┐
│      Computer       │                      KeyName                      │ count_star() │
│       varchar       │                      varchar                      │    int64     │
├─────────────────────┼───────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ Software\Microsoft\Windows\CurrentVersion\Run     │           20 │
│ rd01.shieldbase.com │ Software\Microsoft\Windows\CurrentVersion\RunOnce │           13 │
└─────────────────────┴───────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Shell-Core%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '9705' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────┬──────────────┐
│          fullkey          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ $.Event.EventData.KeyName │           33 │
└───────────────────────────┴──────────────┘
```



## 9706 (Finished enumeration of commands for registry key)
```sql
memory D SELECT Event.System.Computer, Event.EventData.KeyName, COUNT(*)
         FROM read_json('.\Shell-Core%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 9706
         GROUP BY ALL
         ORDER BY Event.EventData.KeyName;
┌─────────────────────┬───────────────────────────────────────────────────┬──────────────┐
│      Computer       │                      KeyName                      │ count_star() │
│       varchar       │                      varchar                      │    int64     │
├─────────────────────┼───────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ Software\Microsoft\Windows\CurrentVersion\Run     │           20 │
│ rd01.shieldbase.com │ Software\Microsoft\Windows\CurrentVersion\RunOnce │           13 │
└─────────────────────┴───────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Shell-Core%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '9706' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────┬──────────────┐
│          fullkey          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ $.Event.EventData.KeyName │           33 │
└───────────────────────────┴──────────────┘
```



## 9707 (Started execution of command)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Command, COUNT(*)
         FROM read_json('.\Shell-Core%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 9707
         GROUP BY ALL
         ORDER BY Event.EventData.Command;
┌─────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │                                             Command                                             │ count_star() │
│       varchar       │                                             varchar                                             │    int64     │
├─────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 21.220.1024.0005"                                                                               │            1 │
│ rd01.shieldbase.com │ 22.248.1127.0001"                                                                               │            1 │
│ rd01.shieldbase.com │ 22.253.1204.0001"                                                                               │            1 │
│ rd01.shieldbase.com │ Ec2Install.exe" /burn.runonce                                                                   │            1 │
│ rd01.shieldbase.com │ OneDrive.exe" /background                                                                       │            9 │
│ rd01.shieldbase.com │ OneDriveSetup.exe /thfirstsetup                                                                 │            1 │
│ rd01.shieldbase.com │ OneDriveSetup.exe"                                                                              │            6 │
│ rd01.shieldbase.com │ SecurityHealthSystray.exe                                                                       │           10 │
│ rd01.shieldbase.com │ amd64"                                                                                          │            1 │
│ rd01.shieldbase.com │ chrmstp.exe" --configure-user-settings --verbose-logging --system-level --channel=stable        │            1 │
│ rd01.shieldbase.com │ ie4uinit.exe -UserConfig                                                                        │            1 │
│ rd01.shieldbase.com │ msedge.exe" --no-startup-window --win-session-start /prefetch:5                                 │            1 │
│ rd01.shieldbase.com │ setup.exe" --configure-user-settings --verbose-logging --system-level --msedge --channel=stable │            1 │
│ rd01.shieldbase.com │ setup.exe" --msedgewebview --delete-old-versions --system-level --verbose-logging --on-logon    │            1 │
│ rd01.shieldbase.com │ slack.exe" --process-start-args --startup                                                       │            8 │
│ rd01.shieldbase.com │ unregmp2.exe /FirstLogon                                                                        │            2 │
└─────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
  16 rows                                                                                                                    3 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Shell-Core%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '9707' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────┬──────────────┐
│          fullkey          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ $.Event.EventData.Command │           46 │
└───────────────────────────┴──────────────┘
```



## 9708 (Finished execution of command)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Command, COUNT(*)
         FROM read_json('.\Shell-Core%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 9708
         GROUP BY ALL
         ORDER BY Event.EventData.Command;
┌─────────────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │                                             Command                                             │ count_star() │
│       varchar       │                                             varchar                                             │    int64     │
├─────────────────────┼─────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 21.220.1024.0005"                                                                               │            1 │
│ rd01.shieldbase.com │ 22.248.1127.0001"                                                                               │            1 │
│ rd01.shieldbase.com │ 22.253.1204.0001"                                                                               │            1 │
│ rd01.shieldbase.com │ Ec2Install.exe" /burn.runonce                                                                   │            1 │
│ rd01.shieldbase.com │ OneDrive.exe" /background                                                                       │            9 │
│ rd01.shieldbase.com │ OneDriveSetup.exe /thfirstsetup                                                                 │            1 │
│ rd01.shieldbase.com │ OneDriveSetup.exe"                                                                              │            6 │
│ rd01.shieldbase.com │ SecurityHealthSystray.exe                                                                       │           10 │
│ rd01.shieldbase.com │ amd64"                                                                                          │            1 │
│ rd01.shieldbase.com │ chrmstp.exe" --configure-user-settings --verbose-logging --system-level --channel=stable        │            1 │
│ rd01.shieldbase.com │ ie4uinit.exe -UserConfig                                                                        │            1 │
│ rd01.shieldbase.com │ msedge.exe" --no-startup-window --win-session-start /prefetch:5                                 │            1 │
│ rd01.shieldbase.com │ setup.exe" --configure-user-settings --verbose-logging --system-level --msedge --channel=stable │            1 │
│ rd01.shieldbase.com │ setup.exe" --msedgewebview --delete-old-versions --system-level --verbose-logging --on-logon    │            1 │
│ rd01.shieldbase.com │ slack.exe" --process-start-args --startup                                                       │            8 │
│ rd01.shieldbase.com │ unregmp2.exe /FirstLogon                                                                        │            2 │
└─────────────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
  16 rows                                                                                                                    3 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Shell-Core%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '9708' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────┬──────────────┐
│          fullkey          │ count_star() │
│          varchar          │    int64     │
├───────────────────────────┼──────────────┤
│ $.Event.EventData.Command │           46 │
│ $.Event.EventData.PID     │           46 │
└───────────────────────────┴──────────────┘
```




## 28115 (Shortcut creation log after program installation)
```sql
memory D SELECT Event.System.Computer, Event.EventData.Name, Event.EventData.AppID, COUNT(*)
         FROM read_json('.\Shell-Core%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 28115
         GROUP BY ALL
         ORDER BY Event.EventData.Name;
┌─────────────────────┬──────────────────────────────────────────────────┬───────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │                       Name                       │                                           AppID                                           │ count_star() │
│       varchar       │                     varchar                      │                                          varchar                                          │    int64     │
├─────────────────────┼──────────────────────────────────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ Character Map                                    │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\charmap.exe                                        │            1 │
│ rd01.shieldbase.com │ Command Prompt                                   │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\cmd.exe                                            │            1 │
│ rd01.shieldbase.com │ Component Services                               │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\comexp.msc                                         │            1 │
│ rd01.shieldbase.com │ Computer Management                              │ Microsoft.AutoGenerated.{8ABD94FB-E7D6-84A6-A997-C918EDDE0AE5}                            │            1 │
│ rd01.shieldbase.com │ Control Panel                                    │ Microsoft.Windows.ControlPanel                                                            │            1 │
│ rd01.shieldbase.com │ Database Compare                                 │ Microsoft.Office.DATABASECOMPARE.EXE.15                                                   │            1 │
│ rd01.shieldbase.com │ Defragment and Optimize Drives                   │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\dfrgui.exe                                         │            1 │
│ rd01.shieldbase.com │ Disk Cleanup                                     │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\cleanmgr.exe                                       │            1 │
│ rd01.shieldbase.com │ EC2ConfigService Settings                        │ Ec2Config                                                                                 │            2 │
│ rd01.shieldbase.com │ Event Viewer                                     │ Microsoft.AutoGenerated.{BB044BFD-25B7-2FAA-22A8-6371A93E0456}                            │            1 │
│ rd01.shieldbase.com │ Excel                                            │ Microsoft.Office.EXCEL.EXE.15                                                             │            1 │
│ rd01.shieldbase.com │ File Explorer                                    │ Microsoft.Windows.Explorer                                                                │            2 │
│ rd01.shieldbase.com │ Firefox                                          │ 308046B0AF4A39CB                                                                          │            2 │
│ rd01.shieldbase.com │ Foxit PDF Reader                                 │ {7C5A40EF-A0FB-4BFC-874A-C0F2E0B9FA8E}\Foxit Software\Foxit PDF Reader\FoxitPDFReader.exe │            2 │
│ rd01.shieldbase.com │ Google Chrome                                    │ Chrome                                                                                    │            3 │
│ rd01.shieldbase.com │ Local Security Policy                            │ Microsoft.AutoGenerated.{BD3F924E-55FB-A1BA-9DE6-B50F9F2460AC}                            │            1 │
│ rd01.shieldbase.com │ Magnifier                                        │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\magnify.exe                                        │            1 │
│ rd01.shieldbase.com │ Microsoft Edge                                   │ MSEdge                                                                                    │            3 │
│ rd01.shieldbase.com │ Narrator                                         │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\narrator.exe                                       │            1 │
│ rd01.shieldbase.com │ ODBC Data Sources (32-bit)                       │ {D65231B0-B2F1-4857-A4CE-A8E7C6EA7D27}\odbcad32.exe                                       │            1 │
│ rd01.shieldbase.com │ ODBC Data Sources (64-bit)                       │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\odbcad32.exe                                       │            1 │
│ rd01.shieldbase.com │ Office Language Preferences                      │ Microsoft.Office.SETLANG.EXE.15                                                           │            1 │
│ rd01.shieldbase.com │ Office Upload Center                             │ Microsoft.Office.MSOUC.EXE.15                                                             │            1 │
│ rd01.shieldbase.com │ On-Screen Keyboard                               │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\osk.exe                                            │            1 │
│ rd01.shieldbase.com │ OneDrive                                         │ Microsoft.SkyDrive.Desktop                                                                │            1 │
│ rd01.shieldbase.com │ OneNote 2016                                     │ Microsoft.Office.ONENOTE.EXE.15                                                           │            1 │
│ rd01.shieldbase.com │ Outlook                                          │ Microsoft.Office.OUTLOOK.EXE.15                                                           │            1 │
│ rd01.shieldbase.com │ Performance Monitor                              │ Microsoft.AutoGenerated.{8AA47365-B2B3-1961-69EB-F866E376B12F}                            │            1 │
│ rd01.shieldbase.com │ PowerPoint                                       │ Microsoft.Office.POWERPNT.EXE.15                                                          │            1 │
│ rd01.shieldbase.com │ Print Management                                 │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\printmanagement.msc                                │            1 │
│ rd01.shieldbase.com │ Quick Assist                                     │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\quickassist.exe                                    │            1 │
│ rd01.shieldbase.com │ Recovery Drive                                   │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\RecoveryDrive.exe                                  │            1 │
│ rd01.shieldbase.com │ Registry Editor                                  │ {F38BF404-1D43-42F2-9305-67DE0B28FC23}\regedit.exe                                        │            1 │
│ rd01.shieldbase.com │ Remote Desktop Connection                        │ Microsoft.Windows.RemoteDesktop                                                           │            1 │
│ rd01.shieldbase.com │ Resource Monitor                                 │ Microsoft.AutoGenerated.{C804BBA7-FA5F-CBF7-8B55-2096E5F972CB}                            │            1 │
│ rd01.shieldbase.com │ Run                                              │ Microsoft.Windows.Shell.RunDialog                                                         │            1 │
│ rd01.shieldbase.com │ Services                                         │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\services.msc                                       │            1 │
│ rd01.shieldbase.com │ Spreadsheet Compare                              │ Microsoft.Office.SPREADSHEETCOMPARE.EXE.15                                                │            1 │
│ rd01.shieldbase.com │ Steps Recorder                                   │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\psr.exe                                            │            1 │
│ rd01.shieldbase.com │ System Configuration                             │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\msconfig.exe                                       │            1 │
│ rd01.shieldbase.com │ System Information                               │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\msinfo32.exe                                       │            1 │
│ rd01.shieldbase.com │ Task Manager                                     │ Microsoft.AutoGenerated.{923DD477-5846-686B-A659-0FCCD73851A8}                            │            1 │
│ rd01.shieldbase.com │ Task Scheduler                                   │ Microsoft.AutoGenerated.{C1C6F8AC-40A3-0F5C-146F-65A9DC70BBB4}                            │            1 │
│ rd01.shieldbase.com │ Telemetry Dashboard for Office                   │ Microsoft.Office.msotd.exe.15                                                             │            1 │
│ rd01.shieldbase.com │ Telemetry Log for Office                         │ Microsoft.Office.msoev.exe.15                                                             │            1 │
│ rd01.shieldbase.com │ Uninstall Foxit PDF Reader                       │ {7C5A40EF-A0FB-4BFC-874A-C0F2E0B9FA8E}\Foxit Software\Foxit PDF Reader\unins000.exe       │            1 │
│ rd01.shieldbase.com │ Windows Defender Firewall with Advanced Security │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\WF.msc                                             │            1 │
│ rd01.shieldbase.com │ Windows Fax and Scan                             │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\WFS.exe                                            │            1 │
│ rd01.shieldbase.com │ Windows Media Player                             │ Microsoft.Windows.MediaPlayer32                                                           │            1 │
│ rd01.shieldbase.com │ Windows Memory Diagnostic                        │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\MdSched.exe                                        │            1 │
│ rd01.shieldbase.com │ Windows PowerShell                               │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\WindowsPowerShell\v1.0\powershell.exe              │            1 │
│ rd01.shieldbase.com │ Windows PowerShell (x86)                         │ {D65231B0-B2F1-4857-A4CE-A8E7C6EA7D27}\WindowsPowerShell\v1.0\powershell.exe              │            1 │
│ rd01.shieldbase.com │ Windows PowerShell ISE                           │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\WindowsPowerShell\v1.0\PowerShell_ISE.exe          │            1 │
│ rd01.shieldbase.com │ Windows PowerShell ISE (x86)                     │ {D65231B0-B2F1-4857-A4CE-A8E7C6EA7D27}\WindowsPowerShell\v1.0\PowerShell_ISE.exe          │            1 │
│ rd01.shieldbase.com │ Windows Speech Recognition                       │ Microsoft.AutoGenerated.{DAA168DE-4306-C8BC-8C11-B596240BDDED}                            │            1 │
│ rd01.shieldbase.com │ Windows Tools                                    │ Microsoft.Windows.AdministrativeTools                                                     │            1 │
│ rd01.shieldbase.com │ Word                                             │ Microsoft.Office.WINWORD.EXE.15                                                           │            1 │
│ rd01.shieldbase.com │ WordPad                                          │ {6D809377-6AF0-444B-8957-A3773F02200E}\Windows NT\Accessories\wordpad.exe                 │            1 │
│ rd01.shieldbase.com │ iSCSI Initiator                                  │ {1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\iscsicpl.exe                                       │            1 │
└─────────────────────┴──────────────────────────────────────────────────┴───────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
  59 rows                                                                                                                                                                 4 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\Shell-Core%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '28115' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────┬──────────────┐
│         fullkey         │ count_star() │
│         varchar         │    int64     │
├─────────────────────────┼──────────────┤
│ $.Event.EventData.AppID │           67 │
│ $.Event.EventData.Flags │           67 │
│ $.Event.EventData.Name  │           67 │
└─────────────────────────┴──────────────┘
```
