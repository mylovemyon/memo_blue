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
        Computer                  = $xml.Event.System.Computer
        IpAddress                 = $eventData["IpAddress"]
        IpPort                    = $eventData["IpPort"]
        WorkstationName           = $eventData["WorkstationName"]
        #SubjectLogonId            = $eventData["SubjectLogonId"]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        #SubjectDomainName         = $eventData["SubjectDomainName"]
        #SubjectUserName           = $eventData["SubjectUserName"]
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
2023/01/23 18:16:17 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
2023/01/23 18:17:00 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x1cb8   
2023/01/25 14:50:15 rd01.shieldbase.com -         -      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\explorer.exe                                      0x1a08   
2023/01/25 14:51:13 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:52:04 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:52:42 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:53:05 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:54:00 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:54:14 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:54:59 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:55:06 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:56:42 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:56:56 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:57:21 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 14:58:29 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 15:07:50 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410    
2023/01/25 15:07:55 rd01.shieldbase.com ::1       0      -               shieldbase       tdungan        shieldbase               wacsvc                 C:\Windows\System32\svchost.exe                              0x410 
```

### security(4625)
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
        #SubjectLogonId            = $eventData["SubjectLogonId"]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        #SubjectDomainName         = $eventData["SubjectDomainName"]
        #SubjectUserName           = $eventData["SubjectUserName"]
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
2022/10/21 16:38:19 rd01.shieldbase.com ::1          0      RD01            RD01             srladmin       11        %%2304        C:\Windows\System32\consent.exe                                                         0x828    
2022/10/21 16:38:19 rd01.shieldbase.com ::1          0      RD01            RD01             srladmin       2         %%2313        C:\Windows\System32\consent.exe                                                         0x828    
2022/10/21 16:38:27 rd01.shieldbase.com ::1          0      RD01            RD01             srladmin       11        %%2304        C:\Windows\System32\consent.exe                                                         0x828    
2023/01/02 22:54:24 rd01.shieldbase.com 172.16.30.20 0      DUNGANATOR      shieldbase       tdungan        3         %%2304        -                                                                                       0x0      
2023/01/02 22:55:06 rd01.shieldbase.com 172.16.30.20 0      DUNGANATOR      shieldbase       tdungan        3         %%2304        -                                                                                       0x0      
2023/01/02 22:55:29 rd01.shieldbase.com -            -      RD01            shieldbase       tdungan        3         %%2304        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/02 22:55:31 rd01.shieldbase.com 172.16.30.20 0      RD01            shieldbase       tdungan        10        %%2313        C:\Windows\System32\svchost.exe                                                         0x888    
2023/01/03 21:53:49 rd01.shieldbase.com 172.16.30.20 0      DUNGANATOR      shieldbase       tdungan        3         %%2313        -                                                                                       0x0      
2023/01/03 21:54:02 rd01.shieldbase.com 172.16.30.20 0      DUNGANATOR      shieldbase       tdungan        3         %%2313        -                                                                                       0x0      
2023/01/17 14:41:59 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 14:49:51 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 14:50:31 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 15:26:20 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 15:26:51 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 20:30:15 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 20:31:27 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 21:34:00 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/18 21:47:56 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/19 14:27:56 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/19 18:45:34 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/19 18:48:08 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/19 20:25:23 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x30c    
2023/01/23 20:52:47 rd01.shieldbase.com -            -      RD01                             sprx           3         %%2313        C:\Windows\System32\svchost.exe                                                         0x408  
```

