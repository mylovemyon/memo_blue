https://wiki.wireshark.org/uploads/__moin_import__/attachments/SampleCaptures/smbtorture.cap.gz

# -z io,phs
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

# -z smb,srt
```powershell
PS C:\users\Public> tshark.exe -r .\smbtorture.cap -q -z smb,srt

===================================================================
SMB SRT Statistics:
Filter: smb.cmd
Index  Commands               Calls    Min SRT    Max SRT    Avg SRT    Sum SRT
    0  Create Directory          150   0.000820   0.110187   0.003823   0.573383
    1  Delete Directory          183   0.000271   0.170288   0.002946   0.539089
    2  Open                       10   0.000272   0.990552   0.099655   0.996554
    3  Create                      4   0.000674   0.001571   0.001191   0.004763
    4  Close                    2848   0.000139   0.246837   0.003536  10.069510
    5  Flush                       4   0.000193   0.022617   0.006002   0.024006
    6  Delete                   1094   0.000203  11.127354   0.018706  20.464417
    7  Rename                     12   0.000388   0.963960   0.085933   1.031193
    8  Query Information           1   0.000943   0.000943   0.000943   0.000943
    9  Set Information             3   0.000298   0.001016   0.000664   0.001991
   10  Read                        9   0.000343   0.001214   0.000742   0.006674
   11  Write                       9   0.001455   0.066657   0.025555   0.229999
   12  Lock Byte Range            11   0.000163   0.295425   0.045202   0.497227
   13  Unlock Byte Range          11   0.000161   0.000461   0.000288   0.003173
   14  Create Temp                 1   0.003164   0.003164   0.003164   0.003164
   15  Create New                  4   0.000365   0.002366   0.001268   0.005070
   16  Check Directory            50   0.000193   0.001672   0.000414   0.020708
   17  Process Exit               81   0.000156   0.025244   0.001248   0.101107
   18  Seek                        8   0.000146   0.000486   0.000289   0.002311
   19  Lock And Read              13   0.000269   0.230518   0.034371   0.446829
   20  Write And Unlock            7   0.000798   0.066998   0.011151   0.078054
   34  Set Information2           42   0.000342   0.021945   0.001767   0.074205
   35  Query Information2         41   0.000182   0.004411   0.000461   0.018899
   36  Locking AndX               66   0.000161   1.018402   0.049763   3.284354
   37  Trans                       8   0.000449   0.144432   0.024620   0.196959
   39  IOCTL                       3   0.000165   0.000328   0.000239   0.000717
   44  Write And Close             9   0.000272   0.003144   0.001203   0.010823
   45  Open AndX                2499   0.000237   1.045968   0.002564   6.408113
   46  Read AndX                 164   0.000183   0.326026   0.144804  23.747882
   47  Write AndX                173   0.000185   0.389378   0.006775   1.172023
  113  Tree Disconnect            27   0.000193   0.029854   0.002282   0.061621
  114  Negotiate Protocol         54   0.000652   2.317621   0.478501  25.839040
  115  Session Setup AndX        151   0.000488   0.292059   0.032533   4.912490
  116  Logoff AndX                11   0.000731   0.046170   0.007830   0.086127
  117  Tree Connect AndX          52   0.000232   0.025335   0.003625   0.188496
  128  Query Information Disk      1   0.000269   0.000269   0.000269   0.000269
  129  Search                    274   0.000188   0.048554   0.000745   0.204049
  130  Find                        2   0.000797   0.004696   0.002747   0.005493
  131  Find Unique                 2   0.000377   0.001204   0.000791   0.001581
  132  Find Close                  1   0.001286   0.001286   0.001286   0.001286
  162  NT Create AndX            415   0.000180   1.078294   0.015676   6.505393
  165  NT Rename                4107   0.000157   0.970267   0.000630   2.587323
  194  Close Print File            1   0.000481   0.000481   0.000481   0.000481

Filter: smb.trans2.cmd
Index  Transaction2 Commands  Calls    Min SRT    Max SRT    Avg SRT    Sum SRT
    0  OPEN2                      17   0.000265   1.006116   0.060693   1.031776
    1  FIND_FIRST2               207   0.000870   0.049089   0.003321   0.687493
    2  FIND_NEXT2                478   0.000984   0.082195   0.003419   1.634491
    3  QUERY_FS_INFO              23   0.000334   0.004503   0.000938   0.021572
    5  QUERY_PATH_INFO           200   0.000308   0.029704   0.001763   0.352649
    6  SET_PATH_INFO              35   0.000318   0.022589   0.001671   0.058490
    7  QUERY_FILE_INFO           152   0.000304   0.040773   0.001879   0.285566
    8  SET_FILE_INFO             135   0.000195   0.267650   0.005293   0.714537
   13  CREATE_DIRECTORY            2   0.002187   0.002504   0.002346   0.004691

Filter: smb.nt.function
Index  NT Transaction Sub-Commands Calls    Min SRT    Max SRT    Avg SRT    Sum SRT
    1  NT CREATE                  21   0.000224   0.168687   0.009643   0.202508
    2  NT IOCTL                    7   0.000410   0.052302   0.013486   0.094403
    3  NT SET SECURITY DESC      198   0.000217   0.186613   0.010805   2.139410
    4  NT NOTIFY                   3   0.003261   0.003834   0.003546   0.010639
    6  NT QUERY SECURITY DESC    208   0.000322   0.103605   0.022093   4.595282
==================================================================
```

