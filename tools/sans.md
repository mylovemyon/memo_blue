## rd01

### security(4624)
tdunganから明示的なwacsvc認証を確認
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4624)] and EventData[Data[@Name='LogonType']=9]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
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

### security(4648)
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
powershell上で、dc01に対する管理者認証を確認（ちなみにこのイベントIDのpowershell.exeはhayabusaでmimikatzと検知した）
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
consent.exeとはuacポップアップ  
イベントID4625でconsent.exeログを複数確認したが、日時を見る限り失敗後にログイン成功していることがわかる
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
        TargetInfo        = $eventData["TargetInfo"]
        TargetDomainName  = $eventData["TargetDomainName"]
        TargetUserName    = $eventData["TargetUserName"]
        #TargetUserSid     = $eventData["TargetUserSid"]
        ProcessName       = $eventData["ProcessName"]
        ProcessId         = $eventData["ProcessId"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Wrap -Property *

Time                SubjectDomainName SubjectUserName IpAddress IpPort TargetServerName TargetInfo TargetDomainName TargetUserName ProcessName                     ProcessId
----                ----------------- --------------- --------- ------ ---------------- ---------- ---------------- -------------- -----------                     ---------
2022/10/21 16:38:27 shieldbase        RD01$           ::1       0      localhost        localhost  RD01             SRLAdmin       C:\Windows\System32\consent.exe 0x828    
2023/01/05 21:41:01 shieldbase        RD01$           ::1       0      localhost        localhost  shieldbase       tdungan        C:\Windows\System32\consent.exe 0x2904
```
dc01に対してrdp認証を確認
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)] and EventData[Data[@Name='ProcessName']='C:\Windows\System32\lsass.exe']]" | ForEach-Object {
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

Time                SubjectDomainName SubjectUserName IpAddress IpPort TargetServerName     TargetInfo                   TargetDomainName TargetUserName ProcessName                   ProcessId
----                ----------------- --------------- --------- ------ ----------------     ----------                   ---------------- -------------- -----------                   ---------
2023/01/18 14:55:11 shieldbase        wacsvc          -         -      dev01.shieldbase.com TERMSRV/dev01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\lsass.exe 0x2bc    
2023/01/18 14:55:11 shieldbase        wacsvc          -         -      dev01.shieldbase.com TERMSRV/dev01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\lsass.exe 0x2bc 
```
横展開しているね
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)] and EventData[Data[@Name='ProcessName']='C:\Windows\System32\wbem\WMIC.exe']]" | ForEach-Object {
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