### security(4648)
プロセス名でグループ化
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
        #SubjectLogonId            = $eventData["SubjectLogonId"]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        #SubjectDomainName         = $eventData["SubjectDomainName"]
        #SubjectUserName           = $eventData["SubjectUserName"]
        #TargetServerName          = $eventData["TargetServerName"]
        #TargetInfo                = $eventData["TargetInfo"]
        #TargetDomainName          = $eventData["TargetDomainName"]
        #TargetUserName            = $eventData["TargetUserName"]
        ProcessName               = $eventData["ProcessName"]
        #ProcessID                 = $eventData["ProcessID"]
    }
} | Group-Object ProcessName | Format-Table -AutoSize -Property Values,Count

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
        Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer                  = $xml.Event.System.Computer
        IpAddress                 = $eventData["IpAddress"]
        IpPort                    = $eventData["IpPort"]
        #SubjectLogonId            = $eventData["SubjectLogonId"]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        SubjectDomainName         = $eventData["SubjectDomainName"]
        SubjectUserName           = $eventData["SubjectUserName"]
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
2022/09/30 23:43:50 rd01     -         -      -                 -               dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:51 rd01     -         -      -                 -               dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:51 rd01     -         -      -                 -               dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:53 rd01     -         -      -                 -               dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:53 rd01     -         -      -                 -               dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
```
consent.exeとはuacポップアップ  
イベントID4625でconsent.exeログを複数確認したが、日時を見る限り失敗後にログイン成功していることがわかる
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)] and EventData[Data[@Name='ProcessName']='C:\Windows\System32\consent.exe']]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer                  = $xml.Event.System.Computer
        IpAddress                 = $eventData["IpAddress"]
        IpPort                    = $eventData["IpPort"]
        #SubjectLogonId            = $eventData["SubjectLogonId"]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        SubjectDomainName         = $eventData["SubjectDomainName"]
        SubjectUserName           = $eventData["SubjectUserName"]
        TargetServerName          = $eventData["TargetServerName"]
        TargetInfo                = $eventData["TargetInfo"]
        TargetDomainName          = $eventData["TargetDomainName"]
        TargetUserName            = $eventData["TargetUserName"]
        ProcessName               = $eventData["ProcessName"]
        ProcessID                 = $eventData["ProcessID"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            IpAddress IpPort SubjectDomainName SubjectUserName TargetServerName TargetInfo TargetDomainName TargetUserName ProcessName                     ProcessID
----                --------            --------- ------ ----------------- --------------- ---------------- ---------- ---------------- -------------- -----------                     ---------
2022/10/21 16:38:27 rd01.shieldbase.com ::1       0      shieldbase        RD01$           localhost        localhost  RD01             SRLAdmin       C:\Windows\System32\consent.exe 0x828    
2023/01/05 21:41:01 rd01.shieldbase.com ::1       0      shieldbase        RD01$           localhost        localhost  shieldbase       tdungan        C:\Windows\System32\consent.exe 0x2904   
```
dc01に対してrdp認証を確認
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)] and EventData[Data[@Name='ProcessName']='C:\Windows\System32\lsass.exe']]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer                  = $xml.Event.System.Computer
        IpAddress                 = $eventData["IpAddress"]
        IpPort                    = $eventData["IpPort"]
        #SubjectLogonId            = $eventData["SubjectLogonId"]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        SubjectDomainName         = $eventData["SubjectDomainName"]
        SubjectUserName           = $eventData["SubjectUserName"]
        TargetServerName          = $eventData["TargetServerName"]
        TargetInfo                = $eventData["TargetInfo"]
        TargetDomainName          = $eventData["TargetDomainName"]
        TargetUserName            = $eventData["TargetUserName"]
        ProcessName               = $eventData["ProcessName"]
        ProcessID                 = $eventData["ProcessID"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            IpAddress IpPort SubjectDomainName SubjectUserName TargetServerName     TargetInfo                   TargetDomainName TargetUserName ProcessName                   ProcessID
----                --------            --------- ------ ----------------- --------------- ----------------     ----------                   ---------------- -------------- -----------                   ---------
2023/01/18 14:55:11 rd01.shieldbase.com -         -      shieldbase        wacsvc          dev01.shieldbase.com TERMSRV/dev01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\lsass.exe 0x2bc    
2023/01/18 14:55:11 rd01.shieldbase.com -         -      shieldbase        wacsvc          dev01.shieldbase.com TERMSRV/dev01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\lsass.exe 0x2bc    
```
横展開しているね
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)] and EventData[Data[@Name='ProcessName']='C:\Windows\System32\wbem\WMIC.exe']]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer                  = $xml.Event.System.Computer
        IpAddress                 = $eventData["IpAddress"]
        IpPort                    = $eventData["IpPort"]
        #SubjectLogonId            = $eventData["SubjectLogonId"]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        SubjectDomainName         = $eventData["SubjectDomainName"]
        SubjectUserName           = $eventData["SubjectUserName"]
        TargetServerName          = $eventData["TargetServerName"]
        TargetInfo                = $eventData["TargetInfo"]
        TargetDomainName          = $eventData["TargetDomainName"]
        TargetUserName            = $eventData["TargetUserName"]
        ProcessName               = $eventData["ProcessName"]
        ProcessID                 = $eventData["ProcessID"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            IpAddress   IpPort SubjectDomainName SubjectUserName TargetServerName       TargetInfo                  TargetDomainName TargetUserName ProcessName                       ProcessID
----                --------            ---------   ------ ----------------- --------------- ----------------       ----------                  ---------------- -------------- -----------                       ---------
2023/01/23 15:14:06 rd01.shieldbase.com 172.16.7.11 51022  shieldbase        tdungan         wkstn01.shieldbase.com wkstn01.shieldbase.com      shieldbase       wacsvc         C:\Windows\System32\wbem\WMIC.exe 0x218c   
2023/01/23 15:14:06 rd01.shieldbase.com 172.16.7.11 51022  shieldbase        tdungan         wkstn01.shieldbase.com host/wkstn01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\wbem\WMIC.exe 0x218c   
```
svchostでも通信ありの怪しいものを発見
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4648)] and EventData[Data[@Name='ProcessName']='C:\Windows\System32\svchost.exe']]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                      = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer                  = $xml.Event.System.Computer
        IpAddress                 = $eventData["IpAddress"]
        IpPort                    = $eventData["IpPort"]
        #SubjectLogonId            = $eventData["SubjectLogonId"]
        #SubjectUserSid            = $eventData["SubjectUserSid"]
        SubjectDomainName         = $eventData["SubjectDomainName"]
        SubjectUserName           = $eventData["SubjectUserName"]
        TargetServerName          = $eventData["TargetServerName"]
        TargetInfo                = $eventData["TargetInfo"]
        TargetDomainName          = $eventData["TargetDomainName"]
        TargetUserName            = $eventData["TargetUserName"]
        ProcessName               = $eventData["ProcessName"]
        ProcessID                 = $eventData["ProcessID"]
    }
} | Where-Object {$_.IpAddress -like "*172*"} | Group-Object Computer,IpAddress,IpPort,SubjectUserName,TargetServerName,TargetInfo,TargetDomainName,TargetUserName,ProcessName | Format-Table -AutoSize -Property Values,Count

