## -z
### -z conv
#### -z conv,eth
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z conv,eth
================================================================================
Ethernet Conversations
Filter:<No Filter>
                                               |       <-      | |       ->      | |     Total     |    Relative    |   Duration   |
                                               | Frames  Bytes | | Frames  Bytes | | Frames  Bytes |      Start     |              |
VMware_a5:0b:40      <-> IETF-VRRP-VRID_1d          0 0 bytes    388517 146 MB     388517 146 MB        2.886106000    336056.2834
VMware_a5:00:10      <-> IETF-VRRP-VRID_1d          0 0 bytes    364921 176 MB     364921 176 MB      342.922837000    106370.8784
Intel_2e:0d:e0       <-> VMware_a5:00:10            0 0 bytes    342286 148 MB     342286 148 MB      342.955988000    106370.9422
Intel_2e:0d:e0       <-> VMware_a5:07:40            5 358 bytes  325338 30 MB      325343 30 MB         3.662066000    336939.5310
Intel_2e:0d:e0       <-> VMware_a5:0b:40            0 0 bytes    306299 64 MB      306299 64 MB         3.990392000    336780.3887
VMware_a5:07:40      <-> IETF-VRRP-VRID_1d          0 0 bytes    287142 50 MB      287142 50 MB         2.582842000    336940.3945
VMware_a5:12:69      <-> IETF-VRRP-VRID_1d          0 0 bytes    223216 74 MB      223216 74 MB         4.392182000    336855.2905
Intel_2e:0d:e0       <-> VMware_a5:12:69            0 0 bytes    209126 78 MB      209126 78 MB         4.652256000    336855.0408
VMware_a5:00:10      <-> VMware_a5:07:40        89986 55 MB       85962 25 MB      175948 80 MB         9.946649000    336616.1596
VMware_a5:0b:40      <-> VMware_a5:00:10        75334 67 MB       60901 56 MB      136235 123 MB        9.961089000    335382.0069
VMware_a5:00:10      <-> VMware_a5:12:69        71050 73 MB       61283 19 MB      132333 93 MB      71828.898931000    264855.5376
VMware_a5:0b:40      <-> VMware_a5:07:40        58002 14 MB       67768 18 MB      125770 32 MB        18.580384000    336222.3475
VMware_a5:12:69      <-> VMware_a5:07:40        46958 13 MB       53328 13 MB      100286 27 MB         0.000000000    336924.1971
VMware_a5:0b:40      <-> VMware_a5:12:69        13633 10 MB        7577 1218 kB     21210 11 MB      136585.240455000    200220.4349
VMware_a5:00:10      <-> Broadcast                  0 0 bytes      6903 1088 kB      6903 1088 kB       9.959904000    336358.4845
VMware_a5:0b:40      <-> Broadcast                  0 0 bytes      2219 341 kB       2219 341 kB       57.384021000    336733.3466
VMware_a5:12:69      <-> Broadcast                  0 0 bytes      1576 222 kB       1576 222 kB       16.699378000    336769.5564
VMware_a5:07:40      <-> Broadcast                  0 0 bytes      1253 115 kB       1253 115 kB     3607.645064000    331609.7419
VMware_a5:07:40      <-> IPv4mcast_7f:02:02         0 0 bytes        94 8178 bytes      94 8178 bytes   623.448274000    335013.1206
VMware_a5:12:69      <-> IPv4mcast_7f:ff:fa         0 0 bytes        49 10 kB          49 10 kB      94574.696069000    143025.9681
VMware_a5:0b:40      <-> IPv4mcast_7f:ff:fa         0 0 bytes        24 4191 bytes      24 4191 bytes  314250.421979000      3714.0659
VMware_a5:07:40      <-> Intel_2d:8a:a8             2 148 bytes      13 1010 bytes      15 1158 bytes  107535.959934000      8160.0884
VMware_a5:00:10      <-> IPv4mcast_7f:ff:fa         0 0 bytes         9 1575 bytes       9 1575 bytes  11335.324176000    324072.3308
================================================================================
```
#### -z conv,ip
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z conv,ip,"ip.src == 10.3.16.5"
================================================================================
IPv4 Conversations
Filter:ip.src==10.3.16.5
                                               |       <-      | |       ->      | |     Total     |    Relative    |   Duration   |
                                               | Frames  Bytes | | Frames  Bytes | | Frames  Bytes |      Start     |              |
10.3.16.5            <-> 10.3.58.6                  0 0 bytes    117069 12 MB      117069 12 MB      237411.033732000     98151.6628
10.3.16.5            <-> 10.3.58.7                  0 0 bytes     77065 5803 kB     77065 5803 kB    1677.837425000     72733.4388
10.3.16.5            <-> 10.3.58.4                  0 0 bytes     64686 6345 kB     64686 6345 kB    10773.050550000    326170.1425
10.3.16.5            <-> 10.3.58.5                  0 0 bytes     38944 4127 kB     38944 4127 kB    82573.192249000    253691.4689
================================================================================
```
#### -z conv,tcp
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z conv,tcp,"ip.src == 10.3.16.5 && ip.dst ==10.3.58.5"
================================================================================
TCP Conversations
Filter:ip.src == 10.3.16.5 && ip.dst ==10.3.58.5
                                                           |       <-      | |       ->      | |     Total     |    Relative    |   Duration   |
                                                           | Frames  Bytes | | Frames  Bytes | | Frames  Bytes |      Start     |              |
