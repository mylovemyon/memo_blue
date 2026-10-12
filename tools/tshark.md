## -z
### -z conv
#### -z conv,eth
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z conv,eth,"ip.src == 10.3.16.5"
================================================================================
Ethernet Conversations
Filter:ip.src == 10.3.16.5
                                               |       <-      | |       ->      | |     Total     |    Relative    |   Duration   |
                                               | Frames  Bytes | | Frames  Bytes | | Frames  Bytes |      Start     |              |
Intel_2e:0d:e0       <-> VMware_a5:0b:40            0 0 bytes    117069 12 MB      117069 12 MB      237411.033732000     98151.6628
Intel_2e:0d:e0       <-> VMware_a5:00:10            0 0 bytes     77065 5803 kB     77065 5803 kB    1677.837425000     72733.4388
Intel_2e:0d:e0       <-> VMware_a5:07:40            0 0 bytes     64686 6345 kB     64686 6345 kB    10773.050550000    326170.1425
Intel_2e:0d:e0       <-> VMware_a5:12:69            0 0 bytes     38944 4127 kB     38944 4127 kB    82573.192249000    253691.4689
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
PS C:\Users\Public> tshark.exe -r .\dns.pcap -q -z dns,tree,"ip.addr == 208.67.222.222"

==============================================================================================================================================
DNS:
Packet Type                    Count         Average       Min Val       Max Val       Rate (ms)     Percent       Burst Rate    Burst Start
----------------------------------------------------------------------------------------------------------------------------------------------
Total Packets                  24937                                                   0.0001        100%          0.4900        38765.901
Response                       24937                                                   0.0001        100%          0.4900        38765.901
 Query                         12483                                                   0.0000        50.06%        0.2600        38765.901
 Response                      12454                                                   0.0000        49.94%        0.2600        38765.907
Query Type                     24937                                                   0.0001        100%          0.4900        38765.901
 A                             24009                                                   0.0001        96.28%        0.4900        38765.901
 SRV                           830                                                     0.0000        3.33%         0.0400        149091.061
 SOA                           46                                                      0.0000        0.18%         0.0200        11358.561
 PTR                           30                                                      0.0000        0.12%         0.0200        81557.474
 NS                            12                                                      0.0000        0.05%         0.0200        11358.578
 AAAA                          10                                                      0.0000        0.04%         0.0200        107005.397
Answer Type                    52219                                                   0.0002        100%          1.9700        38765.907
 A                             29304                                                   0.0001        56.12%        1.3400        38765.907
 OPT                           12441                                                   0.0000        23.82%        0.2600        38765.907
 CNAME                         10426                                                   0.0000        19.97%        0.3900        33359.938
 SOA                           44                                                      0.0000        0.08%         0.0200        11358.567
 AAAA                          4                                                       0.0000        0.01%         0.0100        107005.404
Query Name                     0                                                       0.0000        100%          -             -
Class                          24937                                                   0.0001        100%          0.4900        38765.901
 IN                            24937                                                   0.0001        100.00%       0.4900        38765.901
rcode                          24937                                                   0.0001        100%          0.4900        38765.901
 No error                      24364                                                   0.0001        97.70%        0.4900        38765.901
 No such name                  573                                                     0.0000        2.30%         0.0200        107004.050
opcodes                        24937                                                   0.0001        100%          0.4900        38765.901
 Standard query                24937                                                   0.0001        100.00%       0.4900        38765.901
Payload size                   24937         81.76         28            410           0.0001        100%          0.4900        38765.901
Query Stats                    0                                                       0.0000        100%          -             -
 Qname Len                     12483         19.95         4             85            0.0000                      0.2600        38765.901
 Label Stats                   0                                                       0.0000                      -             -
  3rd Level                    6492                                                    0.0000                      0.1900        38765.901
  4th Level or more            5717                                                    0.0000                      0.1800        33255.904
  2nd Level                    274                                                     0.0000                      0.0200        8058.604
  1st Level                    0                                                       0.0000                      -             -
