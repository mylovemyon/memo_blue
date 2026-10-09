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
        Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer                  = $xml.Event.System.Computer
        IpAddress                 = $eventData["IpAddress"]
        IpPort                    = $eventData["IpPort"]
        WorkstationName           = $eventData["WorkstationName"]
        #SubjectLogonId            = $eventData["SubjectLogonId "]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        #SubjectDomainName         = $eventData["SubjectDomainName"]
        #SubjectUserName           = $eventData["SubjectUserName "]
        #TargetLogonId             = $eventData["TargetLogonId"]
        #TargetUserSid             = $eventData["TargetUserSid"]
        TargetDomainName          = $eventData["TargetDomainName"]
        TargetUserName            = $eventData["TargetUserName"]
        TargetOutboundDomainName  = $eventData["TargetOutboundDomainName"]
        TargetOutboundUserName    = $eventData["TargetOutboundUserName"]
        #LogonType                 = $eventData["LogonType"]
        #LogonProcessName          = $eventData["LogonProcessName"]
        #AuthenticationPackageName = $eventData["AuthenticationPackageName"]
        #ImpersonationLevel        = $eventData["ImpersonationLevel"]
        ProcessName               = $eventData["ProcessName"]
        ProcessID                 = $eventData["ProcessID"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            IpAddress IpPort WorkstationName TargetDomainName TargetUserName TargetOutboundDomainName TargetOutboundUserName ProcessName                                                  ProcessID
----                --------            --------- ------ --------------- ---------------- -------------- ------------------------ ---------------------- -----------                                                  ---------
2023/01/23 15:00:42 rd01.shieldbase.com -         -      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe 0xc8c    
2023/01/23 15:14:05 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
```

## 4625
```powershell
PS C:\Users\SANSDFIR>  Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4625)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
		Computer                  = $xml.Event.System.Computer
        IpAddress                 = $eventData["IpAddress"]
        IpPort                    = $eventData["IpPort"]
        WorkstationName           = $eventData["WorkstationName"]
        #SubjectLogonId            = $eventData["SubjectLogonId "]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        #SubjectDomainName         = $eventData["SubjectDomainName"]
        #SubjectUserName           = $eventData["SubjectUserName "]
        #TargetUserSid             = $eventData["TargetUserSid"]
        TargetDomainName          = $eventData["TargetDomainName"]
        TargetUserName            = $eventData["TargetUserName"]
        LogonType                 = $eventData["LogonType"]
        FailureReason             = $eventData["FailureReason"]
        #LogonProcessName          = $eventData["LogonProcessName"]
        #AuthenticationPackageName = $eventData["AuthenticationPackageName"]
        ProcessName               = $eventData["ProcessName"]
        ProcessID                 = $eventData["ProcessID"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            IpAddress    IpPort WorkstationName TargetDomainName TargetUserName LogonType FailureReason ProcessName                                                                             ProcessID
----                --------            ---------    ------ --------------- ---------------- -------------- --------- ------------- -----------                                                                             ---------
2022/08/31 17:38:01 tpl-packer          -            -      TPL-PACKER      TPL-PACKER       Administrator  2         %%2313        C:\Program Files (x86)\Microsoft\EdgeWebView\Application\90.0.818.66\msedgewebview2.exe 0x1cbc   
2022/08/31 17:40:23 tpl-packer          -            -      TPL-PACKER      TPL-PACKER       Administrator  2         %%2313        C:\Program Files (x86)\Microsoft\EdgeWebView\Application\90.0.818.66\msedgewebview2.exe 0xd88    
```

## 4648
数が多いので、一旦プロセス名でグループ化
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
		    #Computer                  = $xml.Event.System.Computer
        #IpAddress                 = $eventData["IpAddress"]
        #IpPort                    = $eventData["IpPort"]
        #SubjectLogonId            = $eventData["SubjectLogonId "]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        #SubjectDomainName         = $eventData["SubjectDomainName"]
        #SubjectUserName           = $eventData["SubjectUserName "]
        #TargetServerName          = $eventData["TargetServerName"]
        #TargetInfo                = $eventData["TargetInfo"]
        #TargetDomainName          = $eventData["TargetDomainName"]
        #TargetUserName            = $eventData["TargetUserName"]
        ProcessName               = $eventData["ProcessName"]
        #ProcessID                 = $eventData["ProcessID"]
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
        Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer                  = $xml.Event.System.Computer
        IpAddress                 = $eventData["IpAddress"]
        IpPort                    = $eventData["IpPort"]
        #SubjectLogonId            = $eventData["SubjectLogonId "]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        SubjectDomainName         = $eventData["SubjectDomainName"]
        SubjectUserName           = $eventData["SubjectUserName "]
        TargetServerName          = $eventData["TargetServerName"]
        TargetInfo                = $eventData["TargetInfo"]
        TargetDomainName          = $eventData["TargetDomainName"]
        TargetUserName            = $eventData["TargetUserName"]
        ProcessName               = $eventData["ProcessName"]
        ProcessID                 = $eventData["ProcessID"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer IpAddress IpPort SubjectDomainName SubjectUserName TargetServerName    TargetInfo          TargetDomainName TargetUserName ProcessName                                               ProcessID
----                -------- --------- ------ ----------------- --------------- ----------------    ----------          ---------------- -------------- -----------                                               ---------
2022/09/30 23:43:50 rd01     -         -      -                                 dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:51 rd01     -         -      -                                 dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
```


## 4672
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4672)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time               = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        #Computer           = $xml.Event.System.Computer
        #SubjectDomainName  = $eventData["SubjectDomainName"]
        SubjectUserName    = $eventData["SubjectUserName"]
    }
} | Where-Object {$_.SubjectUserName -ne "SYSTEM"} | Group-Object SubjectUserName | Format-Table -AutoSize -Property Values,Count

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

