```powershell
PS C:\Users\SANSDFIR>  .\evtx_dump-v0.12.3.exe -o jsonl -f TerminalServices-RDPClient%4Operational.json C:\Windows\System32\winevt\logs\Microsoft-Windows-TerminalServices-RDPClient%4Operational.evtx
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off"
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
memory D
```



## EventID
```sql
memory D SELECT Event.System.EventID, COUNT(*)
         FROM read_json('.\TerminalServices-RDPClient%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│     226 │           41 │
│    1024 │           54 │
│    1025 │           36 │
│    1026 │           57 │
│    1027 │            3 │
│    1028 │           44 │
│    1029 │           44 │
│    1102 │           36 │
│    1103 │           33 │
│    1104 │            3 │
│    1105 │           58 │
│    1401 │           36 │
│    1402 │           36 │
└─────────┴──────────────┘
  13 rows      2 columns
```



## 1024 (RDP Client is trying to connect to the server)
```sql
memory D SELECT Event.System.Computer, Event.EventData.CustomLevel, Event.EventData.Name, Event.EventData.Value, COUNT(*)
         FROM read_json('.\TerminalServices-RDPClient%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 1024
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────────┬─────────────┬────────────────────────┬──────────────┐
│      Computer       │ CustomLevel │    Name     │         Value          │ count_star() │
│       varchar       │   varchar   │   varchar   │          json          │    int64     │
├─────────────────────┼─────────────┼─────────────┼────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ Info        │ Server Name │ "dev01.shieldbase.com" │            1 │
└─────────────────────┴─────────────┴─────────────┴────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\TerminalServices-RDPClient%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '1024' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────┬──────────────┐
│            fullkey            │ count_star() │
│            varchar            │    int64     │
├───────────────────────────────┼──────────────┤
│ $.Event.EventData.CustomLevel │            1 │
│ $.Event.EventData.Name        │            1 │
│ $.Event.EventData.Value       │            1 │
└───────────────────────────────┴──────────────┘
```



## 1025 (RDP ClientActiveX has connected to the server)
```sql
memory D SELECT Event.System.Computer, Event.System.TimeCreated."#attributes".SystemTime, COUNT(*)
         FROM read_json('.\TerminalServices-RDPClient%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 1025
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬────────────────────────────┬──────────────┐
│      Computer       │         SystemTime         │ count_star() │
│       varchar       │         timestamp          │    int64     │
├─────────────────────┼────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ 2023-01-18 14:55:12.464633 │            1 │
└─────────────────────┴────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\TerminalServices-RDPClient%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '1025' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────┬──────────────┐
│ fullkey │ count_star() │
│ varchar │    int64     │
└─────────┴──────────────┘
          0 rows
```