Response Stats                 0                                                       0.0000        100%          -             -
 no. of questions              24908         1.00          1             1             0.0001                      0.5200        38765.907
 no. of authorities            24908         0.00          0             1             0.0001                      0.5200        38765.907
 no. of answers                24908         3.19          0             21            0.0001                      0.5200        38765.907
 no. of additionals            24908         1.00          0             1             0.0001                      0.5200        38765.907
Service Stats                  0                                                       0.0000        100%          -             -
 request-response time (msec)  12454         13.69         0.349000      950.429016    0.0000                      0.2600        38765.907
 no. of unsolicited responses  0                                                       0.0000                      -             -
 no. of retransmissions        0                                                       0.0000                      -             -

----------------------------------------------------------------------------------------------------------------------------------------------
```

### -z endpoints
#### -z endpoints,eth
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z endpoints,eth,"ip.addr == 10.3.16.5"
================================================================================
Ethernet Endpoints
Filter:ip.addr == 10.3.16.5
                       | Packets | |  Bytes  | | Tx Packets | | Tx Bytes | | Rx Packets | | Rx Bytes |
IETF-VRRP-VRID_1d          483106   263 MB              0      0 bytes         483106      263 MB
VMware_a5:0b:40            334148   94 MB          217079      82 MB           117069      12 MB
Intel_2e:0d:e0             297764   28 MB          297764      28 MB                0      0 bytes
VMware_a5:00:10            207754   127 MB         130689      121 MB           77065      5803 kB
VMware_a5:07:40            134325   31 MB           69639      25 MB            64686      6345 kB
VMware_a5:12:69            104643   37 MB           65699      33 MB            38944      4127 kB
================================================================================
```
#### -z endpoints,ip
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z endpoints,ip,"ip.addr == 10.3.16.5"
================================================================================
IPv4 Endpoints
Filter:ip.addr == 10.3.16.5
                       | Packets | |  Bytes  | | Tx Packets | | Tx Bytes | | Rx Packets | | Rx Bytes |
10.3.16.5                  780870   292 MB         297764      28 MB           483106      263 MB
10.3.58.6                  334148   94 MB          217079      82 MB           117069      12 MB
10.3.58.7                  207754   127 MB         130689      121 MB           77065      5803 kB
10.3.58.4                  134325   31 MB           69639      25 MB            64686      6345 kB
10.3.58.5                  104643   37 MB           65699      33 MB            38944      4127 kB
================================================================================
```
#### -z endpoints,tcp
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z endpoints,tcp,"ip.addr == 10.3.16.5 && ip.addr ==10.3.58.5"
================================================================================
TCP Endpoints
Filter:ip.addr == 10.3.16.5 && ip.addr ==10.3.58.5
                       |  Port  | | Packets | |  Bytes  | | Tx Packets | | Tx Bytes | | Rx Packets | | Rx Bytes |
10.3.58.5                  3389       104643   37 MB           65699      33 MB            38944      4127 kB
10.3.16.5                 14590        40150   9594 kB         13884      993 kB           26266      8600 kB
10.3.16.5                  8526        39583   16 MB           16265      2469 kB          23318      13 MB
10.3.16.5                 47037        12201   5317 kB          4235      295 kB            7966      5021 kB
10.3.16.5                 15889         6172   3064 kB          2209      190 kB            3963      2873 kB
10.3.16.5                 49081         3634   2186 kB          1319      92 kB             2315      2094 kB
10.3.16.5                 47330         2795   1517 kB           984      73 kB             1811      1443 kB
10.3.16.5                 47036           18   4163 bytes          8      1953 bytes          10      2210 bytes
10.3.16.5                 49079           18   4099 bytes          8      1921 bytes          10      2178 bytes
10.3.16.5                  8525           18   4163 bytes          8      1953 bytes          10      2210 bytes
10.3.16.5                 14589           18   4163 bytes          8      1953 bytes          10      2210 bytes
10.3.16.5                 15888           18   4163 bytes          8      1953 bytes          10      2210 bytes
10.3.16.5                 47329           18   4163 bytes          8      1953 bytes          10      2210 bytes
================================================================================
```
#### -z endpoints,udp
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z endpoints,udp,"ip.addr == 10.3.58.4 && ip.addr == 68.71.210.254"
================================================================================
UDP Endpoints
Filter:ip.addr == 10.3.58.4 && ip.addr == 68.71.210.254
                       |  Port  | | Packets | |  Bytes  | | Tx Packets | | Tx Bytes | | Rx Packets | | Rx Bytes |
