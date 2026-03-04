## rd01
### security
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4624)] and EventData[Data[@Name='LogonType']=9]]" | ForEach-Object {
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
1/25/2023 3:07:50 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:58:29 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:57:21 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:56:56 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:56:42 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:55:06 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:54:59 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:54:14 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:54:00 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:53:05 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:52:42 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:52:04 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:51:13 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
1/25/2023 2:50:15 PM -        -      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\explorer.exe                                      0x1a08   
1/23/2023 6:17:00 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
1/23/2023 6:16:17 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
1/23/2023 3:14:05 PM ::1      0      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
1/23/2023 3:00:42 PM -        -      -                shieldbase       tdungan        shieldbase               wacsvc                 C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe 0xc8c
```