10.3.16.5:8526             <-> 10.3.58.5:3389                   0 0 bytes     16265 2469 kB     16265 2469 kB    164856.176695000      2301.2137
10.3.16.5:14590            <-> 10.3.58.5:3389                   0 0 bytes     13884 993 kB      13884 993 kB     231186.487145000      6307.2740
10.3.16.5:47037            <-> 10.3.58.5:3389                   0 0 bytes      4235 295 kB       4235 295 kB     82574.132164000        50.1278
10.3.16.5:15889            <-> 10.3.58.5:3389                   0 0 bytes      2209 190 kB       2209 190 kB     237584.339097000      1284.3718
10.3.16.5:49081            <-> 10.3.58.5:3389                   0 0 bytes      1319 92 kB        1319 92 kB      101506.141648000       884.8373
10.3.16.5:47330            <-> 10.3.58.5:3389                   0 0 bytes       984 73 kB         984 73 kB      336201.293367000        63.3678
10.3.16.5:47036            <-> 10.3.58.5:3389                   0 0 bytes         8 1953 bytes       8 1953 bytes  82573.192249000         0.9175
10.3.16.5:49079            <-> 10.3.58.5:3389                   0 0 bytes         8 1921 bytes       8 1921 bytes  101504.969139000         1.1423
10.3.16.5:8525             <-> 10.3.58.5:3389                   0 0 bytes         8 1953 bytes       8 1953 bytes  164855.773812000         0.3697
10.3.16.5:14589            <-> 10.3.58.5:3389                   0 0 bytes         8 1953 bytes       8 1953 bytes  231186.240008000         0.2071
10.3.16.5:15888            <-> 10.3.58.5:3389                   0 0 bytes         8 1953 bytes       8 1953 bytes  237584.089569000         0.2116
10.3.16.5:47329            <-> 10.3.58.5:3389                   0 0 bytes         8 1953 bytes       8 1953 bytes  336201.053580000         0.2120
================================================================================
```
#### -z conv,udp
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z conv,udp,"ip.src == 10.3.58.4 && ip.dst == 68.71.210.254"
================================================================================
UDP Conversations
Filter:ip.src == 10.3.58.4 && ip.dst == 68.71.210.254
                                                           |       <-      | |       ->      | |     Total     |    Relative    |   Duration   |
                                                           | Frames  Bytes | | Frames  Bytes | | Frames  Bytes |      Start     |              |
10.3.58.4:57908            <-> 68.71.210.254:53                 0 0 bytes         1 95 bytes        1 95 bytes   44065.680688000         0.0000
10.3.58.4:58596            <-> 68.71.210.254:53                 0 0 bytes         1 95 bytes        1 95 bytes   44068.499508000         0.0000
10.3.58.4:57943            <-> 68.71.210.254:53                 0 0 bytes         1 84 bytes        1 84 bytes   44069.588967000         0.0000
10.3.58.4:56354            <-> 68.71.210.254:53                 0 0 bytes         1 84 bytes        1 84 bytes   49467.034186000         0.0000
================================================================================
```