Values                                                                                                                                                          Count
------                                                                                                                                                          -----
{rd01.shieldbase.com, 172.16.30.23, 0, RD01$, localhost, localhost, shieldbase, tdungan, C:\Windows\System32\svchost.exe}                                           2
{rd01.shieldbase.com, 172.16.30.8, 0, RD01$, localhost, localhost, shieldbase, tdungan, C:\Windows\System32\svchost.exe}                                           19
{rd01.shieldbase.com, 172.16.4.9, 0, RD01$, localhost, localhost, shieldbase, wacsvc, C:\Windows\System32\svchost.exe}                                              1
{rd01.shieldbase.com, 172.16.7.11, 135, tdungan, wkstn01.shieldbase.com, RPCSS/wkstn01.shieldbase.com, SHIELDBASE.COM, wacsvc, C:\Windows\System32\svchost.exe}     1
{rd01.shieldbase.com, 172.16.30.14, 0, RD01$, localhost, localhost, shieldbase, tdungan, C:\Windows\System32\svchost.exe}                                           3
{rd01.shieldbase.com, 172.16.30.3, 0, RD01$, localhost, localhost, shieldbase, tdungan, C:\Windows\System32\svchost.exe}                                           11
{rd01.shieldbase.com, 172.16.6.18, 0, RD01$, localhost, localhost, shieldbase, wacsvc, C:\Windows\System32\svchost.exe}                                             6
{rd01.shieldbase.com, 172.16.30.20, 0, RD01$, localhost, localhost, shieldbase, tdungan, C:\Windows\System32\svchost.exe}                                           6
{rd01.shieldbase.com, 172.16.30.4, 0, RD01$, localhost, localhost, shieldbase, tdungan, C:\Windows\System32\svchost.exe}                                           19
{rd01.shieldbase.com, 172.16.30.7, 0, RD01$, localhost, localhost, shieldbase, tdungan, C:\Windows\System32\svchost.exe}                                            1
{rd01.shieldbase.com, 172.16.30.6, 0, RD01$, localhost, localhost, shieldbase, tdungan, C:\Windows\System32\svchost.exe}                                            6
{rd01, 172.16.4.4, 49668, -, dc01.shieldbase.com, dc01.shieldbase.com, SHIELDBASE, srl.admin, C:\Windows\System32\svchost.exe}
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
        #Computer          = $xml.Event.System.Computer
        #SubjectDomainName = $eventData["SubjectDomainName"]
        SubjectUserName    = $eventData["SubjectUserName"]
    }
} | Where-Object {$_.SubjectUserName -ne "SYSTEM"} | Group-Object SubjectUserName | Format-Table -AutoSize -Property Values,Count

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

### security(4688)
あんまログとれてなさそうね
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
        #ProcessId          = $eventData["ProcessId"]
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


