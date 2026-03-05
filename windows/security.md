## LINKS
- 一覧  
  https://github.com/TonyPhipps/SIEM/blob/master/Notable-Event-IDs.md
- 詳細  
  https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx
- EvtxECmdのmapping  
  https://github.com/EricZimmerman/evtx/tree/master/evtx/Maps 
- microsoft  
  https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/security-auditing-overview
- hayabusaのセキュリティログ説明  
  https://github.com/Yamato-Security/EnableWindowsLogSettings/blob/6d2c0a15351650309d9bb325d63c5ef051a2157d/ConfiguringSecurityLogAuditPolicies-Japanese.md


## eventdataのこまんど
```powershell
([xml](($data = Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4624)]]" -MaxEvents 1).ToXml())).Event.EventData.Data | Select-Object Name
```

## 4624
logontype=9の場合、runasコマンド等で明示したユーザがTargetOutboundUserNameに確認可能
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4624)] and EventData[Data[@Name='LogonType']=9]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                     = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        IpAddress                = $eventData["IpAddress"]
        IpPort                   = $eventData["IpPort"]
        WorkstationName          = $eventData["WorkstationName"]
        TargetDomainName         = $eventData["TargetDomainName"]
        TargetUserName           = $eventData["TargetUserName"]
        #TargetUserSid            = $eventData["TargetUserSid"]
        TargetOutboundDomainName = $eventData["TargetOutboundDomainName"]
        TargetOutboundUserName   = $eventData["TargetOutboundUserName"]
        ProcessName              = $eventData["ProcessName"]
        ProcessID                = $eventData["ProcessID"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                IpAddress IpPort WorkstationNamen TargetDomainName TargetUserName TargetOutboundDomainName TargetOutboundUserName ProcessName                                                  ProcessID
----                --------- ------ ---------------- ---------------- -------------- ------------------------ ---------------------- -----------                                                  ---------
2023/01/23 15:00:42 -         -      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe 0xc8c    
2023/01/23 15:14:05 ::1       0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
```

## 4625
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4625)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time             = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        LogonType        = $eventData["LogonType"]
        FailureReason    = $eventData["FailureReason"]
        IpAddress        = $eventData["IpAddress"]
        IpPort           = $eventData["IpPort"]
        WorkstationName  = $eventData["WorkstationName"]
        TargetDomainName = $eventData["TargetDomainName"]
        TargetUserName   = $eventData["TargetUserName"]
        #TargetUserSid   = $eventData["TargetUserSid"]
        LogonProcessName = $eventData["LogonProcessName"]
        ProcessName      = $eventData["ProcessName"]
        ProcessID        = $eventData["ProcessID"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Wrap -Property *

Time                LogonType FailureReason IpAddress   IpPort WorkstationName TargetDomainName TargetUserName LogonProcessName ProcessName                                                                             ProcessID
----                --------- ------------- ---------   ------ --------------- ---------------- -------------- ---------------- -----------                                                                             ---------
2022/08/31 17:38:01 2         %%2313        -            -      TPL-PACKER     TPL-PACKER       Administrator  Advapi           C:\Program Files (x86)\Microsoft\EdgeWebView\Application\90.0.818.66\msedgewebview2.exe 0x1cbc   
2022/10/21 16:38:19 11        %%2304        ::1          0      RD01           RD01             srladmin       CredPro          C:\Windows\System32\consent.exe                                                         0x828    
```

## 4648
数が多いので、一旦プロセス名でグループ化
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time              = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        #SubjectDomainName = $eventData["SubjectDomainName"]
        #SubjectUserName   = $eventData["SubjectUserName"]
        #IpAddress         = $eventData["IpAddress"]
        #IpPort            = $eventData["IpPort"]
        #TargetServerName  = $eventData["TargetServerName"]
        #TargetInfo        = $eventData["TargetInfo"]
        #TargetDomainName  = $eventData["TargetDomainName"]
        #TargetUserName    = $eventData["TargetUserName"]
        #TargetUserSid     = $eventData["TargetUserSid"]
        ProcessName       = $eventData["ProcessName"]
        #ProcessId         = $eventData["ProcessId"]
    }
} | Group-Object ProcessName | Format-Table -AutoSize -Wrap -Property Values,Count

Values                                                      Count
------                                                      -----
{$null}                                                        15
{C:\Windows\System32\taskhostw.exe}                           333
{C:\Windows\System32\svchost.exe}                             704
{C:\Windows\System32\winlogon.exe}                            228
{C:\Windows\System32\wininit.exe}                              23
{C:\Windows\System32\wbem\WMIC.exe}                             2
{C:\Windows\System32\lsass.exe}                                 2
{C:\Windows\System32\consent.exe}                               2
{C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe}     5
{C:\Windows\System32\oobe\msoobe.exe}                           1
```
dcに対する認証を確認できる
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)] and EventData[Data[@Name='ProcessName']='C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe']]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time              = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        SubjectDomainName = $eventData["SubjectDomainName"]
        SubjectUserName   = $eventData["SubjectUserName"]
        IpAddress         = $eventData["IpAddress"]
        IpPort            = $eventData["IpPort"]
        TargetServerName  = $eventData["TargetServerName"]
        TargetInfo        = $eventData["TargetInfo"]
        TargetDomainName  = $eventData["TargetDomainName"]
        TargetUserName    = $eventData["TargetUserName"]
        #TargetUserSid     = $eventData["TargetUserSid"]
        ProcessName       = $eventData["ProcessName"]
        ProcessId         = $eventData["ProcessId"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Wrap -Property *

Time                SubjectDomainName SubjectUserName IpAddress IpPort TargetServerName    TargetInfo          TargetDomainName TargetUserName ProcessName                                               ProcessId
----                ----------------- --------------- --------- ------ ----------------    ----------          ---------------- -------------- -----------                                               ---------
2022/09/30 23:43:50 -                 -               -         -      dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:51 -                 -               -         -      dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:51 -                 -               -         -      dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:53 -                 -               -         -      dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:53 -                 -               -         -      dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30
```


## 4672
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4672)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time              = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        #SubjectDomainName = $eventData["SubjectDomainName"]
        SubjectUserName   = $eventData["SubjectUserName"]
    }
} | Where-Object {$_.SubjectUserName -ne "SYSTEM"} | Group-Object SubjectUserName | Format-Table -AutoSize -Wrap -Property Values,Count

Values            Count
------            -----
{RD01$}            3127
{wacsvc}              9
{LOCAL SERVICE}      24
{NETWORK SERVICE}    24
{cbarton-a}          26
{rsydow-a}          818
{SRLAdmin}            1
{Administrator}     625
{defaultuser0}        2
```