# smb
## smb.cmd
### 0x00
```powershell
PS C:\Users\Public> tshark.exe -r .\smbtorture.cap -Y "smb.cmd == 0x00" -T fields -e frame.time_utc -e smb.cmd -e smb.path -e smb.dir_name -e smb.uid -e smb.mid -e smb.flags.response -e smb.nt_status -e ip.src -e ip.dst -E header=y -E separator=","  -E quote=d -t u | Out-File -Encoding utf8 -FilePath .\0x00.csv
```
```sql
memory D SELECT timezone('UTC', "frame.time_utc") AS time_utc, "smb.path", "smb.dir_name", "smb.uid", "smb.mid", "smb.flags.response", hex("smb.nt_status"), "ip.src", "ip.dst", COUNT(*)
         FROM read_csv_auto('0x00.csv')
         GROUP BY ALL
         ORDER BY time_utc
         LIMIT 10;
┌────────────────────────────┬───────────────────────────┬─────────────────┬─────────┬─────────┬────────────────────┬──────────────────────┬─────────────────┬─────────────────┬──────────────┐
│          time_utc          │         smb.path          │  smb.dir_name   │ smb.uid │ smb.mid │ smb.flags.response │ hex("smb.nt_status") │     ip.src      │     ip.dst      │ count_star() │
│         timestamp          │          varchar          │     varchar     │  int64  │  int64  │      boolean       │       varchar        │     varchar     │     varchar     │    int64     │
├────────────────────────────┼───────────────────────────┼─────────────────┼─────────┼─────────┼────────────────────┼──────────────────────┼─────────────────┼─────────────────┼──────────────┤
│ 2005-11-19 03:31:58.791492 │ \\\\192.168.114.129\\TEST │ \\testsfileinfo │    2048 │      10 │ false              │ 0                    │ 192.168.114.1   │ 192.168.114.129 │            1 │
│ 2005-11-19 03:31:58.804865 │ \\\\192.168.114.129\\TEST │ NULL            │    2048 │      10 │ true               │ 0                    │ 192.168.114.129 │ 192.168.114.1   │            1 │
│ 2005-11-19 03:32:01.278123 │ \\\\192.168.114.129\\TEST │ \\testsearch    │    2048 │      43 │ false              │ 0                    │ 192.168.114.1   │ 192.168.114.129 │            1 │
│ 2005-11-19 03:32:01.283016 │ \\\\192.168.114.129\\TEST │ NULL            │    2048 │      43 │ true               │ 0                    │ 192.168.114.129 │ 192.168.114.1   │            1 │
│ 2005-11-19 03:32:04.401795 │ \\\\192.168.114.129\\TEST │ \\testsearch    │    2048 │    1590 │ false              │ 0                    │ 192.168.114.1   │ 192.168.114.129 │            1 │
│ 2005-11-19 03:32:04.402836 │ \\\\192.168.114.129\\TEST │ NULL            │    2048 │    1590 │ true               │ 0                    │ 192.168.114.129 │ 192.168.114.1   │            1 │
│ 2005-11-19 03:32:07.665328 │ \\\\192.168.114.129\\TEST │ \\testsearch    │    2048 │    3006 │ false              │ 0                    │ 192.168.114.1   │ 192.168.114.129 │            1 │
│ 2005-11-19 03:32:07.667446 │ \\\\192.168.114.129\\TEST │ NULL            │    2048 │    3006 │ true               │ 0                    │ 192.168.114.129 │ 192.168.114.1   │            1 │
│ 2005-11-19 03:32:07.95141  │ \\\\192.168.114.129\\TEST │ \\testsearch    │    2048 │    3071 │ false              │ 0                    │ 192.168.114.1   │ 192.168.114.129 │            1 │
│ 2005-11-19 03:32:07.953721 │ \\\\192.168.114.129\\TEST │ NULL            │    2048 │    3071 │ true               │ 0                    │ 192.168.114.129 │ 192.168.114.1   │            1 │
└────────────────────────────┴───────────────────────────┴─────────────────┴─────────┴─────────┴────────────────────┴──────────────────────┴─────────────────┴─────────────────┴──────────────┘
  10 rows                                                                                                                                                                          10 columns
```
