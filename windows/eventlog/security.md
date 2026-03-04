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
([xml](($data = Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4624)] and EventData[Data[@Name='LogonType']=9]]" -MaxEvents 1).ToXml())).Event.EventData.Data | Select-Object Name
```

## 4624
logontype=9の場合、runasコマンド等で明示したユーザがTargetOutboundUserNameに確認可能
```powershell
PS C:\> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4624)] and EventData[Data[@Name='LogonType']=9]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                     = $_.TimeCreated
        IpAddres                 = $eventData["IpAddress"]
        IpPort                   = $eventData["IpPort"]
        WorkstationNamen         = $eventData["WorkstationName"]
        TargetDomainName         = $eventData["TargetDomainName"]
        TargetUserName           = $eventData["TargetUserName"]
        #TargetUserSid            = $eventData["TargetUserSid"]
        TargetOutboundDomainName = $eventData["TargetOutboundDomainName"]
        TargetOutboundUserName   = $eventData["TargetOutboundUserName"]
        ProcessName              = $eventData["ProcessName"]
        ProcessID                = $eventData["ProcessID"]
    }
} | Format-Table -AutoSize

Time                 IpAddres IpPort WorkstationNamen TargetDomainName TargetUserName TargetOutboundDomainName TargetOutboundUserName ProcessName                                                  ProcessID
----                 -------- ------ ---------------- ---------------- -------------- ------------------------ ---------------------- -----------                                                  ---------
1/25/2023 3:07:55 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/23/2023 3:00:42 PM -        -      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe 0xc8c
```