### security(4692)
dpapiにアクセスしているね
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
2022/11/06 23:32:10 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a        0x80090345    8367f31a-23cc-41ec-92e9-cc34bdaef213                                                    
2023/01/02 18:01:26 rd01.shieldbase.com 0x26a9c239     S-1-5-21-2838623409-1327563992-2591358621-1136 shieldbase        tdungan         0x0           23027cbe-10ef-4c1c-9827-15c1511d33fe                5a29a8b3-26f3-42f5-8abd-854ce52aa72f
2023/01/17 14:43:25 rd01.shieldbase.com 0x1f84faa1     S-1-5-21-2838623409-1327563992-2591358621-1220 shieldbase        wacsvc          0x0           7ce88ffb-d59c-4e66-9123-5ac40901cdb8                5a29a8b3-26f3-42f5-8abd-854ce52aa72f
```


### security(4694)
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
2022/11/06 23:32:09 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a        0x80090345    Export Flag                                      0x0                3DES-192 , SHA1-160 
2022/11/06 23:32:10 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a        0x80090345    Export Flag                                      0x0                3DES-192 , SHA1-160 
2022/11/06 23:32:10 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a        0x80090345    Export Flag                                      0x0                3DES-192 , SHA1-160 
2023/01/17 14:43:38 rd01.shieldbase.com 0x1f84faa1     S-1-5-21-2838623409-1327563992-2591358621-1220 shieldbase        wacsvc          0x0           Edge        7ce88ffb-d59c-4e66-9123-5ac40901cdb8 0x10               3DES-192 , SHA1-160 
2023/01/17 14:45:41 rd01.shieldbase.com 0x1f84faa1     S-1-5-21-2838623409-1327563992-2591358621-1220 shieldbase        wacsvc          0x0           Edge        7ce88ffb-d59c-4e66-9123-5ac40901cdb8 0x10               3DES-192 , SHA1-160 
2023/01/17 14:50:37 rd01.shieldbase.com 0x1f84faa1     S-1-5-21-2838623409-1327563992-2591358621-1220 shieldbase        wacsvc          0x0           Edge        7ce88ffb-d59c-4e66-9123-5ac40901cdb8 0x10               3DES-192 , SHA1-160 
```


### security(4695)
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
        SubjectDomainName  = $eventData["SubjectDomainName"]
        SubjectUserName    = $eventData["SubjectUserName"]
        FailureReason      = $eventData["FailureReason"]
        MasterKeyId        = $eventData["MasterKeyId"]
        DataDescription    = $eventData["DataDescription"]
        ProtectedDataFlags = $eventData["ProtectedDataFlags"]
        CryptoAlgorithms   = $eventData["CryptoAlgorithms"]
    }
} | Group-Object Computer,SubjectDomainName,SubjectUserName,FailureReason,MasterKeyId,DataDescription,ProtectedDataFlags,CryptoAlgorithms | Format-Table -AutoSize -Property Values,Count

Values                                                                                                                 Count
------                                                                                                                 -----
{rd01.shieldbase.com, shieldbase, tdungan, 0x0, Edge, 3d0bc92f-bf43-432e-b348-b4310b829180, 0x0, 3DES-192 , SHA1-160 }   105
{rd01.shieldbase.com, shieldbase, wacsvc, 0x0, Edge, 7ce88ffb-d59c-4e66-9123-5ac40901cdb8, 0x0, 3DES-192 , SHA1-160 }     21
```


### security(4696)
大したログはなさそう
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4696)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time              = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer          = $xml.Event.System.Computer
        #SubjectLogonId    = $eventData["SubjectLogonId"]
        #SubjectUserSid    = $eventData["SubjectUserSid"]
        SubjectDomainName = $eventData["SubjectDomainName"]
        SubjectUserName   = $eventData["SubjectUserName"]
        #TargetLogonId     = $eventData["TargetLogonId"]
        #TargetUserSid     = $eventData["TargetUserSid"]
        TargetDomainName  = $eventData["TargetDomainName"]
        TargetUserName    = $eventData["TargetUserName"]
        #TargetProcessId   = $eventData["TargetProcessId"]
        TargetProcessName = $eventData["TargetProcessName"]
        #ProcessId         = $eventData["ProcessId"]
        ProcessName       = $eventData["ProcessName"]
    }
} | Group-Object Computer,SubjectDomainName,SubjectUserName,TargetDomainName,TargetUserName,TargetProcessName,ProcessName | Format-Table -AutoSize -Property Values,Count

Values                                             Count
------                                             -----
{rd01.shieldbase.com, -, -, -, -, Registry, $null}    18
{rd01, -, -, -, -, Registry, $null}                    1
{tpl-packer, -, -, -, -, Registry, $null}              6
{OFFDEVS-TUHMGJE, -, -, -, -, Registry, $null}         1
```