### -z dests,tree
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z dests,tree,"ip.src == 10.3.16.5"

=======================================================================================================================================================
IPv4 Statistics / Destinations and Ports:
Topic / Item                            Count         Average       Min Val       Max Val       Rate (ms)     Percent       Burst Rate    Burst Start
-------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 Statistics/Destinations and Ports  297764                                                  0.0009        100%          7.0500        166734.190
 10.3.58.6                              117069                                                  0.0003        39.32%        2.8800        237551.965
  TCP                                   117069                                                  0.0003        100.00%       2.8800        237551.965
   3389                                 117063                                                  0.0003        99.99%        2.8800        237551.965
   5681                                 6                                                       0.0000        0.01%         0.0100        261761.536
 10.3.58.7                              77065                                                   0.0002        25.88%        0.8600        8060.068
  TCP                                   77065                                                   0.0002        100.00%       0.8600        8060.068
   3389                                 77065                                                   0.0002        100.00%       0.8600        8060.068
 10.3.58.4                              64686                                                   0.0002        21.72%        0.9700        139067.295
  TCP                                   64686                                                   0.0002        100.00%       0.9700        139067.295
   3389                                 63906                                                   0.0002        98.79%        0.9700        139067.295
   5681                                 780                                                     0.0000        1.21%         0.0500        258064.318
 10.3.58.5                              38944                                                   0.0001        13.08%        7.0500        166734.190
  TCP                                   38944                                                   0.0001        100.00%       7.0500        166734.190
   3389                                 38944                                                   0.0001        100.00%       7.0500        166734.190

-------------------------------------------------------------------------------------------------------------------------------------------------------
```

### -z dhcp,stat
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z dhcp,stat

=====================================================================================================
DHCP (BOOTP) Statistics:
Filter for statistics:
DHCP Message Type |Packets |
DHCP Statistics
Discover                  |0 |
Offer                     |0 |
Request                   |0 |
Decline                   |0 |
ACK                       |0 |
NAK                       |0 |
Release                   |0 |
Inform                    |0 |
Force Renew               |0 |
Lease query               |0 |
Lease Unassigned          |0 |
Lease Unknown             |0 |
Lease Active              |0 |
Bulk Lease Query          |0 |
Lease Query Done          |0 |
Active LeaseQuery         |0 |
Lease Query Status        |0 |
TLS                       |0 |
=====================================================================================================
```

### -z dns,tree
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z dns,tree

==============================================================================================================================================
DNS:
Packet Type                    Count         Average       Min Val       Max Val       Rate (ms)     Percent       Burst Rate    Burst Start
----------------------------------------------------------------------------------------------------------------------------------------------
Total Packets                  45613                                                   0.0001        100%          1.0000        38770.717
Response                       45613                                                   0.0001        100%          1.0000        38770.717
 Query                         22849                                                   0.0001        50.09%        0.5200        38770.711
 Response                      22764                                                   0.0001        49.91%        0.5300        38770.717
Query Type                     45613                                                   0.0001        100%          1.0000        38770.717
 A                             42119                                                   0.0001        92.34%        1.0000        38770.717
 SRV                           3222                                                    0.0000        7.06%         0.0800        136552.470
 SOA                           168                                                     0.0000        0.37%         0.0800        11363.326
 PTR                           48                                                      0.0000        0.11%         0.0400        156737.282
 NS                            24                                                      0.0000        0.05%         0.0400        11363.387
 AAAA                          20                                                      0.0000        0.04%         0.0600        248715.242
 TXT                           8                                                       0.0000        0.02%         0.0200        57184.011
 TKEY                          4                                                       0.0000        0.01%         0.0200        317600.104
Answer Type                    88767                                                   0.0003        100%          3.8700        38770.717
 A                             52780                                                   0.0002        59.46%        2.7600        20743.670
 CNAME                         19806                                                   0.0001        22.31%        0.8800        33364.748
 OPT                           12505                                                   0.0000        14.09%        0.2600        38770.717
 NS                            2114                                                    0.0000        2.38%         0.9100        196890.759
 SOA                           849                                                     0.0000        0.96%         0.1400        233629.496
 SRV                           590                                                     0.0000        0.66%         0.0700        327988.349
 RRSIG                         40                                                      0.0000        0.05%         0.0700        234483.542
 AAAA                          37                                                      0.0000        0.04%         0.0200        13455.662
 NSEC3                         36                                                      0.0000        0.04%         0.0600        234483.542
 TSIG                          4                                                       0.0000        0.00%         0.0100        317600.106
 DS                            4                                                       0.0000        0.00%         0.0100        13455.662
 TKEY                          2                                                       0.0000        0.00%         0.0100        317600.106
