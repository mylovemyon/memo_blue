```powershell
PS C:\users\Public> tshark.exe -r .\smbtorture.cap -q -z io,phs

===================================================================
Protocol Hierarchy Statistics
Filter:

frame                                    frames:36571 bytes:11113471
  eth                                    frames:36571 bytes:11113471
    ip                                   frames:36525 bytes:11110761
      tcp                                frames:36467 bytes:11101646
        nbss                             frames:28680 bytes:4998974
          smb                            frames:28637 bytes:4994313
            _ws.malformed                frames:1 bytes:107
            _ws.unreassembled            frames:213 bytes:30246
            nbss                         frames:20 bytes:28612
              smb                        frames:20 bytes:28612
                nbss                     frames:20 bytes:28612
                  smb                    frames:20 bytes:28612
                    nbss                 frames:19 bytes:27574
                      smb                frames:19 bytes:27574
                        nbss             frames:18 bytes:26516
                          smb            frames:18 bytes:26516
                            nbss         frames:17 bytes:25002
                              smb        frames:17 bytes:25002
                                nbss     frames:9 bytes:13146
                                ...smb   frames:9 bytes:13146
                                ...nbss  frames:8 bytes:11720
                                ...smb   frames:8 bytes:11720
                                ...nbss  frames:7 bytes:10598
                                ...smb   frames:7 bytes:10598
                                ...nbss  frames:7 bytes:10598
                                ...smb   frames:7 bytes:10598
                                ...nbss  frames:6 bytes:9084
                                ...smb   frames:6 bytes:9084
                                ...nbss  frames:1 bytes:1514
                                ...smb   frames:1 bytes:1514
            smb_pipe                     frames:14 bytes:2733
              dcerpc                     frames:12 bytes:2372
                lsarpc                   frames:10 bytes:1954
              lanman                     frames:2 bytes:361
      udp                                frames:49 bytes:8318
        dhcp                             frames:8 bytes:2742
          _ws.malformed                  frames:4 bytes:1374
        nbdgm                            frames:3 bytes:729
          smb                            frames:3 bytes:729
            mailslot                     frames:3 bytes:729
              browser                    frames:3 bytes:729
        nbns                             frames:12 bytes:1320
        cldap                            frames:4 bytes:385
        dns                              frames:22 bytes:3142
      icmp                               frames:4 bytes:497
      igmp                               frames:5 bytes:300
    arp                                  frames:31 bytes:1536
    ipv6                                 frames:15 bytes:1174
      icmpv6                             frames:15 bytes:1174
===================================================================
```