68.71.210.254                53            7   689 bytes           3      331 bytes            4      358 bytes
10.3.58.4                 57908            2   206 bytes           1      95 bytes             1      111 bytes
10.3.58.4                 58596            2   215 bytes           1      95 bytes             1      120 bytes
10.3.58.4                 56354            2   184 bytes           1      84 bytes             1      100 bytes
10.3.58.4                 57943            1   84 bytes            1      84 bytes             0      0 bytes
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

### -z follow
#### -z follow,tcp
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z follow,tcp,utf-8,10.3.58.7:2603,98.124.198.1:80

===================================================================
Follow: tcp,utf-8
Filter: ((ip.src eq 10.3.58.7 and tcp.srcport eq 2603) and (ip.dst eq 98.124.198.1 and tcp.dstport eq 80)) or ((ip.src eq 98.124.198.1 and tcp.srcport eq 80) and (ip.dst eq 10.3.58.7 and tcp.dstport eq 2603))
Node 0: 10.3.58.7:2603
Node 1: 98.124.198.1:80
283
GET / HTTP/1.1
Host: flightstatus.com
User-Agent: Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko/20100101 Firefox/11.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive


        103
HTTP/1.1 302 Found
Connection: close
Pragma: no-cache
cache-control: no-cache
Location: /VfbpQ/


===================================================================
```
#### -z follow,http
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -Y "ip.addr == 10.3.58.7 && ip.addr == 98.124.198.1 && tcp.port == 80" -w http.pcap -F pcap
PS C:\Users\Public>
```
```powershell
PS C:\Users\Public> tshark.exe -r .\http.pcap -q -z follow,http,utf-8,1

===================================================================
Follow: http,utf-8
Filter: tcp.stream eq 1
Node 0: 10.3.58.7:2604
Node 1: 98.124.198.1:80
289
GET /VfbpQ/ HTTP/1.1
Host: flightstatus.com
User-Agent: Mozilla/5.0 (Windows NT 5.1; rv:11.0) Gecko/20100101 Firefox/11.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive


        97
HTTP/1.1 302 Found
Connection: close
Pragma: no-cache
cache-control: no-cache
Location: /


===================================================================
```

### -z hosts
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z hosts | Select-Object -First 10
# TShark hosts output
#
# Host data gathered from .\stark_labs_traffic_no_iSCSI.pcap

65.55.5.170     data.tvdownload.windowsmedia.com.akadns.net
23.66.231.161   a1877.b.akamai.net
66.220.147.58   developers.facebook.com
173.194.29.244  o-o.preferred.mia05s05.v14.lscache7.c.pack.google.com
174.143.139.157 pixel.parsely.com
23.66.232.51    a1470.g.akamai.net
```

### -z http
#### -z http,stat
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z http,stat,"ip.addr == 98.124.198.1"

===================================================================
HTTP Statistics with filter ip.addr == 98.124.198.1
* HTTP Response Status Codes                Packets
  302 Found                                       2
* HTTP Request Methods                      Packets
  GET                                             5
===================================================================
```

#### -z http,tree
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z http,tree,"ip.addr == 98.124.198.1"

=======================================================================================================================================
HTTP / Packet Counter:
Packet Type             Count         Average       Min Val       Max Val       Rate (ms)     Percent       Burst Rate    Burst Start
---------------------------------------------------------------------------------------------------------------------------------------
Total HTTP Packets      7                                                       0.0110        100%          0.0500        68867.095
 HTTP Request Packets   5                                                       0.0079        71.43%        0.0300        68867.095
  GET                   5                                                       0.0079        100.00%       0.0300        68867.095
 HTTP Response Packets  2                                                       0.0032        28.57%        0.0200        68867.100
  3xx: Redirection      2                                                       0.0032        100.00%       0.0200        68867.100
   302 Found            2                                                       0.0032        100.00%       0.0200        68867.100
  ???: broken           0                                                       0.0000        0.00%         -             -
  5xx: Server Error     0                                                       0.0000        0.00%         -             -
  4xx: Client Error     0                                                       0.0000        0.00%         -             -
  2xx: Success          0                                                       0.0000        0.00%         -             -
  1xx: Informational    0                                                       0.0000        0.00%         -             -
 Other HTTP Packets     0                                                       0.0000        0.00%         -             -