Query Name                     0                                                       0.0000        100%          -             -
Class                          45613                                                   0.0001        100%          1.0000        38770.717
 IN                            45605                                                   0.0001        99.98%        1.0000        38770.717
 CH                            8                                                       0.0000        0.02%         0.0200        57184.011
rcode                          45613                                                   0.0001        100%          1.0000        38770.717
 No error                      44275                                                   0.0001        97.07%        1.0000        38770.717
 No such name                  1331                                                    0.0000        2.92%         0.1200        233629.496
 Format error                  4                                                       0.0000        0.01%         0.0100        13455.853
 Refused                       2                                                       0.0000        0.00%         0.0100        317599.975
 Server failure                1                                                       0.0000        0.00%         0.0100        44065.734
opcodes                        45613                                                   0.0001        100%          1.0000        38770.717
 Standard query                45569                                                   0.0001        99.90%        1.0000        38770.717
 Dynamic update                44                                                      0.0000        0.10%         0.0300        317729.964
Payload size                   45613         81.09         24            3267          0.0001        100%          1.0000        38770.717
Query Stats                    0                                                       0.0000        100%          -             -
 Qname Len                     22849         20.95         4             85            0.0001                      0.5200        38770.711
 Label Stats                   0                                                       0.0000                      -             -
  3rd Level                    12185                                                   0.0000                      0.3500        45985.967
  4th Level or more            10188                                                   0.0000                      0.3500        50362.797
  2nd Level                    476                                                     0.0000                      0.0400        8063.414
  1st Level                    0                                                       0.0000                      -             -
Response Stats                 0                                                       0.0000        100%          -             -
 no. of questions              45528         1.00          1             1             0.0001                      1.0600        38770.717
 no. of authorities            45528         0.14          0             17            0.0001                      1.0600        38770.717
 no. of answers                45528         3.13          0             21            0.0001                      1.0600        38770.717
 no. of additionals            45528         0.63          0             16            0.0001                      1.0600        38770.717
Service Stats                  0                                                       0.0000        100%          -             -
 request-response time (msec)  22755         21.70         0.117000      43646.085938  0.0001                      0.5300        38770.717
 no. of retransmissions        9                                                       0.0000                      0.0600        327988.349
 no. of unsolicited responses  0                                                       0.0000                      -             -

----------------------------------------------------------------------------------------------------------------------------------------------
```

### -z endpoints
#### -z endpoints,eth
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z endpoints,eth
================================================================================
Ethernet Endpoints
Filter:<No Filter>
                       | Packets | |  Bytes  | | Tx Packets | | Tx Bytes | | Rx Packets | | Rx Bytes |
IETF-VRRP-VRID_1d         1263796   447 MB              0      0 bytes        1263796      447 MB
Intel_2e:0d:e0            1183054   321 MB        1183049      321 MB               5      358 bytes
VMware_a5:00:10           1158635   624 MB         594412      290 MB          564223      333 MB
VMware_a5:07:40           1015851   221 MB         483453      134 MB          532398      87 MB
VMware_a5:0b:40            980274   379 MB         527006      222 MB          453268      157 MB
VMware_a5:12:69            687796   286 MB         362852      172 MB          324944      113 MB
Broadcast                   11951   1768 kB             0      0 bytes          11951      1768 kB
IPv4mcast_7f:02:02             94   8178 bytes          0      0 bytes             94      8178 bytes
IPv4mcast_7f:ff:fa             82   16 kB               0      0 bytes             82      16 kB
Intel_2d:8a:a8                 15   1158 bytes          2      148 bytes           13      1010 bytes
================================================================================
```
#### -z endpoints,ip
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z endpoints,ip,"ip.src == 10.3.16.5"
================================================================================
IPv4 Endpoints
Filter:ip.src == 10.3.16.5
                       | Packets | |  Bytes  | | Tx Packets | | Tx Bytes | | Rx Packets | | Rx Bytes |
