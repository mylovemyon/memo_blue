## -z
### -z conv,eth
```powershell
PS C:\Users\Public> tshark -r .\stark_labs_traffic_no_iSCSI.pcap -q -z conv,eth
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
