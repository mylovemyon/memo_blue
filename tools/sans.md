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
2022/09/30 23:43:51 rd01     -         -      -                                 dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:53 rd01     -         -      -                                 dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30    
2022/09/30 23:43:53 rd01     -         -      -                                 dc01.shieldbase.com dc01.shieldbase.com SHIELDBASE       srl.admin      C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe 0xa30 
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

Time                Computer            IpAddress IpPort SubjectDomainName SubjectUserName TargetServerName TargetInfo TargetDomainName TargetUserName ProcessName                     ProcessID
----                --------            --------- ------ ----------------- --------------- ---------------- ---------- ---------------- -------------- -----------                     ---------
2022/10/21 16:38:27 rd01.shieldbase.com ::1       0      shieldbase                        localhost        localhost  RD01             SRLAdmin       C:\Windows\System32\consent.exe 0x828    
2023/01/05 21:41:01 rd01.shieldbase.com ::1       0      shieldbase                        localhost        localhost  shieldbase       tdungan        C:\Windows\System32\consent.exe 0x2904 
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

Time                Computer            IpAddress IpPort SubjectDomainName SubjectUserName TargetServerName     TargetInfo                   TargetDomainName TargetUserName ProcessName                   ProcessID
----                --------            --------- ------ ----------------- --------------- ----------------     ----------                   ---------------- -------------- -----------                   ---------
2023/01/18 14:55:11 rd01.shieldbase.com -         -      shieldbase                        dev01.shieldbase.com TERMSRV/dev01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\lsass.exe 0x2bc    
2023/01/18 14:55:11 rd01.shieldbase.com -         -      shieldbase                        dev01.shieldbase.com TERMSRV/dev01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\lsass.exe 0x2bc  
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

