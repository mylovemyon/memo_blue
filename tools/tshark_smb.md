## smb.cmd
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -Y "smb.cmd" -T fields -e smb.cmd | Group-Object -NoElement | Select-Object Name,count | Sort-Object Name

Name      Count
----      -----
0x04       1336
0x10         76
0x24,0xff   806
0x25      18183
0x2b      15077
0x2d,0xff     4
0x2e         39
0x2e,0xff  9366
0x2f,0xff  3590
0x32       5550
0x71       3760
0x72       4599
0x73         14
0x73,0xff  5962
0x74,0xff  3546
0x75          6
0x75,0xff  3824
0xa2         74
0xa2,0xff  1478
0xa4          7
```
### 0x04
```powershell
PS C:\Users\Public> tshark.exe -r .\smb.pcap -Y "smb.cmd == 0x04" -T fields -e ip.src -e ip.dst -e smb.cmd -e smb.path -e smb.flags.response -E header=y -E separator=","  -E quote=d | Out-File -Encoding utf8 -FilePath .\0x04.csv
```
```powershell
0x04 D SELECT "smb.path", "smb.flags.response", "ip.src", "ip.dst", COUNT(*)
       FROM read_csv_auto('0x04.csv')
       GROUP BY ALL
       ORDER BY "smb.path";
┌───────────────────────────────────────────┬────────────────────┬───────────┬───────────┬──────────────┐
│                 smb.path                  │ smb.flags.response │  ip.src   │  ip.dst   │ count_star() │
│                  varchar                  │      boolean       │  varchar  │  varchar  │    int64     │
├───────────────────────────────────────────┼────────────────────┼───────────┼───────────┼──────────────┤
│ ADMIN$                                    │ false              │ 10.3.58.7 │ 10.3.58.5 │            2 │
│ ADMIN$                                    │ true               │ 10.3.58.5 │ 10.3.58.7 │            2 │
│ \\\\10.3.58.4\\ADMIN$                     │ true               │ 10.3.58.4 │ 10.3.58.7 │           12 │
│ \\\\10.3.58.4\\ADMIN$                     │ false              │ 10.3.58.7 │ 10.3.58.4 │           12 │
│ \\\\10.3.58.4\\C$                         │ true               │ 10.3.58.4 │ 10.3.58.7 │           11 │
│ \\\\10.3.58.4\\C$                         │ false              │ 10.3.58.7 │ 10.3.58.4 │           11 │
│ \\\\10.3.58.4\\IPC$                       │ false              │ 10.3.58.7 │ 10.3.58.4 │           52 │
│ \\\\10.3.58.4\\IPC$                       │ true               │ 10.3.58.4 │ 10.3.58.7 │           52 │
│ \\\\10.3.58.5\\ADMIN$                     │ true               │ 10.3.58.5 │ 10.3.58.7 │            8 │
│ \\\\10.3.58.5\\ADMIN$                     │ false              │ 10.3.58.7 │ 10.3.58.5 │            8 │
│ \\\\10.3.58.5\\IPC$                       │ false              │ 10.3.58.7 │ 10.3.58.5 │           49 │
│ \\\\10.3.58.5\\IPC$                       │ true               │ 10.3.58.5 │ 10.3.58.7 │           49 │
│ \\\\10.3.58.6\\ADMIN$                     │ false              │ 10.3.58.7 │ 10.3.58.6 │            9 │
│ \\\\10.3.58.6\\ADMIN$                     │ true               │ 10.3.58.6 │ 10.3.58.7 │            9 │
│ \\\\10.3.58.6\\C$                         │ false              │ 10.3.58.7 │ 10.3.58.6 │           45 │
│ \\\\10.3.58.6\\C$                         │ true               │ 10.3.58.6 │ 10.3.58.7 │           45 │
│ \\\\10.3.58.6\\IPC$                       │ false              │ 10.3.58.7 │ 10.3.58.6 │           41 │
│ \\\\10.3.58.6\\IPC$                       │ true               │ 10.3.58.6 │ 10.3.58.7 │           41 │
│ \\\\10.3.58.7\\C$                         │ false              │ 10.3.58.4 │ 10.3.58.7 │           21 │
│ \\\\10.3.58.7\\C$                         │ true               │ 10.3.58.7 │ 10.3.58.4 │           21 │
│ \\\\10.3.58.7\\IPC$                       │ false              │ 10.3.58.5 │ 10.3.58.7 │           13 │
│ \\\\10.3.58.7\\IPC$                       │ true               │ 10.3.58.7 │ 10.3.58.5 │           13 │
│ \\\\10.3.58.7\\IPC$                       │ false              │ 10.3.58.4 │ 10.3.58.7 │           38 │
│ \\\\10.3.58.7\\IPC$                       │ true               │ 10.3.58.7 │ 10.3.58.4 │           38 │
│ \\\\CONTROLLER.SHIELDBASE.LOCAL\\IPC$     │ false              │ 10.3.58.7 │ 10.3.58.4 │           17 │
│ \\\\CONTROLLER.SHIELDBASE.LOCAL\\IPC$     │ true               │ 10.3.58.4 │ 10.3.58.7 │           17 │
│ \\\\CONTROLLER.SHIELDBASE.LOCAL\\SYSVOL   │ false              │ 10.3.58.7 │ 10.3.58.9 │           65 │
│ \\\\CONTROLLER.SHIELDBASE.LOCAL\\SYSVOL   │ true               │ 10.3.58.9 │ 10.3.58.7 │           65 │
│ \\\\CONTROLLER.SHIELDBASE.LOCAL\\SYSVOL   │ true               │ 10.3.58.4 │ 10.3.58.7 │          113 │
│ \\\\CONTROLLER.SHIELDBASE.LOCAL\\SYSVOL   │ false              │ 10.3.58.7 │ 10.3.58.4 │          113 │
│ \\\\CONTROLLER\\IPC$                      │ false              │ 10.3.58.7 │ 10.3.58.4 │           17 │
│ \\\\CONTROLLER\\IPC$                      │ true               │ 10.3.58.9 │ 10.3.58.7 │            8 │
│ \\\\CONTROLLER\\IPC$                      │ false              │ 10.3.58.7 │ 10.3.58.9 │            8 │
│ \\\\CONTROLLER\\IPC$                      │ true               │ 10.3.58.4 │ 10.3.58.7 │           17 │
│ \\\\WKS-WINXP32BIT.SHIELDBASE.LOCAL\\IPC$ │ false              │ 10.3.58.4 │ 10.3.58.7 │            2 │
│ \\\\WKS-WINXP32BIT.SHIELDBASE.LOCAL\\IPC$ │ true               │ 10.3.58.7 │ 10.3.58.4 │            2 │
│ \\\\WKS-WINXP32BIT\\IPC$                  │ false              │ 10.3.58.4 │ 10.3.58.7 │           30 │
│ \\\\WKS-WINXP32BIT\\IPC$                  │ true               │ 10.3.58.7 │ 10.3.58.4 │           30 │
└───────────────────────────────────────────┴────────────────────┴───────────┴───────────┴──────────────┘
  38 rows                                                                                     5 columns
