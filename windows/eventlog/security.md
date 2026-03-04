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
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                IpAddres IpPort WorkstationNamen TargetDomainName TargetUserName TargetOutboundDomainName TargetOutboundUserName ProcessName                                                  ProcessID
----                -------- ------ ---------------- ---------------- -------------- ------------------------ ---------------------- -----------                                                  ---------
2023/01/23 15:00:42 -        -      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe 0xc8c    
2023/01/23 15:14:05 ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
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
        IpAddres         = $eventData["IpAddress"]
        IpPort           = $eventData["IpPort"]
        WorkstationNamen = $eventData["WorkstationName"]
        TargetDomainName = $eventData["TargetDomainName"]
        TargetUserName   = $eventData["TargetUserName"]
        #TargetUserSid   = $eventData["TargetUserSid"]
        LogonProcessName = $eventData["LogonProcessName"]
        ProcessName      = $eventData["ProcessName"]
        ProcessID        = $eventData["ProcessID"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Wrap -Property *

Time                LogonType FailureReason IpAddres     IpPort WorkstationNamen TargetDomainName TargetUserName LogonProcessName ProcessName                                                                             ProcessID
----                --------- ------------- --------     ------ ---------------- ---------------- -------------- ---------------- -----------                                                                             ---------
2022/08/31 17:38:01 2         %%2313        -            -      TPL-PACKER       TPL-PACKER       Administrator  Advapi           C:\Program Files (x86)\Microsoft\EdgeWebView\Application\90.0.818.66\msedgewebview2.exe 0x1cbc   
2022/10/21 16:38:19 11        %%2304        ::1          0      RD01             RD01             srladmin       CredPro          C:\Windows\System32\consent.exe                                                         0x828    
```
