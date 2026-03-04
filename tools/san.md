

PS C:\Users\SANSDFIR\Desktop\hayabusa-3.8.1-win-x64-live-response> .\hayabusa-3.8.1-win-x64.exe search -q -f E:\c\Windows\System32\winevt\logs\Security.evtx -r "." -F EventID:4624 -F LogonType:9 -b

Searching...

Start time: 2026/03/04 01:30
Total event log files: 1
Total file size: 115.1 MiB

Currently searching. Please wait.

Timestamp · EventTitle · Hostname · Channel · Event ID · Record ID · AllFieldInfo · EvtxFile
2023-01-23 15:00:42.631 +00:00 · Logon success · rd01.shieldbase.com · Security · 4624 · 163859 · AuthenticationPackageName: Negotiate ¦ ElevatedToken: %%1843 ¦ ImpersonationLevel: %%1833 ¦ IpAddress: - ¦ IpPort: - ¦ KeyLength: 0 ¦ LmPackageName: - ¦ LogonGuid: 00000000-0000-0000-0000-000000000000 ¦ LogonProcessName: Advapi ¦ LogonType: 9 ¦ ProcessId: 0xc8c ¦ ProcessName: C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe ¦ RestrictedAdminMode: - ¦ SubjectDomainName: shieldbase ¦ SubjectLogonId: 0xe1f07 ¦ SubjectUserName: tdungan ¦ SubjectUserSid: S-1-5-21-2838623409-1327563992-2591358621-1136 ¦ TargetDomainName: shieldbase ¦ TargetLinkedLogonId: 0x0 ¦ TargetLogonId: 0x28caf8 ¦ TargetOutboundDomainName: shieldbase ¦ TargetOutboundUserName: wacsvc ¦ TargetUserName: tdungan ¦ TargetUserSid: S-1-5-21-2838623409-1327563992-2591358621-1136 ¦ TransmittedServices: - ¦ VirtualAccount: %%1843 ¦ WorkstationName: - · E:\c\Windows\System32\winevt\logs\Security.evtx