Time                Computer            IpAddress   IpPort SubjectDomainName SubjectUserName TargetServerName       TargetInfo                  TargetDomainName TargetUserName ProcessName                       ProcessID
----                --------            ---------   ------ ----------------- --------------- ----------------       ----------                  ---------------- -------------- -----------                       ---------
2023/01/23 15:14:06 rd01.shieldbase.com 172.16.7.11 51022  shieldbase                        wkstn01.shieldbase.com wkstn01.shieldbase.com      shieldbase       wacsvc         C:\Windows\System32\wbem\WMIC.exe 0x218c   
2023/01/23 15:14:06 rd01.shieldbase.com 172.16.7.11 51022  shieldbase                        wkstn01.shieldbase.com host/wkstn01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\wbem\WMIC.exe 0x218c
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
} | Where-Object {$_.IpAddress -like "*172*"} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            IpAddress    IpPort SubjectDomainName SubjectUserName TargetServerName       TargetInfo                   TargetDomainName TargetUserName ProcessName                     ProcessID
----                --------            ---------    ------ ----------------- --------------- ----------------       ----------                   ---------------- -------------- -----------                     ---------
2022/09/30 23:44:12 rd01                172.16.4.4   49668  -                                 dc01.shieldbase.com    dc01.shieldbase.com          SHIELDBASE       srl.admin      C:\Windows\System32\svchost.exe 0x7bc    
2022/10/01 14:55:12 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xb74    
2022/10/20 01:51:51 rd01.shieldbase.com 172.16.30.6  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x604    
2022/10/20 01:54:35 rd01.shieldbase.com 172.16.30.6  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x598    
2022/10/20 03:02:24 rd01.shieldbase.com 172.16.30.6  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5d4    
2022/10/20 18:41:16 rd01.shieldbase.com 172.16.30.6  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x644    
2022/10/21 16:58:19 rd01.shieldbase.com 172.16.30.6  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5ec    
2022/10/21 17:07:26 rd01.shieldbase.com 172.16.30.6  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5dc    
2022/10/29 14:13:12 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5dc    
2022/11/06 21:23:04 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5dc    
2022/11/07 14:58:40 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5dc    
2022/11/13 12:51:13 rd01.shieldbase.com 172.16.30.7  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/16 17:57:53 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/17 19:55:33 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/18 18:53:55 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/21 17:20:21 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/22 16:28:07 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/22 20:31:44 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/11/24 17:30:49 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/01 19:34:18 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/01 19:36:13 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/01 19:50:39 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/04 12:25:19 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/04 14:34:36 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/05 14:32:23 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/05 20:11:03 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/07 19:52:03 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/12 10:33:26 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/12 14:07:14 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/14 14:17:12 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/14 14:31:01 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/14 15:33:32 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x698    
2022/12/16 17:35:28 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/20 21:03:58 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/21 21:54:42 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:01:46 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:06:08 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:07:16 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:08:13 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 00:32:32 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 15:51:50 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 16:26:16 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 16:34:54 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/22 16:37:40 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/24 05:51:02 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2022/12/26 21:22:44 rd01.shieldbase.com 172.16.30.4  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2023/01/02 18:01:15 rd01.shieldbase.com 172.16.30.20 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0xbb8    
2023/01/02 22:56:33 rd01.shieldbase.com 172.16.30.20 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/03 21:54:16 rd01.shieldbase.com 172.16.30.20 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/04 04:35:14 rd01.shieldbase.com 172.16.30.20 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/05 20:49:49 rd01.shieldbase.com 172.16.30.20 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/09 16:57:25 rd01.shieldbase.com 172.16.30.20 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/12 06:19:43 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/12 20:40:54 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/13 18:32:16 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/17 00:12:38 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/17 04:21:56 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/17 14:43:03 rd01.shieldbase.com 172.16.6.18  0      shieldbase                        localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/17 23:30:56 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/18 14:50:19 rd01.shieldbase.com 172.16.6.18  0      shieldbase                        localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/18 15:26:39 rd01.shieldbase.com 172.16.6.18  0      shieldbase                        localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/18 20:32:06 rd01.shieldbase.com 172.16.6.18  0      shieldbase                        localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/18 21:48:12 rd01.shieldbase.com 172.16.6.18  0      shieldbase                        localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/19 02:58:37 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/19 14:28:14 rd01.shieldbase.com 172.16.6.18  0      shieldbase                        localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x888    
2023/01/19 14:53:02 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/19 18:02:01 rd01.shieldbase.com 172.16.30.3  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/22 23:31:01 rd01.shieldbase.com 172.16.30.14 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/23 14:37:29 rd01.shieldbase.com 172.16.30.14 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x888    
2023/01/23 14:52:56 rd01.shieldbase.com 172.16.30.14 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x628    
2023/01/23 15:14:05 rd01.shieldbase.com 172.16.7.11  135    shieldbase                        wkstn01.shieldbase.com RPCSS/wkstn01.shieldbase.com SHIELDBASE.COM   wacsvc         C:\Windows\System32\svchost.exe 0x3b4    
2023/01/23 20:53:05 rd01.shieldbase.com 172.16.4.9   0      shieldbase                        localhost              localhost                    shieldbase       wacsvc         C:\Windows\System32\svchost.exe 0x628    
2023/01/24 03:21:30 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x628    
2023/01/24 14:17:13 rd01.shieldbase.com 172.16.30.8  0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x628    
2023/01/25 14:19:26 rd01.shieldbase.com 172.16.30.23 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x60c    
2023/01/25 14:38:50 rd01.shieldbase.com 172.16.30.23 0      shieldbase                        localhost              localhost                    shieldbase       tdungan        C:\Windows\System32\svchost.exe 0x5f8
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
        FailureReason     = $eventData["FailureReason "]
        MasterKeyId       = $eventData["MasterKeyId"]
        RecoveryServer    = $eventData["RecoveryServer"]
        RecoveryKeyId     = $eventData["RecoveryKeyId"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            SubjectLogonId SubjectUserSid                                 SubjectDomainName SubjectUserName FailureReason MasterKeyId                          RecoveryServer RecoveryKeyId                       
----                --------            -------------- --------------                                 ----------------- --------------- ------------- -----------                          -------------- -------------                       
2022/11/06 23:32:09 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a                      7ff8a496-3871-4c2d-ad25-2ee9bcd363fe                                                    
2022/11/06 23:32:10 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a                      44effb1e-f066-43fa-b032-c5602f4a5121                                                    
2022/11/06 23:32:10 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a                      8367f31a-23cc-41ec-92e9-cc34bdaef213                                                    
2023/01/02 18:01:26 rd01.shieldbase.com 0x26a9c239     S-1-5-21-2838623409-1327563992-2591358621-1136 shieldbase        tdungan                       23027cbe-10ef-4c1c-9827-15c1511d33fe                5a29a8b3-26f3-42f5-8abd-854ce52aa72f
2023/01/17 14:43:25 rd01.shieldbase.com 0x1f84faa1     S-1-5-21-2838623409-1327563992-2591358621-1220 shieldbase        wacsvc                        7ce88ffb-d59c-4e66-9123-5ac40901cdb8                5a29a8b3-26f3-42f5-8abd-854ce52aa72f
```


## security(4694)
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
        FailureReason      = $eventData["FailureReason "]
        MasterKeyId        = $eventData["MasterKeyId"]
        ProtectedDataFlags = $eventData["ProtectedDataFlags"]
        CryptoAlgorithms   = $eventData["CryptoAlgorithms"]
    }
} | Sort-Object Time | Format-Table -AutoSize -Property *

Time                Computer            SubjectLogonId SubjectUserSid                                 SubjectDomainName SubjectUserName FailureReason MasterKeyId ProtectedDataFlags CryptoAlgorithms    
----                --------            -------------- --------------                                 ----------------- --------------- ------------- ----------- ------------------ ----------------    
2022/10/21 16:38:27 rd01.shieldbase.com 0x225ecb8      S-1-5-21-2908624845-2463485410-257172065-1001  RD01              SRLAdmin                      Resync      0x20000000         AES-256 , SHA2-512  
2022/10/21 16:38:27 rd01.shieldbase.com 0x225ecd6      S-1-5-21-2908624845-2463485410-257172065-1001  RD01              SRLAdmin                      Resync      0x20000000         AES-256 , SHA2-512  
2022/11/06 23:32:09 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a                      Export Flag 0x0                3DES-192 , SHA1-160 
2022/11/06 23:32:10 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a                      Export Flag 0x0                3DES-192 , SHA1-160 
2022/11/06 23:32:10 rd01.shieldbase.com 0x249ae982     S-1-5-21-2838623409-1327563992-2591358621-1125 shieldbase        rsydow-a                      Export Flag 0x0                3DES-192 , SHA1-160 
2023/01/17 14:43:38 rd01.shieldbase.com 0x1f84faa1     S-1-5-21-2838623409-1327563992-2591358621-1220 shieldbase        wacsvc                        Edge        0x10               3DES-192 , SHA1-160 
2023/01/17 14:45:41 rd01.shieldbase.com 0x1f84faa1     S-1-5-21-2838623409-1327563992-2591358621-1220 shieldbase        wacsvc                        Edge        0x10               3DES-192 , SHA1-160 
2023/01/17 14:50:37 rd01.shieldbase.com 0x1f84faa1     S-1-5-21-2838623409-1327563992-2591358621-1220 shieldbase        wacsvc                        Edge        0x10               3DES-192 , SHA1-160
```