---------------------------------------------------------------------------------------------------------------------------------------
```

### -z http_req,tree
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z http_req,tree,"ip.addr == 98.124.198.1"

===========================================================================================================================================
HTTP / Requests:
Request Type                Count         Average       Min Val       Max Val       Rate (ms)     Percent       Burst Rate    Burst Start
-------------------------------------------------------------------------------------------------------------------------------------------
HTTP Requests by HTTP Host  5                                                       0.0079        100%          0.0300        68867.095
 flightstatus.com           5                                                       0.0079        100.00%       0.0300        68867.095
  /favicon.ico              2                                                       0.0032        40.00%        0.0200        68867.707
  /                         2                                                       0.0032        40.00%        0.0200        68867.095
  /VfbpQ/                   1                                                       0.0016        20.00%        0.0100        68867.128

-------------------------------------------------------------------------------------------------------------------------------------------
```

### -z http_seq,tree
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z http_seq,tree,"ip.addr == 98.124.198.1"

=================================================================================================================================================
HTTP / Request Sequences:
Sequence Type                     Count         Average       Min Val       Max Val       Rate (ms)     Percent       Burst Rate    Burst Start
-------------------------------------------------------------------------------------------------------------------------------------------------
HTTP Request Sequences            2                                                       0.0032        100%          0.0200        68867.100
 http://flightstatus.com/         3                                                       0.0047        150.00%       0.0300        68867.100
  http://flightstatus.com/VfbpQ/  3                                                       0.0047        100.00%       0.0300        68867.100
   http://flightstatus.com/       1                                                       0.0016        33.33%        0.0100        68867.132

-------------------------------------------------------------------------------------------------------------------------------------------------
```

### -z http_srv,tree
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z http_srv,tree,"ip.addr == 98.124.198.1"

=================================================================================================================================================
HTTP / Load Distribution:
Packet Type                       Count         Average       Min Val       Max Val       Rate (ms)     Percent       Burst Rate    Burst Start
-------------------------------------------------------------------------------------------------------------------------------------------------
HTTP Requests by Server           5                                                       0.0079        100%          0.0300        68867.095
 HTTP Requests by Server Address  5                                                       0.0079        100.00%       0.0300        68867.095
  98.124.198.1                    5                                                       0.0079        100.00%       0.0300        68867.095
   flightstatus.com               5                                                       0.0079        100.00%       0.0300        68867.095
 HTTP Requests by HTTP Host       5                                                       0.0079        100.00%       0.0300        68867.095
  flightstatus.com                5                                                       0.0079        100.00%       0.0300        68867.095
   98.124.198.1                   5                                                       0.0079        100.00%       0.0300        68867.095
HTTP Responses by Server Address  2                                                       0.0032        100%          0.0200        68867.100
 98.124.198.1                     2                                                       0.0032        100.00%       0.0200        68867.100
  OK                              2                                                       0.0032        100.00%       0.0200        68867.100

-------------------------------------------------------------------------------------------------------------------------------------------------
```

### -z icmp,srt
```powershell
PS C:\Users\Public> tshark.exe -r .\stark_labs_traffic_no_iSCSI.pcap -q -z icmp,srt

==========================================================================
ICMP Service Response Time (SRT) Statistics (all times in ms):
Filter: <none>

Requests  Replies   Lost      % Loss
1569      1557      12          0.8%

Minimum   Maximum   Mean      Median    SDeviation     Min Frame Max Frame
0.004     705.081   1.415     0.414     20.181         2415709   2458313
==========================================================================
```