### security(4697)
怪しいサービス登録はたぶんなさそう
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4697)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time              = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer          = $xml.Event.System.Computer
        #SubjectLogonId    = $eventData["SubjectLogonId"]
        #SubjectUserSid    = $eventData["SubjectUserSid"]
        #SubjectDomainName = $eventData["SubjectDomainName"]
        #SubjectUserName   = $eventData["SubjectUserName"]
        #ServiceType       = $eventData["ServiceType"]
        #ServiceStartType  = $eventData["ServiceStartType"]
        #ServiceName       = $eventData["ServiceName"]
        ServiceFileName   = $eventData["ServiceFileName"]
        #ServiceAccount    = $eventData["ServiceAccount"]
    }
} | Group-Object Computer,ServiceFileName | Format-Table -AutoSize -Property Values,Count

Values                                                                                                                                             Count
------                                                                                                                                             -----
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k UnistackSvcGroup}                                                                           273
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k UdkSvcGroup}                                                                                 39
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k PrintWorkflow}                                                                               39
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k PenService}                                                                                  39
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k P9RdrService -p}                                                                             39
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k LocalService -p}                                                                             78
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k DevicesFlow}                                                                                117
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k DevicesFlow -p}                                                                              39
{rd01.shieldbase.com, C:\Windows\system32\CredentialEnrollmentManager.exe}                                                                            39
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k ClipboardSvcGroup -p}                                                                        39
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k BthAppGroup -p}                                                                              39
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k BcastDVRUserService}                                                                         39
{rd01.shieldbase.com, C:\Windows\system32\svchost.exe -k AarSvcGroup -p}                                                                              39
{rd01.shieldbase.com, C:\windows/Mnemosyne.sys}                                                                                                        2
{rd01.shieldbase.com, "C:\windows\subject_srv.exe" -s "172.16.5.25:5682" -l 3262 -v "F-Response Subject Service" -k "155522845"}                       1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{1FEC7FC9-D47D-4480-85E5-AE4DD6CCA988}\MpKslDrv.sys}                1
{rd01.shieldbase.com, "C:\Program Files\Amazon\Ec2ConfigService\Ec2Config.exe"}                                                                        1
{rd01.shieldbase.com, C:\Windows\system32\MpEngineStore\MpKslDrv.sys}                                                                                 17
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{A0638AC4-7F42-4619-900A-6971E7AD116E}\MpKslDrv.sys}                1
{rd01.shieldbase.com, "C:\Program Files\Velociraptor\Velociraptor.exe"  --config "C:\Program Files\Velociraptor\/client.config.yaml" service run }     1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{24528CF9-8D64-474D-9D9C-30AFB08D4D67}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{86516CAC-6376-4FBF-A7FA-192676A321FE}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{32A2C6DB-0411-4CFC-B3E6-70DDA2F015A8}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{21D418C2-5C25-4FC2-8CEF-15E5F856D979}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{3246E6BB-2507-479C-8B17-2BA91F1A0BE3}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{1D3FF1B2-7860-4BD2-AEC3-E7D91260AEFB}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{F9A8FA83-EBB6-4FD2-B714-24AFD2F898E2}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{B58C05AE-3427-477D-B3AC-0788B8A81BB7}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{7324D37E-F38A-459F-B066-88930B9D0F12}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{B004DCDD-2BE8-4CD2-896B-64E532B4C741}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{77C4C2F2-51B5-497E-9864-A777038D47E1}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{D862184F-8E06-4A7C-89C1-2A1E2E974DC9}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{4DF489B5-2633-4895-B7E0-623B637F6629}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{FF164DF6-19E2-4853-BEEA-E60ACB80BEDE}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{CE3A3E64-98AF-4E1C-841B-5C065174CD8C}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{FB7350FB-29F9-4741-A2DA-465467FA2264}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{8A717F01-7A5D-431A-979D-7C6B8D58CA92}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{236972CC-D79A-4B14-AB1D-CDC9498BF155}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{73919D2E-CDE5-424D-A375-3BA5FEF0FE1E}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{BCB20E31-B458-42D0-9404-F41D8B687600}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{9D00F7BA-5F6E-4647-9F5A-89CBCD782C75}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{5F3D24C1-3115-45E2-B9A1-BDBD56760597}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{1884BE8D-ABD1-43E7-AD40-1D211AB01B88}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{1D4155F3-64B8-4676-8F87-E2DF09A954D4}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{CFFAD193-E012-4461-B3A8-554909F7D28F}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{4C8C2B94-A2DC-4E16-AF23-D79AD151C9BA}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{46C40000-85F3-4561-9963-6C4D4C853D3B}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{DB5BA17F-5CB1-4633-B20D-B81F971A10E3}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{839DFD5E-3419-429A-B13E-43A2263DF7E3}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{66BE480C-5B8F-4B85-8E29-59EAA304A6E6}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{CE2729BA-DA1B-4FC9-9BD1-5CAE17CDCB5E}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{B48F4E00-ACC4-4E88-9DFB-E5D6A53AB858}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{607F7734-C10D-4D56-9647-4DF2551A34D8}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{17D75271-95E9-4480-B556-1203DFEF3562}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{7CD272AC-2EA6-47B9-9753-E8EA180BDA70}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{6FF8CABC-D3FB-472F-934F-F653B6D56D39}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{4D31E84D-5E0D-49C7-8DC7-C94A636BCF51}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{98088365-28B3-4556-BF96-FF16FF44CA7A}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{35295CA9-D440-417B-ADBB-FAAB0C230A37}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{2E685413-DCC6-4616-943C-BAE6442253E7}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{E0ABBCCF-B2C7-4DCE-BA69-0B9C27D17FF4}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{EADA6ABC-205A-48A7-9EDF-D3A10D2E2BE0}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{1C019127-C96D-4A6A-8331-79947712D532}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{F28510DE-C4F8-4146-851A-3F2F101AD8EB}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{C16190E6-8F8D-4050-88FF-E03B8D95DB5C}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{CBF1CA42-B2DE-42A8-82FC-FAF59D67592B}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{7792244A-7A1B-4AB9-9A41-4F3A219175DF}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{2ACF8CCF-495B-49F9-A474-627B0EC268DE}\MpKslDrv.sys}                1
{rd01.shieldbase.com, "C:\Program Files (x86)\Foxit Software\Foxit PDF Reader\FoxitPDFReaderUpdateService.exe"}                                        1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{40DB57DA-113E-4963-94F5-7705059A29C6}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{B3B47647-DB65-4924-B5BF-F3929FE1003A}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{DFB0F502-63D6-48C4-980C-52BE1008A79A}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{38181567-E532-47BA-BB3D-79DABFA5C8F9}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{AB5E3EE9-F100-42CB-8F06-8D5EA8A9F769}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{04926DA5-A48D-4603-825E-8F5DC7789F7D}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{4783882F-8630-471A-AA0A-19CEA184B3E1}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{B39D94ED-7528-4EBB-ADA2-F3FA59B44EAB}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{9C6D2B05-6327-4FA6-A3C8-2CDC93BAC8CB}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{CF3833AE-C3A6-49FB-9338-1F55FC7CEB20}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{DBA063FF-F85F-4CE5-B775-639FA8F871A2}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{8733F3B8-4BEB-4221-BBC9-9A9F80DFF621}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{2A2C8C59-8D56-4DAD-8644-8E4D3B527730}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{2E004DB6-C30F-49BC-8A98-06F13B6F464E}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{C6A92A7F-AB09-42FA-B1D0-8C44ACDE6B1B}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{98415CBF-4B52-4B5D-B40C-1461D1632BC6}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{AD6B2DB7-944A-4E0E-9A47-EFE45FE9EBF2}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{B6D5D1A1-237D-4483-ABF0-6CF6907CCAE3}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{D77CC778-81FD-4F28-822B-962E9AC1754E}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{F669AA27-6B0E-4E0B-80CE-6071F1969A37}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{05C40E94-20BE-43E1-9857-6F9288F4D341}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{1EAD4465-81D8-4836-9AD1-9A3093B8861A}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{D82DF3AC-CDB9-45F6-AC48-F309DDA76F54}\MpKslDrv.sys}                1
{rd01.shieldbase.com, C:\ProgramData\Microsoft\Windows Defender\Definition Updates\{DE440370-3AFA-40FF-91C3-2BF3DDF2AE36}\MpKslDrv.sys}                1
```


### security(4717)
関係ないログっぽい
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4717)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time              = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer          = $xml.Event.System.Computer
        #SubjectLogonId    = $eventData["SubjectLogonId"]
        #SubjectUserSid    = $eventData["SubjectUserSid"]
        SubjectDomainName = $eventData["SubjectDomainName"]
        SubjectUserName   = $eventData["SubjectUserName"]
        TargetSid         = $eventData["TargetSid"]
        AccessGranted     = $eventData["AccessGranted"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Computer        SubjectDomainName SubjectUserName TargetSid                                                      AccessGranted                
--------        ----------------- --------------- ---------                                                      -------------                
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-80-0                                                     SeServiceLogonRight          
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-555                                                   SeRemoteInteractiveLogonRight
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-21-2908624845-2463485410-257172065-501                   SeDenyNetworkLogonRight      
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-21-2908624845-2463485410-257172065-501                   SeDenyInteractiveLogonRight  
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-21-2908624845-2463485410-257172065-501                   SeInteractiveLogonRight      
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-559                                                   SeBatchLogonRight            
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-551                                                   SeBatchLogonRight            
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-551                                                   SeNetworkLogonRight          
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-544                                                   SeRemoteInteractiveLogonRight
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-80-3169285310-278349998-1452333686-3865143136-4212226833 SeServiceLogonRight          
```