## 4688
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4688)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time               = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        #Computer           = $xml.Event.System.Computer
        #SubjectLogonId     = $eventData["SubjectLogonId"]
        #SubjectUserSid     = $eventData["SubjectUserSid"]
        #SubjectDomainName  = $eventData["SubjectDomainName"]
        #SubjectUserName    = $eventData["SubjectUserName"]
        #TargetLogonId      = $eventData["TargetLogonId"]
        #TargetUserSid      = $eventData["TargetUserSid"]
        #TargetDomainName   = $eventData["TargetDomainName"]
        #TargetUserName     = $eventData["TargetUserName"]
        #MandatoryLabel     = $eventData["MandatoryLabel"]
        #TokenElevationType = $eventData["TokenElevationType"]
        #ParentProcessId    = $eventData["ProcessId "]
        #ParentProcessName  = $eventData["ParentProcessName"]
        #NewProcessId       = $eventData["NewProcessId"]
        NewProcessName     = $eventData["NewProcessName"]
        #CommandLine        = $eventData["CommandLine"]
    }
} | Group-Object NewProcessName | Format-Table -AutoSize -Property Values,Count

Values                             Count
------                             -----
{C:\Windows\System32\lsass.exe}       26
{C:\Windows\System32\services.exe}    26
{C:\Windows\System32\winlogon.exe}    26
{C:\Windows\System32\csrss.exe}       52
{C:\Windows\System32\wininit.exe}     26
{C:\Windows\System32\smss.exe}        78
{C:\Windows\System32\autochk.exe}     26
{Registry}                            26
{C:\Windows\System32\setupcl.exe}      1
```


## 4692
dpapiアクセスを確認
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4692)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time              = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer          = $xml.Event.System.Computer
        SubjectLogonId    = $eventData["SubjectLogonId"]
        SubjectUserSid    = $eventData["SubjectUserSid"]
        SubjectDomainName = $eventData["SubjectDomainName"]
        SubjectUserName   = $eventData["SubjectUserName"]
        FailureReason     = $eventData["FailureReason"]
        MasterKeyId       = $eventData["MasterKeyId"]
        RecoveryServer    = $eventData["RecoveryServer"]
        RecoveryKeyId     = $eventData["RecoveryKeyId"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            SubjectLogonId SubjectUserSid                                 SubjectDomainName SubjectUserName FailureReason MasterKeyId                          RecoveryServer RecoveryKeyId                       
----                --------            -------------- --------------                                 ----------------- --------------- ------------- -----------                          -------------- -------------                       
2022/11/06 23:32:09 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a        0x80090345    7ff8a496-3871-4c2d-ad25-2ee9bcd363fe                                                    
2022/11/06 23:32:10 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a        0x80090345    44effb1e-f066-43fa-b032-c5602f4a5121                                                                                                  
```


## 4694
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4694)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time               = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer           = $xml.Event.System.Computer
        SubjectLogonId     = $eventData["SubjectLogonId"]
        SubjectUserSid     = $eventData["SubjectUserSid"]
        SubjectDomainName  = $eventData["SubjectDomainName"]
        SubjectUserName    = $eventData["SubjectUserName"]
        FailureReason      = $eventData["FailureReason"]
        MasterKeyId        = $eventData["MasterKeyId"]
        DataDescription    = $eventData["DataDescription"]
        ProtectedDataFlags = $eventData["ProtectedDataFlags"]
        CryptoAlgorithms   = $eventData["CryptoAlgorithms"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            SubjectLogonId SubjectUserSid                                 SubjectDomainName SubjectUserName FailureReason MasterKeyId DataDescription                      ProtectedDataFlags CryptoAlgorithms    
----                --------            -------------- --------------                                 ----------------- --------------- ------------- ----------- ---------------                      ------------------ ----------------    
2022/10/21 16:38:27 rd01.shieldbase.com 0x225ecb8      S-1-5-21-2908624845-2463485410-257172065-1001  RD01              SRLAdmin        0x2           Resync                                           0x20000000         AES-256 , SHA2-512  
2022/10/21 16:38:27 rd01.shieldbase.com 0x225ecd6      S-1-5-21-2908624845-2463485410-257172065-1001  RD01              SRLAdmin        0x2           Resync                                           0x20000000         AES-256 , SHA2-512  
```


### 4695
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4695)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time               = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer           = $xml.Event.System.Computer
        #SubjectLogonId     = $eventData["SubjectLogonId"]
        #SubjectUserSid     = $eventData["SubjectUserSid"]
        #SubjectDomainName  = $eventData["SubjectDomainName"]
        SubjectUserName    = $eventData["SubjectUserName"]
        FailureReason      = $eventData["FailureReason"]
        MasterKeyId        = $eventData["MasterKeyId"]
        DataDescription    = $eventData["DataDescription"]
        ProtectedDataFlags = $eventData["ProtectedDataFlags"]
        CryptoAlgorithms   = $eventData["CryptoAlgorithms"]
    }
} | Group-Object Computer,SubjectUserName,FailureReason,MasterKeyId,DataDescription,ProtectedDataFlags,CryptoAlgorithms | Format-Table -AutoSize -Property Values,Count

Values                                                                                                     Count
------                                                                                                     -----
{rd01.shieldbase.com, tdungan, 0x0, Edge, 3d0bc92f-bf43-432e-b348-b4310b829180, 0x0, 3DES-192 , SHA1-160 }   105
{rd01.shieldbase.com, wacsvc, 0x0, Edge, 7ce88ffb-d59c-4e66-9123-5ac40901cdb8, 0x0, 3DES-192 , SHA1-160 }     21
```
