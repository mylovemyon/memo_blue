## rd01
### security(4624)
tdunganから明示的なwacsvcログインを確認
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

Time                IpAddress IpPort WorkstationName TargetDomainName TargetUserName TargetOutboundDomainName TargetOutboundUserName ProcessName                                                  ProcessID
----                --------- ------ --------------- ---------------- -------------- ------------------------ ---------------------- -----------                                                  ---------
2023/01/23 15:00:42 -         -      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe 0xc8c    
2023/01/23 15:14:05 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
2023/01/23 18:16:17 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
2023/01/23 18:17:00 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
2023/01/25 14:50:15 -         -      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\explorer.exe                                      0x1a08   
2023/01/25 14:51:13 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:52:04 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:52:42 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:53:05 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:54:00 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:54:14 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:54:59 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:55:06 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:56:42 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:56:56 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:57:21 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:58:29 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 15:07:50 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 15:07:55 ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
```

### security(4625)
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

Time                LogonType FailureReason IpAddress    IpPort WorkstationName TargetDomainName TargetUserName LogonProcessName ProcessName                                                                             ProcessID
----                --------- ------------- ---------    ------ --------------- ---------------- -------------- ---------------- -----------                                                                             ---------
2022/08/31 17:38:01 2         %%2313        -            -      TPL-PACKER      TPL-PACKER       Administrator  Advapi           C:\Program Files (x86)\Microsoft\EdgeWebView\Application\90.0.818.66\msedgewebview2.exe 0x1cbc   
2022/08/31 17:40:23 2         %%2313        -            -      TPL-PACKER      TPL-PACKER       Administrator  Advapi           C:\Program Files (x86)\Microsoft\EdgeWebView\Application\90.0.818.66\msedgewebview2.exe 0xd88    
2022/10/21 16:38:19 11        %%2304        ::1          0      RD01            RD01             srladmin       CredPro          C:\Windows\System32\consent.exe                                                         0x828    
2022/10/21 16:38:19 2         %%2313        ::1          0      RD01            RD01             srladmin       CredPro          C:\Windows\System32\consent.exe                                                         0x828    
2022/10/21 16:38:27 11        %%2304        ::1          0      RD01            RD01             srladmin       CredPro          C:\Windows\System32\consent.exe                                                         0x828    
2023/01/02 22:54:24 3         %%2304        172.16.30.20 0      DUNGANATOR      shieldbase       tdungan        NtLmSsp          -                                                                                       0x0      
2023/01/02 22:55:06 3         %%2304        172.16.30.20 0      DUNGANATOR      shieldbase       tdungan        NtLmSsp          -                                                                                       0x0      
2023/01/02 22:55:29 3         %%2304        -            -      RD01            shieldbase       tdungan        Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/02 22:55:31 10        %%2313        172.16.30.20 0      RD01            shieldbase       tdungan        User32           C:\Windows\System32\svchost.exe                                                         0x888    
2023/01/03 21:53:49 3         %%2313        172.16.30.20 0      DUNGANATOR      shieldbase       tdungan        NtLmSsp          -                                                                                       0x0      
2023/01/03 21:54:02 3         %%2313        172.16.30.20 0      DUNGANATOR      shieldbase       tdungan        NtLmSsp          -                                                                                       0x0      
2023/01/17 14:41:59 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 14:49:51 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 14:50:31 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 15:26:20 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 15:26:51 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 20:30:15 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 20:31:27 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 21:34:00 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 21:47:56 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/19 14:27:56 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/19 18:45:34 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/19 18:48:08 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/19 20:25:23 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/23 20:52:47 3         %%2313        -            -      RD01                             sprx           Advapi           C:\Windows\System32\svchost.exe                                                         0x408
```

## security(4648)
プロセス名でグループ化
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
        #TargetDomainName  = $eventData["TargetDomainName"]
        #TargetUserName    = $eventData["TargetUserName"]
        #TargetUserSid     = $eventData["TargetUserSid"]
        ProcessName       = $eventData["ProcessName"]
        #ProcessId     = $eventData["ProcessId"]
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
powershell上で、dc01に対する管理者ログインを確認（ちなみにこのイベントIDのpowershell.exeはhayabusaでmimikatzと検知した）
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
        TargetDomainName  = $eventData["TargetDomainName"]
        TargetUserName    = $eventData["TargetUserName"]
        TargetUserSid     = $eventData["TargetUserSid"]
        ProcessName       = $eventData["ProcessName"]
        ProcessId         = $eventData["ProcessId"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Wrap -Property *

Time                SubjectDomainName SubjectUserName IpAddress IpPort TargetServerName    TargetDomainName TargetUserName TargetUserSid ProcessName                                               ProcessId
----                ----------------- --------------- --------- ------ ----------------    ---------------- -------------- ------------- -----------                                               ---------
2022/09/30 23:43:50 -                 -               -         -      dc01.shieldbase.com SHIELDBASE       srl.admin                    C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:51 -                 -               -         -      dc01.shieldbase.com SHIELDBASE       srl.admin                    C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:51 -                 -               -         -      dc01.shieldbase.com SHIELDBASE       srl.admin                    C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:53 -                 -               -         -      dc01.shieldbase.com SHIELDBASE       srl.admin                    C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:53 -                 -               -         -      dc01.shieldbase.com SHIELDBASE       srl.admin                    C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30
```
consent.exeとはuacポップアップ  
イベントID4624でconsent.exeログを複数確認したが、日時を見る限り失敗後にログイン成功していることがわかる
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)] and EventData[Data[@Name='ProcessName']='C:\Windows\System32\consent.exe']]" | ForEach-Object {
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
        TargetDomainName  = $eventData["TargetDomainName"]
        TargetUserName    = $eventData["TargetUserName"]
        TargetUserSid     = $eventData["TargetUserSid"]
        ProcessName       = $eventData["ProcessName"]
        ProcessId         = $eventData["ProcessId"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Wrap -Property *

Time                SubjectDomainName SubjectUserName IpAddress IpPort TargetServerName TargetDomainName TargetUserName TargetUserSid ProcessName                     ProcessId
----                ----------------- --------------- --------- ------ ---------------- ---------------- -------------- ------------- -----------                     ---------
2022/10/21 16:38:27 shieldbase        RD01$           ::1       0      localhost        RD01             SRLAdmin                     C:\Windows\System32\consent.exe 0x828    
2023/01/05 21:41:01 shieldbase        RD01$           ::1       0      localhost        shieldbase       tdungan                      C:\Windows\System32\consent.exe 0x2904
```