### security(4718)
関係ないログっぽい
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4718)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        #Time              = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer          = $xml.Event.System.Computer
        #SubjectLogonId    = $eventData["SubjectLogonId"]
        #SubjectUserSid    = $eventData["SubjectUserSid"]
        SubjectDomainName = $eventData["SubjectDomainName"]
        SubjectUserName   = $eventData["SubjectUserName"]
        TargetSid         = $eventData["TargetSid"]
        AccessRemoved     = $eventData["AccessRemoved"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Computer        SubjectDomainName SubjectUserName TargetSid                                                      AccessRemoved                
--------        ----------------- --------------- ---------                                                      -------------                
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-90-0                                                     SeInteractiveLogonRight      
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-1-0                                                        SeRemoteInteractiveLogonRight
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-1-0                                                        SeInteractiveLogonRight      
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-80-3169285310-278349998-1452333686-3865143136-4212226833 SeServiceLogonRight          
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-583                                                   SeNetworkLogonRight          
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-583                                                   SeInteractiveLogonRight      
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-581                                                   SeNetworkLogonRight          
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-581                                                   SeInteractiveLogonRight      
OFFDEVS-TUHMGJE                   MINWINPC$       S-1-5-32-546                                                   SeInteractiveLogonRight      
```


### security(4720)
ローカルユーザ作成を確認
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4720)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer            = $xml.Event.System.Computer
        #SubjectLogonId      = $eventData["SubjectLogonId"]
        #SubjectUserSid      = $eventData["SubjectUserSid"]
        SubjectDomainName   = $eventData["SubjectDomainName"]
        SubjectUserName     = $eventData["SubjectUserName"]
        #TargetUserSid       = $eventData["TargetUserSid"]
        TargetDomainName    = $eventData["TargetDomainName"]
        TargetUserName      = $eventData["TargetUserName"]
        SamAccountName      = $eventData["SamAccountName"]
        DisplayName         = $eventData["DisplayName"]
        UserPrincipalName   = $eventData["UserPrincipalName"]
        HomeDirectory       = $eventData["HomeDirectory"]
        HomePath            = $eventData["HomePath"]
        ScriptPath          = $eventData["ScriptPath"]
        ProfilePath         = $eventData["ProfilePath"]
        UserWorkstations    = $eventData["UserWorkstations"]
        PasswordLastSet     = $eventData["PasswordLastSet"]
        AccountExpires      = $eventData["AccountExpires"]
        PrimaryGroupId      = $eventData["PrimaryGroupId"]
        AllowedToDelegateTo = $eventData["AllowedToDelegateTo"]
        OldUacValue         = $eventData["OldUacValue"]
        NewUacValue         = $eventData["NewUacValue"]
        SidHistory          = $eventData["SidHistory"]
        LogonHours          = $eventData["LogonHours"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer        SubjectDomainName SubjectUserName TargetDomainName TargetUserName     SamAccountName     DisplayName UserPrincipalName HomeDirectory HomePath ScriptPath ProfilePath UserWorkstations PasswordLastSet AccountExpires PrimaryGroupId AllowedToDelegateTo OldUacValue NewUacValue SidHistory LogonHours
----                --------        ----------------- --------------- ---------------- --------------     --------------     ----------- ----------------- ------------- -------- ---------- ----------- ---------------- --------------- -------------- -------------- ------------------- ----------- ----------- ---------- ----------
2022/08/31 19:34:47 OFFDEVS-TUHMGJE                   MINWINPC$       MINWINPC         WDAGUtilityAccount WDAGUtilityAccount %%1793      -                 %%1793        %%1793   %%1793     %%1793      %%1793           %%1794          %%1794         513            -                   0x0         0x15        -          %%1797    
2022/08/31 19:36:10 tpl-packer      WORKGROUP         TPL-PACKER$     TPL-PACKER       defaultuser0       defaultuser0       %%1793      -                 %%1793        %%1793   %%1793     %%1793      %%1793           %%1794          %%1794         513            -                   0x0         0x15        -          %%1797    
2022/09/30 23:38:54 rd01            WORKGROUP         RD01$           RD01             SRLAdmin           SRLAdmin           %%1793      -                 %%1793        %%1793   %%1793     %%1793      %%1793           %%1794          %%1794         513            -                   0x0         0x15        -          %%1797    
```


### security(4722)
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4722)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer            = $xml.Event.System.Computer
        #SubjectLogonId      = $eventData["SubjectLogonId"]
        #SubjectUserSid      = $eventData["SubjectUserSid"]
        SubjectDomainName   = $eventData["SubjectDomainName"]
        SubjectUserName     = $eventData["SubjectUserName"]
        #TargetUserSid       = $eventData["TargetUserSid"]
        TargetDomainName    = $eventData["TargetDomainName"]
        TargetUserName      = $eventData["TargetUserName"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer   SubjectDomainName SubjectUserName TargetDomainName TargetUserName
----                --------   ----------------- --------------- ---------------- --------------
2022/08/31 19:36:09 tpl-packer WORKGROUP         TPL-PACKER$     TPL-PACKER       Administrator 
2022/08/31 19:36:10 tpl-packer WORKGROUP         TPL-PACKER$     TPL-PACKER       defaultuser0  
2022/09/30 23:38:54 rd01       WORKGROUP         RD01$           RD01             SRLAdmin  
```


### security(4724)
sqladminのパスワードリセット試行を確認
```powershell
PS C:\Users\SANSDFIR> Get-WinEvent -Path .\Security.evtx -FilterXPath "*[System[(EventID=4724)]]" | ForEach-Object {
    $xml = [xml]$_.ToXml()
    $eventData = @{}
    $xml.Event.EventData.Data | ForEach-Object { $eventData[$_.Name] = $_.'#text' }

    [PSCustomObject]@{
        Time                = ("{0:yyyy/MM/dd HH:mm:ss}" -f $_.TimeCreated)
        Computer            = $xml.Event.System.Computer
        #SubjectLogonId      = $eventData["SubjectLogonId"]
        #SubjectUserSid      = $eventData["SubjectUserSid"]
        #SubjectDomainName   = $eventData["SubjectDomainName"]
        SubjectUserName     = $eventData["SubjectUserName"]
        #TargetUserSid       = $eventData["TargetUserSid"]
        #TargetDomainName    = $eventData["TargetDomainName"]
        TargetUserName      = $eventData["TargetUserName"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            SubjectUserName TargetUserName
----                --------            --------------- --------------
2022/08/31 19:36:09 tpl-packer          TPL-PACKER$     Administrator 
2022/08/31 19:36:10 tpl-packer          TPL-PACKER$     defaultuser0  
2022/08/31 19:36:10 tpl-packer          TPL-PACKER$     defaultuser0  
2022/09/30 23:38:54 rd01                RD01$           SRLAdmin      
2022/09/30 23:38:54 rd01                RD01$           SRLAdmin      
2022/09/30 23:46:08 rd01.shieldbase.com RD01$           SRLAdmin      
2022/10/01 07:42:24 rd01.shieldbase.com RD01$           SRLAdmin      
2022/10/02 16:43:08 rd01.shieldbase.com RD01$           SRLAdmin      
2022/10/12 08:15:33 rd01.shieldbase.com RD01$           SRLAdmin      
2022/10/14 16:12:18 rd01.shieldbase.com RD01$           SRLAdmin      
2022/10/20 01:53:17 rd01.shieldbase.com RD01$           SRLAdmin      
2022/10/20 03:01:44 rd01.shieldbase.com RD01$           SRLAdmin      
2022/10/20 18:39:54 rd01.shieldbase.com RD01$           SRLAdmin      
2022/10/21 16:52:48 rd01.shieldbase.com RD01$           SRLAdmin      
2022/10/21 17:04:01 rd01.shieldbase.com RD01$           SRLAdmin      
2022/11/11 23:30:57 rd01.shieldbase.com RD01$           SRLAdmin      
2022/11/12 06:17:22 rd01.shieldbase.com RD01$           SRLAdmin      
2023/01/02 19:59:48 rd01.shieldbase.com RD01$           SRLAdmin
```