## 1026 (RDP ClientActiveX has been disconnected)
```sql
memory D SELECT Event.System.Computer, Event.EventData.CustomLevel, Event.EventData.Name, Event.EventData.Value, COUNT(*)
         FROM read_json('.\TerminalServices-RDPClient%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 1026
         GROUP BY ALL
         ORDER BY ALL;
┌─────────────────────┬─────────────┬───────────────────┬───────┬──────────────┐
│      Computer       │ CustomLevel │       Name        │ Value │ count_star() │
│       varchar       │   varchar   │      varchar      │ json  │    int64     │
├─────────────────────┼─────────────┼───────────────────┼───────┼──────────────┤
│ rd01.shieldbase.com │ Info        │ Disconnect Reason │ 1     │            1 │
└─────────────────────┴─────────────┴───────────────────┴───────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\TerminalServices-RDPClient%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '1026' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────┬──────────────┐
│            fullkey            │ count_star() │
│            varchar            │    int64     │
├───────────────────────────────┼──────────────┤
│ $.Event.EventData.CustomLevel │            1 │
│ $.Event.EventData.Name        │            1 │
│ $.Event.EventData.Value       │            1 │
└───────────────────────────────┴──────────────┘
```
Value
```
0: No error
1: User-initiated client disconnect
2: User-initiated client logoff
3: The administrator has ended the session/An error occurred while the connection was being established/A network problem occurred
260: Remote Desktop can't find the computer ""
261: The Remote Desktop client access license stored on this computer is in an invalid format
262: Your computer does not have enough virtual memory available
263: No error
264: The two computers couldn't connect in the amount of time allotted
265: The local computer's client access license could not be upgraded or renewed
266: The smart card service is not running
267: The remote session was disconnected because license store creation failed with access denied.
516: Remote access to the server is not enabled/The remote computer is turned off/The remote computer is not available on the network
522: A smart card reader was not detected
772: The connection was lost due to a network error
778: There is no card inserted in the smart card reader
1024: Remote Desktop Connection could not find the destination computer
1026: An error occurred while Remote Desktop Connection was loading the destination computer
1028: An error occurred while Remote Desktop Connection was redirecting to the destination computer
1029: Tthere was a problem setting up the virtual machine
1030: Because of a security error, the client could not connect to the remote computer
1031: Windows can't find the IP address of the destination virtual machine
1032: The specified computer name contains invalid characters
1033: Connection processing has been canceled
1034: An error has occurred in the smart card subsystem
1040: The Connection Broker couldn't validate the settings specified in your RDP file
1041: A timeout error occurred while Remote Desktop Connection was starting the virtual machine
1042: A session monitoring error occurred while Remote Desktop Connection was starting the virtual machine
1796: This computer can't connect to the remote computer
1800: You already have a console session in progress
2056: The remote computer disconnected the session because of an error in the licensing protocol
2308: The connection to the remote computer was lost, possibly due to network connectivity problems
2311: The connection has been terminated because an unexpected server authentication certificate was received from the remote computer
2312: A licensing error occurred while the client was attempting to connect (Licensing timed out)
2567: The specified username does not exist
2820: An error occurred that prevented the connection
2822: Because of an error in data encryption, this session will end
2823: The user account is currently disabled and cannot be used
2825: The remote computer requires Network Level Authentication, which your computer does not support
3079: A user account restriction (for example, a time-of-day restriction) is preventing you from logging on
3080: The remote session was disconnected because of a decompression failure at the client side
3335: The user account has been locked because there were too many logon attempts or password change attempts
3337: The security policy of your computer requires you to type a password on the Windows Security dialog box
3590: The client can't connect because it doesn't support FIPS encryption level
3591: This user account has expired
3592: Failed to reconnect to your remote session
3593: The remote PC doesn't support Restricted Administration mode
3847: This user account's password has expired
3848: A connection will not be made because credentials may not be sent to the remote computer
4103: The system administrator has restricted the times during which you may log in
4104: The remote session was disconnected because your computer is running low on video resources
4339: The remote computer does not support RemoteApp
4359: The system administrator has limited the computers you can log on with
4498: The remote session was disconnected because of a decryption error at the server
4615: You must change your password before logging on the first time
4871: The system administrator has restricted the types of logon (network or interactive) that you may use
5127: The Kerberos sub-protocol User2User is required
6919: The authentication certificate received from the remote computer is expired or invalid
7431: There is a time or date difference between your computer and the remote computer
8711: Your computer can't connect to the remote computer because your smart card is locked out
9479: Could not auto-reconnect to your applications,please re-launch your applications
9732: Client and server versions do not match
33554433: Failed to reconnect to the remote program
33554434: The remote computer does not support RemoteApp
50331649: The username or password is not valid
50331650: Your computer can't connect to the remote computer because it can't verify the certificate revocation list
50331651: The requested Remote Desktop Gateway server address and the server SSL certificate subject name do not match/The certificate is expired or revoked
50331652: The SSL certificate was revoked by the certification authority
50331653: This computer can't verify the identity of the RD Gateway
50331654: The Remote Desktop Gateway server address requested and the certificate subject name do not match
50331655: The Remote Desktop Gateway server's certificate has expired or has been revoked
50331656: An error occurred on the remote computer that you want to connect to
50331657: An error occurred while sending data to the Remote Desktop Gateway server/The server is temporarily unavailable or a network connection is down
50331658: Either the server is temporarily unavailable or a network connection is down
50331659: An alternate logon method is required
50331660: The Remote Desktop Gateway server address is unreachable or incorrect
50331661: The Remote Desktop Gateway server is temporarily unavailable
50331662: The Remote Desktop Services client component is missing or is an incorrect version
50331663: The Remote Desktop Gateway server is running low on server resources and is temporarily unavailable
50331664: An incorrect version of rpcrt4.dll has been detected
50331665: No smart card service is installed
50331666: The smart card has been removed
50331667: No smart card is available
50331668: The smart card has been removed
50331669: The user name or password is not valid
50331671: A security package error occurred in the transport layer
50331672: The Remote Desktop Gateway server has ended the connection
50331673: The Remote Desktop Gateway server administrator has ended the connection
50331674: Your credentials were incorrect or your smart card was not recognized
50331675: Your user account is not listed in the RD Gateway's permission list
50331676: Your user account is not authorized to access the RD Gateway/You are using an incompatible authentication method
50331679: Your network administrator has restricted access to this RD Gateway server.
50331680: Your computer can't connect to the remote computer because the web proxy server requires authentication.
50331681: Your password has expired or you must change the password
50331682: The Remote Desktop Gateway server reached its maximum allowed connections
50331683: The Remote Desktop Gateway server does not support the request
50331684: The client does not support one of the Remote Desktop Gateway's capabilities
50331685: The Remote Desktop Gateway server and this computer are incompatible
50331686: The credentials used are not valid
50331687: Your computer or device did not pass the Network Access Protection requirements set by your network administrator
50331688: No certificate was configured to use at the Remote Desktop Gateway server
50331689: The RD Gateway server that you are trying to connect to is not allowed by your computer administrator
50331690: Your computer or device did not meet the Network Access Protection requirements set by your network administrator
50331691: A user name and password are required to authenticate to the Remote Desktop Gateway server instead of smart card credentials
50331692: Ssmart card credentials are required to authenticate to the Remote Desktop Gateway server instead of a user name and password
50331693: No smart card reader is detected
50331695: Authentication to the firewall failed due to missing firewall credentials
50331696: Authentication to the firewall failed due to invalid firewall credentials
50331698: The remote computer didn't receive any input from you
50331699: The session timeout limit was reached
50331700: An invalid cookie was sent to the Remote Desktop Gateway server
50331701: The cookie was rejected by the Remote Desktop Gateway server
50331703: The Remote Desktop Gateway server is expecting an authentication method different from the one attempted
50331704: The RD Gateway connection ended because periodic user authentication failed
50331705: The RD Gateway connection ended because periodic user authorization failed
50331707: The Remote Desktop Gateway and the remote computer are unable to exchange policies
50331708: The smart card is not valid, the smart card certificate was not found in the certificate store, or the Certificate Propagation service is not running
50331709: First log on to the following website= <a href=""></a>
50331710: You must first log on to an authentication website
50331711: Your session has ended. To continue using the program or computer, first log on to the following website= <a href=""></a>
50331712: Your session has ended. To continue using the program or computer, you must first log on to an authentication website
50331713: Periodic user authorization failed
50331714: The size of the cookie exceeded the supported size
50331716: Your computer can't connect to the remote computer using the specified forward proxy configuration.
50331717: You do not have permission to this resource
50331718: There are currently no resources available to connect to
50331719: An error occurred while Remote Desktop Connection was accessing this resource
50331721: Your Remote Desktop Client needs to be updated to the newest version
50331722: Your network configuration doesn't allow the necessary HTTPS ports
50331723: We're setting up more resources, and it might take a few minutes
50331724: The user name you entered does not match the user name used to subscribe to your applications
50331725: Looks like there are too many users trying out the Azure RemoteApp service at the moment
50331726: Maximum user limit has been reached
50331727: Your trial period for Azure RemoteApp has expired
50331728: You no longer have access to Azure RemoteApp
```