10.3.16.5                  297764   28 MB          297764      28 MB                0      0 bytes
10.3.58.6                  117069   12 MB               0      0 bytes         117069      12 MB
10.3.58.7                   77065   5803 kB             0      0 bytes          77065      5803 kB
10.3.58.4                   64686   6345 kB             0      0 bytes          64686      6345 kB
10.3.58.5                   38944   4127 kB             0      0 bytes          38944      4127 kB
================================================================================
```
#### -z endpoints,tcp
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z endpoints,tcp,"ip.src == 10.3.16.5 && ip.dst ==10.3.58.5"
================================================================================
TCP Endpoints
Filter:ip.src == 10.3.16.5 && ip.dst ==10.3.58.5
                       |  Port  | | Packets | |  Bytes  | | Tx Packets | | Tx Bytes | | Rx Packets | | Rx Bytes |
10.3.58.5                  3389        38944   4127 kB             0      0 bytes          38944      4127 kB
10.3.16.5                  8526        16265   2469 kB         16265      2469 kB              0      0 bytes
10.3.16.5                 14590        13884   993 kB          13884      993 kB               0      0 bytes
10.3.16.5                 47037         4235   295 kB           4235      295 kB               0      0 bytes
10.3.16.5                 15889         2209   190 kB           2209      190 kB               0      0 bytes
10.3.16.5                 49081         1319   92 kB            1319      92 kB                0      0 bytes
10.3.16.5                 47330          984   73 kB             984      73 kB                0      0 bytes
10.3.16.5                 47036            8   1953 bytes          8      1953 bytes           0      0 bytes
10.3.16.5                 49079            8   1921 bytes          8      1921 bytes           0      0 bytes
10.3.16.5                  8525            8   1953 bytes          8      1953 bytes           0      0 bytes
10.3.16.5                 14589            8   1953 bytes          8      1953 bytes           0      0 bytes
10.3.16.5                 15888            8   1953 bytes          8      1953 bytes           0      0 bytes
10.3.16.5                 47329            8   1953 bytes          8      1953 bytes           0      0 bytes
================================================================================
```
#### -z endpoints,udp
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z endpoints,udp,"ip.src == 10.3.58.4 && ip.dst == 68.71.210.254"
================================================================================
UDP Endpoints
Filter:ip.src == 10.3.58.4 && ip.dst == 68.71.210.254
                       |  Port  | | Packets | |  Bytes  | | Tx Packets | | Tx Bytes | | Rx Packets | | Rx Bytes |
68.71.210.254                53            4   358 bytes           0      0 bytes              4      358 bytes
10.3.58.4                 57908            1   95 bytes            1      95 bytes             0      0 bytes
10.3.58.4                 58596            1   95 bytes            1      95 bytes             0      0 bytes
10.3.58.4                 57943            1   84 bytes            1      84 bytes             0      0 bytes
10.3.58.4                 56354            1   84 bytes            1      84 bytes             0      0 bytes
================================================================================
```

### -z expert
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z expert,error

Errors (15282)
=============
   Frequency      Group           Protocol  Summary
       11626  Malformed                TLS  Malformed Packet (Exception occurred)
          27   Protocol                TLS  TLSCiphertext length MUST NOT exceed 2^14 + 2048
        3494  Malformed               SMB2  Negotiate Protocol response Reserved2 is nonzero
          13   Protocol                TLS  Zero-length Handshake fragments are not allowed
           3  Malformed                AMF  Malformed Packet (Exception occurred)
           1  Malformed  JFIF (JPEG) image  Malformed Packet (Exception occurred)
           1  Malformed             SVCCTL  Malformed Packet (Exception occurred)
          14  Malformed               RTCP  Malformed Packet (Exception occurred)
           2   Protocol               RTCP  Application name is not a string
          43  Malformed             LSARPC  Malformed Packet (Exception occurred)
           2  Malformed               SAMR  Malformed Packet (Exception occurred)
          30  Malformed       SMB_NETLOGON  Malformed Packet (Exception occurred)
          26  Malformed               HTTP  Malformed Packet (Exception occurred)
```

### 
```powershell

```