Time                SubjectDomainName SubjectUserName IpAddress   IpPort TargetServerName       TargetInfo                  TargetDomainName TargetUserName ProcessName                       ProcessId
----                ----------------- --------------- ---------   ------ ----------------       ----------                  ---------------- -------------- -----------                       ---------
2023/01/23 15:14:06 shieldbase        tdungan         172.16.7.11 51022  wkstn01.shieldbase.com wkstn01.shieldbase.com      shieldbase       wacsvc         C:\Windows\System32\wbem\WMIC.exe 0x218c   
2023/01/23 15:14:06 shieldbase        tdungan         172.16.7.11 51022  wkstn01.shieldbase.com host/wkstn01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\wbem\WMIC.exe 0x218c
```
svchostでも通信ありの怪しいものを発見
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)] and EventData[Data[@Name='ProcessName']='C:\Windows\System32\svchost.exe']]" | ForEach-Object {
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
} | Where-Object {$_.IpAddress -like "*172*"} | Sort-Object Time | Format-Table -AutoSize -Wrap -Property *

Time                SubjectDomainName SubjectUserName IpAddress    IpPort TargetServerName       TargetInfo                   TargetDomainName TargetUserName ProcessName                     ProcessId
----                ----------------- --------------- ---------    ------ ----------------       ----------                   ---------------- -------------- -----------                     ---------
2022/09/30 23:44:12 -                 -               172.16.4.4   49668  dc01.shieldbase.com    dc01.shieldbase.com          SHIELDBASE       srl.admin      C:\Windows\System32\svchost.exe 0x7bc    
2022/10/01 14:55:12 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xb74    
2022/10/20 01:51:51 shieldbase        RD01$           172.16.30.6  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x604    
2022/10/20 01:54:35 shieldbase        RD01$           172.16.30.6  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x598    
2022/10/20 03:02:24 shieldbase        RD01$           172.16.30.6  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5d4    
2022/10/20 18:41:16 shieldbase        RD01$           172.16.30.6  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x644    
2022/10/21 16:58:19 shieldbase        RD01$           172.16.30.6  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5ec    
2022/10/21 17:07:26 shieldbase        RD01$           172.16.30.6  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5dc    
2022/10/29 14:13:12 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5dc    
2022/11/06 21:23:04 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5dc    
2022/11/07 14:58:40 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5dc    
2022/11/13 12:51:13 shieldbase        RD01$           172.16.30.7  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/16 17:57:53 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/17 19:55:33 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/18 18:53:55 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/21 17:20:21 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/22 16:28:07 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/22 20:31:44 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/24 17:30:49 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/01 19:34:18 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/01 19:36:13 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/01 19:50:39 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/04 12:25:19 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/04 14:34:36 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/05 14:32:23 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/05 20:11:03 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/07 19:52:03 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/12 10:33:26 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/12 14:07:14 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/14 14:17:12 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/14 14:31:01 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/14 15:33:32 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/16 17:35:28 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/20 21:03:58 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/21 21:54:42 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:01:46 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:06:08 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:07:16 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:08:13 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:32:32 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 15:51:50 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 16:26:16 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 16:34:54 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 16:37:40 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/24 05:51:02 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/26 21:22:44 shieldbase        RD01$           172.16.30.4  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2023/01/02 18:01:15 shieldbase        RD01$           172.16.30.20 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2023/01/02 22:56:33 shieldbase        RD01$           172.16.30.20 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/03 21:54:16 shieldbase        RD01$           172.16.30.20 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/04 04:35:14 shieldbase        RD01$           172.16.30.20 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/05 20:49:49 shieldbase        RD01$           172.16.30.20 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/09 16:57:25 shieldbase        RD01$           172.16.30.20 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/12 06:19:43 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/12 20:40:54 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/13 18:32:16 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/17 00:12:38 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/17 04:21:56 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/17 14:43:03 shieldbase        RD01$           172.16.6.18  0      localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/17 23:30:56 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/18 14:50:19 shieldbase        RD01$           172.16.6.18  0      localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/18 15:26:39 shieldbase        RD01$           172.16.6.18  0      localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/18 20:32:06 shieldbase        RD01$           172.16.6.18  0      localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/18 21:48:12 shieldbase        RD01$           172.16.6.18  0      localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/19 02:58:37 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/19 14:28:14 shieldbase        RD01$           172.16.6.18  0      localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/19 14:53:02 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/19 18:02:01 shieldbase        RD01$           172.16.30.3  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/22 23:31:01 shieldbase        RD01$           172.16.30.14 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/23 14:37:29 shieldbase        RD01$           172.16.30.14 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/23 14:52:56 shieldbase        RD01$           172.16.30.14 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x628    
2023/01/23 15:14:05 shieldbase        tdungan         172.16.7.11  135    wkstn01.shieldbase.com RPCSS/wkstn01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\svchost.exe 0x3b4    
2023/01/23 20:53:05 shieldbase        RD01$           172.16.4.9   0      localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x628    
2023/01/24 03:21:30 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x628    
2023/01/24 14:17:13 shieldbase        RD01$           172.16.30.8  0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x628    
2023/01/25 14:19:26 shieldbase        RD01$           172.16.30.23 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x60c    
2023/01/25 14:38:50 shieldbase        RD01$           172.16.30.23 0      localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5f8
```

### security(4672)
件数多いので、ユーザ名でグループ化、まあまあ時間かかる
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
{DWM-2}              25
{LOCAL SERVICE}      24
{DWM-1}              42
{NETWORK SERVICE}    24
{cbarton-a}          26
{DWM-5}              10
{DWM-4}               8
{DWM-3}              42
{DWM-15}             10
{rsydow-a}          818
{DWM-14}             20
{DWM-13}              2
{DWM-12}              2
{DWM-11}              8
{DWM-10}              4
{DWM-9}               8
{DWM-8}              16
{DWM-7}              12
{DWM-6}              14
{SRLAdmin}            1
{Administrator}     625
{defaultuser0}        2
```