```


```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -Y "smb.cmd == 0x2b" -T fields -e smb.path -e smb.flags.response -e smb.nt_status | Group-Object -NoElement | Select-Object Name,count | Sort-Object Name

Name                                      Count
----                                      -----
\\\\10.3.58.5\\ADMIN$   False   0x00000000     2466
\\\\10.3.58.5\\ADMIN$   True    0x00000000      2465
\\\\10.3.58.5\\C$       False   0x00000000         2875
\\\\10.3.58.5\\C$       True    0x00000000          2875
\\\\10.3.58.5\\IPC$     False   0x00000000         23
\\\\10.3.58.5\\IPC$     True    0x00000000          22
\\\\10.3.58.6\\ADMIN$   False   0x00000000       16
\\\\10.3.58.6\\ADMIN$   True    0x00000000        16
\\\\10.3.58.6\\C$       False   0x00000000            1
\\\\10.3.58.6\\C$       True    0x00000000             1
\\\\10.3.58.6\\IPC$     False   0x00000000       2158
\\\\10.3.58.6\\IPC$     True    0x00000000        2157
\\\\WKS-WIN764BITB\\IPC$        False   0x00000000     1
\\\\WKS-WIN764BITB\\IPC$        True    0x00000000      1
```

```powershell
PS C:\Users\Public> tshark.exe -r .\smb.pcap -Y "smb.cmd" -T fields -e smb.path | Group-Object -NoElement | Select-Object Name,Count | Sort-Object Name

Name                                      Count
----                                      -----
                                           5274
\\\\10.3.58.4\\ADMIN$                       308
\\\\10.3.58.4\\C$                          2128
\\\\10.3.58.4\\IPC$                         796
\\\\10.3.58.5\\ADMIN$                       787
\\\\10.3.58.5\\C$                             6
\\\\10.3.58.5\\IPC$                         907
\\\\10.3.58.5\\IPC$,ADMIN$                    3
\\\\10.3.58.6\\ADMIN$                       204
\\\\10.3.58.6\\C$                          3629
\\\\10.3.58.6\\IPC$                        5140
\\\\10.3.58.7\\C$                           408
\\\\10.3.58.7\\IPC$                        9622
\\\\CONTROLLER.SHIELDBASE.LOCAL\\IPC$      4644
\\\\CONTROLLER.SHIELDBASE.LOCAL\\SYSVOL    3940
\\\\CONTROLLER\\HOME                         96
\\\\CONTROLLER\\IPC$                       1242
\\\\CONTROLLER\\NETLOGON                     14
\\\\CONTROLLER\\PUBLIC                       40
\\\\WKS-WINXP32BIT.SHIELDBASE.LOCAL\\IPC$    28
\\\\WKS-WINXP32BIT\\IPC$                    622
ADMIN$                                      143
IPC$                                         74
```
