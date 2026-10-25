```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f SmbClient%4Connectivity.json C:\Windows\System32\winevt\logs\Microsoft-Windows-SmbClient%4Connectivity.evtx
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
         FROM read_json('.\SmbClient%4Connectivity.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│   30800 │          182 │
│   30801 │            2 │
│   30803 │           43 │
│   30804 │           81 │
│   30805 │          103 │
│   30806 │           76 │
│   30807 │          118 │
│   30808 │          155 │
│   30810 │          220 │
│   30811 │           26 │
│   30812 │           99 │
│   30813 │            3 │
│   30822 │            9 │
└─────────┴──────────────┘
  13 rows      2 columns
```



## 30805 (SMB Client: the client lost its session to the server")
```sql
memory D SELECT Event.System.Computer, Event.EventData.ServerName, Event.EventData.Address, hex(Event.EventData.Status), COUNT(*)
         FROM read_json('.\SmbClient%4Connectivity-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 30805
         GROUP BY ALL
         ORDER BY Event.EventData.ServerName;
┌─────────────────────┬────────────┬─────────┬───────────────────────────────┬──────────────┐
│      Computer       │ ServerName │ Address │ hex("Event".EventData.Status) │ count_star() │
│       varchar       │  varchar   │ varchar │            varchar            │    int64     │
├─────────────────────┼────────────┼─────────┼───────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ \file01    │         │ C0000203                      │            6 │
│ rd01.shieldbase.com │ \file01    │         │ C000020C                      │            3 │
└─────────────────────┴────────────┴─────────┴───────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey
         FROM read_json_objects('.\SmbClient%4Connectivity-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '30805' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┐
│              fullkey               │
│              varchar               │
├────────────────────────────────────┤
│ $.Event.EventData.Address          │
│ $.Event.EventData.AddressLength    │
│ $.Event.EventData.ServerName       │
│ $.Event.EventData.ServerNameLength │
│ $.Event.EventData.SessionId        │
│ $.Event.EventData.Status           │
│ $.Event.EventData.TreeId           │
└────────────────────────────────────┘
```



## 30806 (SMB Client: The client re-established its session to the server)
```sql
memory D SELECT Event.System.Computer, Event.EventData.ServerName,
             (SELECT string_agg(CAST(TRY_CAST('0x' || substr(ip_hex, i, 2) AS INTEGER) AS VARCHAR), '.' ORDER BY i)
              FROM (SELECT substr(Event.EventData.Address, 9, 8) AS ip_hex) AS a, range(1, 8, 2) AS r(i))
             || ':' ||
             CAST(TRY_CAST('0x' || substr(Event.EventData.Address, 5, 4) AS INTEGER) AS VARCHAR) AS IP_Port,
             hex(Event.EventData.Status),
             COUNT(*)
         FROM read_json('.\SmbClient%4Connectivity-rd01.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 30806
         GROUP BY ALL
         ORDER BY Event.EventData.ServerName;
┌─────────────────────┬────────────┬────────────────┬───────────────────────────────┬──────────────┐
│      Computer       │ ServerName │    IP_Port     │ hex("Event".EventData.Status) │ count_star() │
│       varchar       │  varchar   │    varchar     │            varchar            │    int64     │
├─────────────────────┼────────────┼────────────────┼───────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ \file01    │ 172.16.4.5:445 │ 0                             │            7 │
└─────────────────────┴────────────┴────────────────┴───────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey
         FROM read_json_objects('.\SmbClient%4Connectivity-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '30806' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┐
│              fullkey               │
│              varchar               │
├────────────────────────────────────┤
│ $.Event.EventData.Address          │
│ $.Event.EventData.AddressLength    │
│ $.Event.EventData.ServerName       │
│ $.Event.EventData.ServerNameLength │
│ $.Event.EventData.SessionId        │
│ $.Event.EventData.Status           │
│ $.Event.EventData.TreeId           │
└────────────────────────────────────┘
```



## 30807 (The connection to the share was lost)
```sql
memory D SELECT Event.System.Computer, Event.EventData.ServerName, Event.EventData.Address, hex(Event.EventData.Status), COUNT(*)
         FROM read_json('.\SmbClient%4Connectivity-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 30807
         GROUP BY ALL
         ORDER BY Event.EventData.ServerName;
┌─────────────────────┬──────────────────────────┬─────────┬───────────────────────────────┬──────────────┐
│      Computer       │        ServerName        │ Address │ hex("Event".EventData.Status) │ count_star() │
│       varchar       │         varchar          │ varchar │            varchar            │    int64     │
├─────────────────────┼──────────────────────────┼─────────┼───────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ \file01\Installers       │         │ C000020C                      │            1 │
│ rd01.shieldbase.com │ \file01\shieldbase-share │         │ C000020C                      │            2 │
│ rd01.shieldbase.com │ \file01\shieldbase-share │         │ C0000203                      │            6 │
└─────────────────────┴──────────────────────────┴─────────┴───────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey
         FROM read_json_objects('.\SmbClient%4Connectivity-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '30807' AND je.fullkey LIKE '$.Event.EventData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────┐
│              fullkey               │
│              varchar               │
├────────────────────────────────────┤
│ $.Event.EventData.Address          │
│ $.Event.EventData.AddressLength    │
│ $.Event.EventData.ServerName       │
│ $.Event.EventData.ServerNameLength │
│ $.Event.EventData.SessionId        │
│ $.Event.EventData.Status           │
│ $.Event.EventData.TreeId           │
└────────────────────────────────────┘
```
Status
Status
| Return value/code | Description |
| --- | --- |
| 0x00000000<br>STATUS\_SUCCESS | The operation completed successfully. |
| 0x00000000<br>STATUS\_WAIT\_0 | The caller specified WaitAny for WaitType and one of the dispatcher objects in the Object array has been set to the signaled state. |
| 0x00000001<br>STATUS\_WAIT\_1 | The caller specified WaitAny for WaitType and one of the dispatcher objects in the Object array has been set to the signaled state. |
| 0x00000002<br>STATUS\_WAIT\_2 | The caller specified WaitAny for WaitType and one of the dispatcher objects in the Object array has been set to the signaled state. |
| 0x00000003<br>STATUS\_WAIT\_3 | The caller specified WaitAny for WaitType and one of the dispatcher objects in the Object array has been set to the signaled state. |
| 0x0000003F<br>STATUS\_WAIT\_63 | The caller specified WaitAny for WaitType and one of the dispatcher objects in the Object array has been set to the signaled state. |
| 0x00000080<br>STATUS\_ABANDONED | The caller attempted to wait for a mutex that has been abandoned. |
| 0x00000080<br>STATUS\_ABANDONED\_WAIT\_0 | The caller attempted to wait for a mutex that has been abandoned. |
| 0x000000BF<br>STATUS\_ABANDONED\_WAIT\_63 | The caller attempted to wait for a mutex that has been abandoned. |
| 0x000000C0<br>STATUS\_USER\_APC | A user-mode APC was delivered before the given Interval expired. |
| 0x00000101<br>STATUS\_ALERTED | The delay completed because the thread was alerted. |
| 0x00000102<br>STATUS\_TIMEOUT | The given Timeout interval expired. |
| 0x00000103<br>STATUS\_PENDING | The operation that was requested is pending completion. |
| 0x00000104<br>STATUS\_REPARSE | A reparse should be performed by the Object Manager because the name of the file resulted in a symbolic link. |
| 0x00000105<br>STATUS\_MORE\_ENTRIES | Returned by enumeration APIs to indicate more information is available to successive calls. |
| 0x00000106<br>STATUS\_NOT\_ALL\_ASSIGNED | Indicates not all privileges or groups that are referenced are assigned to the caller. This allows, for example, all privileges to be disabled without having to know exactly which privileges are assigned. |
| 0x00000107<br>STATUS\_SOME\_NOT\_MAPPED | Some of the information to be translated has not been translated. |
| 0x00000108<br>STATUS\_OPLOCK\_BREAK\_IN\_PROGRESS | An open/create operation completed while an opportunistic lock (oplock) break is underway. |
| 0x00000109<br>STATUS\_VOLUME\_MOUNTED | A new volume has been mounted by a file system. |
| 0x0000010A<br>STATUS\_RXACT\_COMMITTED | This success level status indicates that the transaction state already exists for the registry subtree but that a transaction commit was previously aborted. The commit has now been completed. |
| 0x0000010B<br>STATUS\_NOTIFY\_CLEANUP | Indicates that a notify change request has been completed due to closing the handle that made the notify change request. |
| 0x0000010C<br>STATUS\_NOTIFY\_ENUM\_DIR | Indicates that a notify change request is being completed and that the information is not being returned in the caller's buffer. The caller now needs to enumerate the files to find the changes. |
| 0x0000010D<br>STATUS\_NO\_QUOTAS\_FOR\_ACCOUNT | {No Quotas} No system quota limits are specifically set for this account. |
| 0x0000010E<br>STATUS\_PRIMARY\_TRANSPORT\_CONNECT\_FAILED | {Connect Failure on Primary Transport} An attempt was made to connect to the remote server %hs on the primary transport, but the connection failed. The computer WAS able to connect on a secondary transport. |
| 0x00000110<br>STATUS\_PAGE\_FAULT\_TRANSITION | The page fault was a transition fault. |
| 0x00000111<br>STATUS\_PAGE\_FAULT\_DEMAND\_ZERO | The page fault was a demand zero fault. |
| 0x00000112<br>STATUS\_PAGE\_FAULT\_COPY\_ON\_WRITE | The page fault was a demand zero fault. |
| 0x00000113<br>STATUS\_PAGE\_FAULT\_GUARD\_PAGE | The page fault was a demand zero fault. |
| 0x00000114<br>STATUS\_PAGE\_FAULT\_PAGING\_FILE | The page fault was satisfied by reading from a secondary storage device. |
| 0x00000115<br>STATUS\_CACHE\_PAGE\_LOCKED | The cached page was locked during operation. |
| 0x00000116<br>STATUS\_CRASH\_DUMP | The crash dump exists in a paging file. |
| 0x00000117<br>STATUS\_BUFFER\_ALL\_ZEROS | The specified buffer contains all zeros. |
| 0x00000118<br>STATUS\_REPARSE\_OBJECT | A reparse should be performed by the Object Manager because the name of the file resulted in a symbolic link. |
| 0x00000119<br>STATUS\_RESOURCE\_REQUIREMENTS\_CHANGED | The device has succeeded a query-stop and its resource requirements have changed. |
| 0x00000120<br>STATUS\_TRANSLATION\_COMPLETE | The translator has translated these resources into the global space and no additional translations should be performed. |
| 0x00000121<br>STATUS\_DS\_MEMBERSHIP\_EVALUATED\_LOCALLY | The directory service evaluated group memberships locally, because it was unable to contact a global catalog server. |
| 0x00000122<br>STATUS\_NOTHING\_TO\_TERMINATE | A process being terminated has no threads to terminate. |
| 0x00000123<br>STATUS\_PROCESS\_NOT\_IN\_JOB | The specified process is not part of a job. |
| 0x00000124<br>STATUS\_PROCESS\_IN\_JOB | The specified process is part of a job. |
| 0x00000125<br>STATUS\_VOLSNAP\_HIBERNATE\_READY | {Volume Shadow Copy Service} The system is now ready for hibernation. |
| 0x00000126<br>STATUS\_FSFILTER\_OP\_COMPLETED\_SUCCESSFULLY | A file system or file system filter driver has successfully completed an FsFilter operation. |
| 0x00000127<br>STATUS\_INTERRUPT\_VECTOR\_ALREADY\_CONNECTED | The specified interrupt vector was already connected. |
| 0x00000128<br>STATUS\_INTERRUPT\_STILL\_CONNECTED | The specified interrupt vector is still connected. |
| 0x00000129<br>STATUS\_PROCESS\_CLONED | The current process is a cloned process. |
| 0x0000012A<br>STATUS\_FILE\_LOCKED\_WITH\_ONLY\_READERS | The file was locked and all users of the file can only read. |
| 0x0000012B<br>STATUS\_FILE\_LOCKED\_WITH\_WRITERS | The file was locked and at least one user of the file can write. |
| 0x00000202<br>STATUS\_RESOURCEMANAGER\_READ\_ONLY | The specified ResourceManager made no changes or updates to the resource under this transaction. |
| 0x00000367<br>STATUS\_WAIT\_FOR\_OPLOCK | An operation is blocked and waiting for an oplock. |
| 0x00010001<br>DBG\_EXCEPTION\_HANDLED | Debugger handled the exception. |
| 0x00010002<br>DBG\_CONTINUE | The debugger continued. |
| 0x001C0001<br>STATUS\_FLT\_IO\_COMPLETE | The IO was completed by a filter. |
| 0xC0000467<br>STATUS\_FILE\_NOT\_AVAILABLE | The file is temporarily unavailable. |
| 0xC0000480<br>STATUS\_SHARE\_UNAVAILABLE | The share is temporarily unavailable. |
| 0xC0000721<br>STATUS\_CALLBACK\_RETURNED\_THREAD\_AFFINITY | A threadpool worker thread entered a callback at thread affinity %p and exited at affinity %p.<br>This is unexpected, indicating that the callback missed restoring the priority. |
| 0x40000000<br>STATUS\_OBJECT\_NAME\_EXISTS | {Object Exists} An attempt was made to create an object but the object name already exists. |
| 0x40000001<br>STATUS\_THREAD\_WAS\_SUSPENDED | {Thread Suspended} A thread termination occurred while the thread was suspended. The thread resumed, and termination proceeded. |
| 0x40000002<br>STATUS\_WORKING\_SET\_LIMIT\_RANGE | {Working Set Range Error} An attempt was made to set the working set minimum or maximum to values that are outside the allowable range. |
| 0x40000003<br>STATUS\_IMAGE\_NOT\_AT\_BASE | {Image Relocated} An image file could not be mapped at the address that is specified in the image file. Local fixes must be performed on this image. |
| 0x40000004<br>STATUS\_RXACT\_STATE\_CREATED | This informational level status indicates that a specified registry subtree transaction state did not yet exist and had to be created. |
| 0x40000005<br>STATUS\_SEGMENT\_NOTIFICATION | {Segment Load} A virtual DOS machine (VDM) is loading, unloading, or moving an MS-DOS or Win16 program segment image. An exception is raised so that a debugger can load, unload, or track symbols and breakpoints within these 16-bit segments. |
| 0x40000006<br>STATUS\_LOCAL\_USER\_SESSION\_KEY | {Local Session Key} A user session key was requested for a local remote procedure call (RPC) connection. The session key that is returned is a constant value and not unique to this connection. |
| 0x40000007<br>STATUS\_BAD\_CURRENT\_DIRECTORY | {Invalid Current Directory} The process cannot switch to the startup current directory %hs. Select OK to set the current directory to %hs, or select CANCEL to exit. |
| 0x40000008<br>STATUS\_SERIAL\_MORE\_WRITES | {Serial IOCTL Complete} A serial I/O operation was completed by another write to a serial port. (The IOCTL\_SERIAL\_XOFF\_COUNTER reached zero.) |
| 0x40000009<br>STATUS\_REGISTRY\_RECOVERED | {Registry Recovery} One of the files that contains the system registry data had to be recovered by using a log or alternate copy. The recovery was successful. |
| 0x4000000A<br>STATUS\_FT\_READ\_RECOVERY\_FROM\_BACKUP | {Redundant Read} To satisfy a read request, the Windows NT operating system fault-tolerant file system successfully read the requested data from a redundant copy. This was done because the file system encountered a failure on a member of the fault-tolerant volume but was unable to reassign the failing area of the device. |
| 0x4000000B<br>STATUS\_FT\_WRITE\_RECOVERY | {Redundant Write} To satisfy a write request, the Windows NT fault-tolerant file system successfully wrote a redundant copy of the information. This was done because the file system encountered a failure on a member of the fault-tolerant volume but was unable to reassign the failing area of the device. |
| 0x4000000C<br>STATUS\_SERIAL\_COUNTER\_TIMEOUT | {Serial IOCTL Timeout} A serial I/O operation completed because the time-out period expired. (The IOCTL\_SERIAL\_XOFF\_COUNTER had not reached zero.) |
| 0x4000000D<br>STATUS\_NULL\_LM\_PASSWORD | {Password Too Complex} The Windows password is too complex to be converted to a LAN Manager password. The LAN Manager password that returned is a NULL string. |
| 0x4000000E<br>STATUS\_IMAGE\_MACHINE\_TYPE\_MISMATCH | {Machine Type Mismatch} The image file %hs is valid but is for a machine type other than the current machine. Select OK to continue, or CANCEL to fail the DLL load. |
| 0x4000000F<br>STATUS\_RECEIVE\_PARTIAL | {Partial Data Received} The network transport returned partial data to its client. The remaining data will be sent later. |
| 0x40000010<br>STATUS\_RECEIVE\_EXPEDITED | {Expedited Data Received} The network transport returned data to its client that was marked as expedited by the remote system. |
| 0x40000011<br>STATUS\_RECEIVE\_PARTIAL\_EXPEDITED | {Partial Expedited Data Received} The network transport returned partial data to its client and this data was marked as expedited by the remote system. The remaining data will be sent later. |
| 0x40000012<br>STATUS\_EVENT\_DONE | {TDI Event Done} The TDI indication has completed successfully. |
| 0x40000013<br>STATUS\_EVENT\_PENDING | {TDI Event Pending} The TDI indication has entered the pending state. |
| 0x40000014<br>STATUS\_CHECKING\_FILE\_SYSTEM | Checking file system on %wZ. |
| 0x40000015<br>STATUS\_FATAL\_APP\_EXIT | {Fatal Application Exit} %hs |
| 0x40000016<br>STATUS\_PREDEFINED\_HANDLE | The specified registry key is referenced by a predefined handle. |
| 0x40000017<br>STATUS\_WAS\_UNLOCKED | {Page Unlocked} The page protection of a locked page was changed to 'No Access' and the page was unlocked from memory and from the process. |
| 0x40000018<br>STATUS\_SERVICE\_NOTIFICATION | %hs |
| 0x40000019<br>STATUS\_WAS\_LOCKED | {Page Locked} One of the pages to lock was already locked. |
| 0x4000001A<br>STATUS\_LOG\_HARD\_ERROR | Application popup: %1 : %2 |
| 0x4000001B<br>STATUS\_ALREADY\_WIN32 | A Win32 process already exists. |
| 0x4000001C<br>STATUS\_WX86\_UNSIMULATE | An exception status code that is used by the Win32 x86 emulation subsystem. |
| 0x4000001D<br>STATUS\_WX86\_CONTINUE | An exception status code that is used by the Win32 x86 emulation subsystem. |
| 0x4000001E<br>STATUS\_WX86\_SINGLE\_STEP | An exception status code that is used by the Win32 x86 emulation subsystem. |
| 0x4000001F<br>STATUS\_WX86\_BREAKPOINT | An exception status code that is used by the Win32 x86 emulation subsystem. |
| 0x40000020<br>STATUS\_WX86\_EXCEPTION\_CONTINUE | An exception status code that is used by the Win32 x86 emulation subsystem. |
| 0x40000021<br>STATUS\_WX86\_EXCEPTION\_LASTCHANCE | An exception status code that is used by the Win32 x86 emulation subsystem. |
| 0x40000022<br>STATUS\_WX86\_EXCEPTION\_CHAIN | An exception status code that is used by the Win32 x86 emulation subsystem. |
| 0x40000023<br>STATUS\_IMAGE\_MACHINE\_TYPE\_MISMATCH\_EXE | {Machine Type Mismatch} The image file %hs is valid but is for a machine type other than the current machine. |
| 0x40000024<br>STATUS\_NO\_YIELD\_PERFORMED | A yield execution was performed and no thread was available to run. |
| 0x40000025<br>STATUS\_TIMER\_RESUME\_IGNORED | The resume flag to a timer API was ignored. |
| 0x40000026<br>STATUS\_ARBITRATION\_UNHANDLED | The arbiter has deferred arbitration of these resources to its parent. |
| 0x40000027<br>STATUS\_CARDBUS\_NOT\_SUPPORTED | The device has detected a CardBus card in its slot. |
| 0x40000028<br>STATUS\_WX86\_CREATEWX86TIB | An exception status code that is used by the Win32 x86 emulation subsystem. |
| 0x40000029<br>STATUS\_MP\_PROCESSOR\_MISMATCH | The CPUs in this multiprocessor system are not all the same revision level. To use all processors, the operating system restricts itself to the features of the least capable processor in the system. If problems occur with this system, contact the CPU manufacturer to see if this mix of processors is supported. |
| 0x4000002A<br>STATUS\_HIBERNATED | The system was put into hibernation. |
| 0x4000002B<br>STATUS\_RESUME\_HIBERNATION | The system was resumed from hibernation. |
| 0x4000002C<br>STATUS\_FIRMWARE\_UPDATED | Windows has detected that the system firmware (BIOS) was updated [previous firmware date = %2, current firmware date %3]. |
| 0x4000002D<br>STATUS\_DRIVERS\_LEAKING\_LOCKED\_PAGES | A device driver is leaking locked I/O pages and is causing system degradation. The system has automatically enabled the tracking code to try and catch the culprit. |
| 0x4000002E<br>STATUS\_MESSAGE\_RETRIEVED | The ALPC message being canceled has already been retrieved from the queue on the other side. |
| 0x4000002F<br>STATUS\_SYSTEM\_POWERSTATE\_TRANSITION | The system power state is transitioning from %2 to %3. |
| 0x40000030<br>STATUS\_ALPC\_CHECK\_COMPLETION\_LIST | The receive operation was successful. Check the ALPC completion list for the received message. |
| 0x40000031<br>STATUS\_SYSTEM\_POWERSTATE\_COMPLEX\_TRANSITION | The system power state is transitioning from %2 to %3 but could enter %4. |
| 0x40000032<br>STATUS\_ACCESS\_AUDIT\_BY\_POLICY | Access to %1 is monitored by policy rule %2. |
| 0x40000033<br>STATUS\_ABANDON\_HIBERFILE | A valid hibernation file has been invalidated and should be abandoned. |
| 0x40000034<br>STATUS\_BIZRULES\_NOT\_ENABLED | Business rule scripts are disabled for the calling application. |
| 0x40000294<br>STATUS\_WAKE\_SYSTEM | The system has awoken. |
| 0x40000370<br>STATUS\_DS\_SHUTTING\_DOWN | The directory service is shutting down. |
| 0x40010001<br>DBG\_REPLY\_LATER | Debugger will reply later. |
| 0x40010002<br>DBG\_UNABLE\_TO\_PROVIDE\_HANDLE | Debugger cannot provide a handle. |
| 0x40010003<br>DBG\_TERMINATE\_THREAD | Debugger terminated the thread. |
| 0x40010004<br>DBG\_TERMINATE\_PROCESS | Debugger terminated the process. |
| 0x40010005<br>DBG\_CONTROL\_C | Debugger obtained control of C. |
| 0x40010006<br>DBG\_PRINTEXCEPTION\_C | Debugger printed an exception on control C. |
| 0x40010007<br>DBG\_RIPEXCEPTION | Debugger received a RIP exception. |
| 0x40010008<br>DBG\_CONTROL\_BREAK | Debugger received a control break. |
| 0x40010009<br>DBG\_COMMAND\_EXCEPTION | Debugger command communication exception. |
| 0x40020056<br>RPC\_NT\_UUID\_LOCAL\_ONLY | A [UUID](6b46e050-0761-44b1-858b-9b37a74ca32e#gt_c4813fc3-b2e5-4aa3-bde7-421d950d68d3) that is valid only on this computer has been allocated. |
| 0x400200AF<br>RPC\_NT\_SEND\_INCOMPLETE | Some data remains to be sent in the request buffer. |
| 0x400A0004<br>STATUS\_CTX\_CDM\_CONNECT | The Client Drive Mapping Service has connected on Terminal Connection. |
| 0x400A0005<br>STATUS\_CTX\_CDM\_DISCONNECT | The Client Drive Mapping Service has disconnected on Terminal Connection. |
| 0x4015000D<br>STATUS\_SXS\_RELEASE\_ACTIVATION\_CONTEXT | A kernel mode component is releasing a reference on an activation context. |
| 0x40190034<br>STATUS\_RECOVERY\_NOT\_NEEDED | The transactional resource manager is already consistent. Recovery is not needed. |
| 0x40190035<br>STATUS\_RM\_ALREADY\_STARTED | The transactional resource manager has already been started. |
| 0x401A000C<br>STATUS\_LOG\_NO\_RESTART | The log service encountered a log stream with no restart area. |
| 0x401B00EC<br>STATUS\_VIDEO\_DRIVER\_DEBUG\_REPORT\_REQUEST | {Display Driver Recovered From Failure} The %hs display driver has detected a failure and recovered from it. Some graphical operations might have failed. The next time you restart the machine, a dialog box appears, giving you an opportunity to upload data about this failure to Microsoft. |
| 0x401E000A<br>STATUS\_GRAPHICS\_PARTIAL\_DATA\_POPULATED | The specified buffer is not big enough to contain the entire requested dataset. Partial data is populated up to the size of the buffer.<br>The caller needs to provide a buffer of the size as specified in the partially populated buffer's content (interface specific). |
| 0x401E0117<br>STATUS\_GRAPHICS\_DRIVER\_MISMATCH | The kernel driver detected a version mismatch between it and the user mode driver. |
| 0x401E0307<br>STATUS\_GRAPHICS\_MODE\_NOT\_PINNED | No mode is pinned on the specified VidPN source/target. |
| 0x401E031E<br>STATUS\_GRAPHICS\_NO\_PREFERRED\_MODE | The specified mode set does not specify a preference for one of its modes. |
| 0x401E034B<br>STATUS\_GRAPHICS\_DATASET\_IS\_EMPTY | The specified dataset (for example, mode set, frequency range set, descriptor set, or topology) is empty. |
| 0x401E034C<br>STATUS\_GRAPHICS\_NO\_MORE\_ELEMENTS\_IN\_DATASET | The specified dataset (for example, mode set, frequency range set, descriptor set, or topology) does not contain any more elements. |
| 0x401E0351<br>STATUS\_GRAPHICS\_PATH\_CONTENT\_GEOMETRY\_TRANSFORMATION\_NOT\_PINNED | The specified content transformation is not pinned on the specified VidPN present path. |
| 0x401E042F<br>STATUS\_GRAPHICS\_UNKNOWN\_CHILD\_STATUS | The child device presence was not reliably detected. |
| 0x401E0437<br>STATUS\_GRAPHICS\_LEADLINK\_START\_DEFERRED | Starting the lead adapter in a linked configuration has been temporarily deferred. |
| 0x401E0439<br>STATUS\_GRAPHICS\_POLLING\_TOO\_FREQUENTLY | The display adapter is being polled for children too frequently at the same polling level. |
| 0x401E043A<br>STATUS\_GRAPHICS\_START\_DEFERRED | Starting the adapter has been temporarily deferred. |
| 0x40230001<br>STATUS\_NDIS\_INDICATION\_REQUIRED | The request will be completed later by an NDIS status indication. |
| 0x80000001<br>STATUS\_GUARD\_PAGE\_VIOLATION | {EXCEPTION} Guard Page Exception A page of memory that marks the end of a data structure, such as a stack or an array, has been accessed. |
| 0x80000002<br>STATUS\_DATATYPE\_MISALIGNMENT | {EXCEPTION} Alignment Fault A data type misalignment was detected in a load or store instruction. |
| 0x80000003<br>STATUS\_BREAKPOINT | {EXCEPTION} Breakpoint A breakpoint has been reached. |
| 0x80000004<br>STATUS\_SINGLE\_STEP | {EXCEPTION} Single Step A single step or trace operation has just been completed. |
| 0x80000005<br>STATUS\_BUFFER\_OVERFLOW | {Buffer Overflow} The data was too large to fit into the specified buffer. |
| 0x80000006<br>STATUS\_NO\_MORE\_FILES | {No More Files} No more files were found which match the file specification. |
| 0x80000007<br>STATUS\_WAKE\_SYSTEM\_DEBUGGER | {Kernel Debugger Awakened} The system debugger was awakened by an interrupt. |
| 0x8000000A<br>STATUS\_HANDLES\_CLOSED | {Handles Closed} Handles to objects have been automatically closed because of the requested operation. |
| 0x8000000B<br>STATUS\_NO\_INHERITANCE | {Non-Inheritable ACL} An access control list (ACL) contains no components that can be inherited. |
| 0x8000000C<br>STATUS\_GUID\_SUBSTITUTION\_MADE | {GUID Substitution} During the translation of a globally unique identifier (GUID) to a Windows security ID (SID), no administratively defined GUID prefix was found. A substitute prefix was used, which will not compromise system security. However, this might provide a more restrictive access than intended. |
| 0x8000000D<br>STATUS\_PARTIAL\_COPY | Because of protection conflicts, not all the requested bytes could be copied. |
| 0x8000000E<br>STATUS\_DEVICE\_PAPER\_EMPTY | {Out of Paper} The printer is out of paper. |
| 0x8000000F<br>STATUS\_DEVICE\_POWERED\_OFF | {Device Power Is Off} The printer power has been turned off. |
| 0x80000010<br>STATUS\_DEVICE\_OFF\_LINE | {Device Offline} The printer has been taken offline. |
| 0x80000011<br>STATUS\_DEVICE\_BUSY | {Device Busy} The device is currently busy. |
| 0x80000012<br>STATUS\_NO\_MORE\_EAS | {No More EAs} No more extended attributes (EAs) were found for the file. |
| 0x80000013<br>STATUS\_INVALID\_EA\_NAME | {Illegal EA} The specified extended attribute (EA) name contains at least one illegal character. |
| 0x80000014<br>STATUS\_EA\_LIST\_INCONSISTENT | {Inconsistent EA List} The extended attribute (EA) list is inconsistent. |
| 0x80000015<br>STATUS\_INVALID\_EA\_FLAG | {Invalid EA Flag} An invalid extended attribute (EA) flag was set. |
| 0x80000016<br>STATUS\_VERIFY\_REQUIRED | {Verifying Disk} The media has changed and a verify operation is in progress; therefore, no reads or writes can be performed to the device, except those that are used in the verify operation. |
| 0x80000017<br>STATUS\_EXTRANEOUS\_INFORMATION | {Too Much Information} The specified access control list (ACL) contained more information than was expected. |
| 0x80000018<br>STATUS\_RXACT\_COMMIT\_NECESSARY | This warning level status indicates that the transaction state already exists for the registry subtree, but that a transaction commit was previously aborted. The commit has NOT been completed but has not been rolled back either; therefore, it can still be committed, if needed. |
| 0x8000001A<br>STATUS\_NO\_MORE\_ENTRIES | {No More Entries} No more entries are available from an enumeration operation. |
| 0x8000001B<br>STATUS\_FILEMARK\_DETECTED | {Filemark Found} A filemark was detected. |
| 0x8000001C<br>STATUS\_MEDIA\_CHANGED | {Media Changed} The media has changed. |
| 0x8000001D<br>STATUS\_BUS\_RESET | {I/O Bus Reset} An I/O bus reset was detected. |
| 0x8000001E<br>STATUS\_END\_OF\_MEDIA | {End of Media} The end of the media was encountered. |
| 0x8000001F<br>STATUS\_BEGINNING\_OF\_MEDIA | The beginning of a tape or partition has been detected. |
| 0x80000020<br>STATUS\_MEDIA\_CHECK | {Media Changed} The media might have changed. |
| 0x80000021<br>STATUS\_SETMARK\_DETECTED | A tape access reached a set mark. |
| 0x80000022<br>STATUS\_NO\_DATA\_DETECTED | During a tape access, the end of the data written is reached. |
| 0x80000023<br>STATUS\_REDIRECTOR\_HAS\_OPEN\_HANDLES | The redirector is in use and cannot be unloaded. |
| 0x80000024<br>STATUS\_SERVER\_HAS\_OPEN\_HANDLES | The server is in use and cannot be unloaded. |
| 0x80000025<br>STATUS\_ALREADY\_DISCONNECTED | The specified connection has already been disconnected. |
| 0x80000026<br>STATUS\_LONGJUMP | A long jump has been executed. |
| 0x80000027<br>STATUS\_CLEANER\_CARTRIDGE\_INSTALLED | A cleaner cartridge is present in the tape library. |
| 0x80000028<br>STATUS\_PLUGPLAY\_QUERY\_VETOED | The Plug and Play query operation was not successful. |
| 0x80000029<br>STATUS\_UNWIND\_CONSOLIDATE | A frame consolidation has been executed. |
| 0x8000002A<br>STATUS\_REGISTRY\_HIVE\_RECOVERED | {Registry Hive Recovered} The registry hive (file): %hs was corrupted and it has been recovered. Some data might have been lost. |
| 0x8000002B<br>STATUS\_DLL\_MIGHT\_BE\_INSECURE | The application is attempting to run executable code from the module %hs. This might be insecure. An alternative, %hs, is available. Should the application use the secure module %hs? |
| 0x8000002C<br>STATUS\_DLL\_MIGHT\_BE\_INCOMPATIBLE | The application is loading executable code from the module %hs. This is secure but might be incompatible with previous releases of the operating system. An alternative, %hs, is available. Should the application use the secure module %hs? |
| 0x8000002D<br>STATUS\_STOPPED\_ON\_SYMLINK | The create operation stopped after reaching a symbolic link. |
| 0x80000288<br>STATUS\_DEVICE\_REQUIRES\_CLEANING | The device has indicated that cleaning is necessary. |
| 0x80000289<br>STATUS\_DEVICE\_DOOR\_OPEN | The device has indicated that its door is open. Further operations require it closed and secured. |
| 0x80000803<br>STATUS\_DATA\_LOST\_REPAIR | Windows discovered a corruption in the file %hs. This file has now been repaired. Check if any data in the file was lost because of the corruption. |
| 0x80010001<br>DBG\_EXCEPTION\_NOT\_HANDLED | Debugger did not handle the exception. |
| 0x80130001<br>STATUS\_CLUSTER\_NODE\_ALREADY\_UP | The cluster node is already up. |
| 0x80130002<br>STATUS\_CLUSTER\_NODE\_ALREADY\_DOWN | The cluster node is already down. |
| 0x80130003<br>STATUS\_CLUSTER\_NETWORK\_ALREADY\_ONLINE | The cluster network is already online. |
| 0x80130004<br>STATUS\_CLUSTER\_NETWORK\_ALREADY\_OFFLINE | The cluster network is already offline. |
| 0x80130005<br>STATUS\_CLUSTER\_NODE\_ALREADY\_MEMBER | The cluster node is already a member of the cluster. |
| 0x80190009<br>STATUS\_COULD\_NOT\_RESIZE\_LOG | The log could not be set to the requested size. |
| 0x80190029<br>STATUS\_NO\_TXF\_METADATA | There is no transaction metadata on the file. |
| 0x80190031<br>STATUS\_CANT\_RECOVER\_WITH\_HANDLE\_OPEN | The file cannot be recovered because there is a handle still open on it. |
| 0x80190041<br>STATUS\_TXF\_METADATA\_ALREADY\_PRESENT | Transaction metadata is already present on this file and cannot be superseded. |
| 0x80190042<br>STATUS\_TRANSACTION\_SCOPE\_CALLBACKS\_NOT\_SET | A transaction scope could not be entered because the scope handler has not been initialized. |
| 0x801B00EB<br>STATUS\_VIDEO\_HUNG\_DISPLAY\_DRIVER\_THREAD\_RECOVERED | {Display Driver Stopped Responding and recovered} The %hs display driver has stopped working normally. The recovery had been performed. |
| 0x801C0001<br>STATUS\_FLT\_BUFFER\_TOO\_SMALL | {Buffer too small} The buffer is too small to contain the entry. No information has been written to the buffer. |
| 0x80210001<br>STATUS\_FVE\_PARTIAL\_METADATA | Volume metadata read or write is incomplete. |
| 0x80210002<br>STATUS\_FVE\_TRANSIENT\_STATE | BitLocker encryption keys were ignored because the volume was in a transient state. |
| 0xC0000001<br>STATUS\_UNSUCCESSFUL | {Operation Failed} The requested operation was unsuccessful. |
| 0xC0000002<br>STATUS\_NOT\_IMPLEMENTED | {Not Implemented} The requested operation is not implemented. |
| 0xC0000003<br>STATUS\_INVALID\_INFO\_CLASS | {Invalid Parameter} The specified information class is not a valid information class for the specified object. |
| 0xC0000004<br>STATUS\_INFO\_LENGTH\_MISMATCH | The specified information record length does not match the length that is required for the specified information class. |
| 0xC0000005<br>STATUS\_ACCESS\_VIOLATION | The instruction at 0x%08lx referenced memory at 0x%08lx. The memory could not be %s. |
| 0xC0000006<br>STATUS\_IN\_PAGE\_ERROR | The instruction at 0x%08lx referenced memory at 0x%08lx. The required data was not placed into memory because of an I/O error status of 0x%08lx. |
| 0xC0000007<br>STATUS\_PAGEFILE\_QUOTA | The page file quota for the process has been exhausted. |
| 0xC0000008<br>STATUS\_INVALID\_HANDLE | An invalid HANDLE was specified. |
| 0xC0000009<br>STATUS\_BAD\_INITIAL\_STACK | An invalid initial stack was specified in a call to NtCreateThread. |
| 0xC000000A<br>STATUS\_BAD\_INITIAL\_PC | An invalid initial start address was specified in a call to NtCreateThread. |
| 0xC000000B<br>STATUS\_INVALID\_CID | An invalid client ID was specified. |
| 0xC000000C<br>STATUS\_TIMER\_NOT\_CANCELED | An attempt was made to cancel or set a timer that has an associated APC and the specified thread is not the thread that originally set the timer with an associated APC routine. |
| 0xC000000D<br>STATUS\_INVALID\_PARAMETER | An invalid parameter was passed to a service or function. |
| 0xC000000E<br>STATUS\_NO\_SUCH\_DEVICE | A device that does not exist was specified. |
| 0xC000000F<br>STATUS\_NO\_SUCH\_FILE | {File Not Found} The file %hs does not exist. |
| 0xC0000010<br>STATUS\_INVALID\_DEVICE\_REQUEST | The specified request is not a valid operation for the target device. |
| 0xC0000011<br>STATUS\_END\_OF\_FILE | The end-of-file marker has been reached. There is no valid data in the file beyond this marker. |
| 0xC0000012<br>STATUS\_WRONG\_VOLUME | {Wrong Volume} The wrong volume is in the drive. Insert volume %hs into drive %hs. |
| 0xC0000013<br>STATUS\_NO\_MEDIA\_IN\_DEVICE | {No Disk} There is no disk in the drive. Insert a disk into drive %hs. |
| 0xC0000014<br>STATUS\_UNRECOGNIZED\_MEDIA | {Unknown Disk Format} The disk in drive %hs is not formatted properly. Check the disk, and reformat it, if needed. |
| 0xC0000015<br>STATUS\_NONEXISTENT\_SECTOR | {Sector Not Found} The specified sector does not exist. |
| 0xC0000016<br>STATUS\_MORE\_PROCESSING\_REQUIRED | {Still Busy} The specified I/O request packet (IRP) cannot be disposed of because the I/O operation is not complete. |
| 0xC0000017<br>STATUS\_NO\_MEMORY | {Not Enough Quota} Not enough virtual memory or paging file quota is available to complete the specified operation. |
| 0xC0000018<br>STATUS\_CONFLICTING\_ADDRESSES | {Conflicting Address Range} The specified address range conflicts with the address space. |
| 0xC0000019<br>STATUS\_NOT\_MAPPED\_VIEW | The address range to unmap is not a mapped view. |
| 0xC000001A<br>STATUS\_UNABLE\_TO\_FREE\_VM | The virtual memory cannot be freed. |
| 0xC000001B<br>STATUS\_UNABLE\_TO\_DELETE\_SECTION | The specified section cannot be deleted. |
| 0xC000001C<br>STATUS\_INVALID\_SYSTEM\_SERVICE | An invalid system service was specified in a system service call. |
| 0xC000001D<br>STATUS\_ILLEGAL\_INSTRUCTION | {EXCEPTION} Illegal Instruction An attempt was made to execute an illegal instruction. |
| 0xC000001E<br>STATUS\_INVALID\_LOCK\_SEQUENCE | {Invalid Lock Sequence} An attempt was made to execute an invalid lock sequence. |
| 0xC000001F<br>STATUS\_INVALID\_VIEW\_SIZE | {Invalid Mapping} An attempt was made to create a view for a section that is bigger than the section. |
| 0xC0000020<br>STATUS\_INVALID\_FILE\_FOR\_SECTION | {Bad File} The attributes of the specified mapping file for a section of memory cannot be read. |
| 0xC0000021<br>STATUS\_ALREADY\_COMMITTED | {Already Committed} The specified address range is already committed. |
| 0xC0000022<br>STATUS\_ACCESS\_DENIED | {Access Denied} A process has requested access to an object but has not been granted those access rights. |
| 0xC0000023<br>STATUS\_BUFFER\_TOO\_SMALL | {Buffer Too Small} The buffer is too small to contain the entry. No information has been written to the buffer. |
| 0xC0000024<br>STATUS\_OBJECT\_TYPE\_MISMATCH | {Wrong Type} There is a mismatch between the type of object that is required by the requested operation and the type of object that is specified in the request. |
| 0xC0000025<br>STATUS\_NONCONTINUABLE\_EXCEPTION | {EXCEPTION} Cannot Continue Windows cannot continue from this exception. |
| 0xC0000026<br>STATUS\_INVALID\_DISPOSITION | An invalid exception disposition was returned by an exception handler. |
| 0xC0000027<br>STATUS\_UNWIND | Unwind exception code. |
| 0xC0000028<br>STATUS\_BAD\_STACK | An invalid or unaligned stack was encountered during an unwind operation. |
| 0xC0000029<br>STATUS\_INVALID\_UNWIND\_TARGET | An invalid unwind target was encountered during an unwind operation. |
| 0xC000002A<br>STATUS\_NOT\_LOCKED | An attempt was made to unlock a page of memory that was not locked. |
| 0xC000002B<br>STATUS\_PARITY\_ERROR | A device parity error on an I/O operation. |
| 0xC000002C<br>STATUS\_UNABLE\_TO\_DECOMMIT\_VM | An attempt was made to decommit uncommitted virtual memory. |
| 0xC000002D<br>STATUS\_NOT\_COMMITTED | An attempt was made to change the attributes on memory that has not been committed. |
| 0xC000002E<br>STATUS\_INVALID\_PORT\_ATTRIBUTES | Invalid object attributes specified to NtCreatePort or invalid port attributes specified to NtConnectPort. |
| 0xC000002F<br>STATUS\_PORT\_MESSAGE\_TOO\_LONG | The length of the message that was passed to NtRequestPort or NtRequestWaitReplyPort is longer than the maximum message that is allowed by the port. |
| 0xC0000030<br>STATUS\_INVALID\_PARAMETER\_MIX | An invalid combination of parameters was specified. |
| 0xC0000031<br>STATUS\_INVALID\_QUOTA\_LOWER | An attempt was made to lower a quota limit below the current usage. |
| 0xC0000032<br>STATUS\_DISK\_CORRUPT\_ERROR | {Corrupt Disk} The file system structure on the disk is corrupt and unusable. Run the Chkdsk utility on the volume %hs. |
| 0xC0000033<br>STATUS\_OBJECT\_NAME\_INVALID | The object name is invalid. |
| 0xC0000034<br>STATUS\_OBJECT\_NAME\_NOT\_FOUND | The object name is not found. |
| 0xC0000035<br>STATUS\_OBJECT\_NAME\_COLLISION | The object name already exists. |
| 0xC0000037<br>STATUS\_PORT\_DISCONNECTED | An attempt was made to send a message to a disconnected communication port. |
| 0xC0000038<br>STATUS\_DEVICE\_ALREADY\_ATTACHED | An attempt was made to attach to a device that was already attached to another device. |
| 0xC0000039<br>STATUS\_OBJECT\_PATH\_INVALID | The object path component was not a directory object. |
| 0xC000003A<br>STATUS\_OBJECT\_PATH\_NOT\_FOUND | {Path Not Found} The path %hs does not exist. |
| 0xC000003B<br>STATUS\_OBJECT\_PATH\_SYNTAX\_BAD | The object path component was not a directory object. |
| 0xC000003C<br>STATUS\_DATA\_OVERRUN | {Data Overrun} A data overrun error occurred. |
| 0xC000003D<br>STATUS\_DATA\_LATE\_ERROR | {Data Late} A data late error occurred. |
| 0xC000003E<br>STATUS\_DATA\_ERROR | {Data Error} An error occurred in reading or writing data. |
| 0xC000003F<br>STATUS\_CRC\_ERROR | {Bad CRC} A cyclic redundancy check (CRC) checksum error occurred. |
| 0xC0000040<br>STATUS\_SECTION\_TOO\_BIG | {Section Too Large} The specified section is too big to map the file. |
| 0xC0000041<br>STATUS\_PORT\_CONNECTION\_REFUSED | The NtConnectPort request is refused. |
| 0xC0000042<br>STATUS\_INVALID\_PORT\_HANDLE | The type of port handle is invalid for the operation that is requested. |
| 0xC0000043<br>STATUS\_SHARING\_VIOLATION | A file cannot be opened because the share access flags are incompatible. |
| 0xC0000044<br>STATUS\_QUOTA\_EXCEEDED | Insufficient quota exists to complete the operation. |
| 0xC0000045<br>STATUS\_INVALID\_PAGE\_PROTECTION | The specified page protection was not valid. |
| 0xC0000046<br>STATUS\_MUTANT\_NOT\_OWNED | An attempt to release a mutant object was made by a thread that was not the owner of the mutant object. |
| 0xC0000047<br>STATUS\_SEMAPHORE\_LIMIT\_EXCEEDED | An attempt was made to release a semaphore such that its maximum count would have been exceeded. |
| 0xC0000048<br>STATUS\_PORT\_ALREADY\_SET | An attempt was made to set the DebugPort or ExceptionPort of a process, but a port already exists in the process, or an attempt was made to set the CompletionPort of a file but a port was already set in the file, or an attempt was made to set the associated completion port of an ALPC port but it is already set. |
| 0xC0000049<br>STATUS\_SECTION\_NOT\_IMAGE | An attempt was made to query image information on a section that does not map an image. |
| 0xC000004A<br>STATUS\_SUSPEND\_COUNT\_EXCEEDED | An attempt was made to suspend a thread whose suspend count was at its maximum. |
| 0xC000004B<br>STATUS\_THREAD\_IS\_TERMINATING | An attempt was made to suspend a thread that has begun termination. |
| 0xC000004C<br>STATUS\_BAD\_WORKING\_SET\_LIMIT | An attempt was made to set the working set limit to an invalid value (for example, the minimum greater than maximum). |
| 0xC000004D<br>STATUS\_INCOMPATIBLE\_FILE\_MAP | A section was created to map a file that is not compatible with an already existing section that maps the same file. |
| 0xC000004E<br>STATUS\_SECTION\_PROTECTION | A view to a section specifies a protection that is incompatible with the protection of the initial view. |
| 0xC000004F<br>STATUS\_EAS\_NOT\_SUPPORTED | An operation involving EAs failed because the file system does not support EAs. |
| 0xC0000050<br>STATUS\_EA\_TOO\_LARGE | An EA operation failed because the EA set is too large. |
| 0xC0000051<br>STATUS\_NONEXISTENT\_EA\_ENTRY | An EA operation failed because the name or EA index is invalid. |
| 0xC0000052<br>STATUS\_NO\_EAS\_ON\_FILE | The file for which EAs were requested has no EAs. |
| 0xC0000053<br>STATUS\_EA\_CORRUPT\_ERROR | The EA is corrupt and cannot be read. |
| 0xC0000054<br>STATUS\_FILE\_LOCK\_CONFLICT | A requested read/write cannot be granted due to a conflicting file lock. |
| 0xC0000055<br>STATUS\_LOCK\_NOT\_GRANTED | A requested file lock cannot be granted due to other existing locks. |
| 0xC0000056<br>STATUS\_DELETE\_PENDING | A non-close operation has been requested of a file object that has a delete pending. |
| 0xC0000057<br>STATUS\_CTL\_FILE\_NOT\_SUPPORTED | An attempt was made to set the control attribute on a file. This attribute is not supported in the destination file system. |
| 0xC0000058<br>STATUS\_UNKNOWN\_REVISION | Indicates a revision number that was encountered or specified is not one that is known by the service. It might be a more recent revision than the service is aware of. |
| 0xC0000059<br>STATUS\_REVISION\_MISMATCH | Indicates that two revision levels are incompatible. |
| 0xC000005A<br>STATUS\_INVALID\_OWNER | Indicates a particular security ID cannot be assigned as the owner of an object. |
| 0xC000005B<br>STATUS\_INVALID\_PRIMARY\_GROUP | Indicates a particular security ID cannot be assigned as the primary group of an object. |
| 0xC000005C<br>STATUS\_NO\_IMPERSONATION\_TOKEN | An attempt has been made to operate on an impersonation token by a thread that is not currently impersonating a client. |
| 0xC000005D<br>STATUS\_CANT\_DISABLE\_MANDATORY | A mandatory group cannot be disabled. |
| 0xC000005E<br>STATUS\_NO\_LOGON\_SERVERS | No logon servers are currently available to service the logon request. |
| 0xC000005F<br>STATUS\_NO\_SUCH\_LOGON\_SESSION | A specified logon session does not exist. It might already have been terminated. |
| 0xC0000060<br>STATUS\_NO\_SUCH\_PRIVILEGE | A specified privilege does not exist. |
| 0xC0000061<br>STATUS\_PRIVILEGE\_NOT\_HELD | A required privilege is not held by the client. |
| 0xC0000062<br>STATUS\_INVALID\_ACCOUNT\_NAME | The name provided is not a properly formed account name. |
| 0xC0000063<br>STATUS\_USER\_EXISTS | The specified account already exists. |
| 0xC0000064<br>STATUS\_NO\_SUCH\_USER | The specified account does not exist. |
| 0xC0000065<br>STATUS\_GROUP\_EXISTS | The specified group already exists. |
| 0xC0000066<br>STATUS\_NO\_SUCH\_GROUP | The specified group does not exist. |
| 0xC0000067<br>STATUS\_MEMBER\_IN\_GROUP | The specified user account is already in the specified group account. Also used to indicate a group cannot be deleted because it contains a member. |
| 0xC0000068<br>STATUS\_MEMBER\_NOT\_IN\_GROUP | The specified user account is not a member of the specified group account. |
| 0xC0000069<br>STATUS\_LAST\_ADMIN | Indicates the requested operation would disable or delete the last remaining administration account. This is not allowed to prevent creating a situation in which the system cannot be administrated. |
| 0xC000006A<br>STATUS\_WRONG\_PASSWORD | When trying to update a password, this return status indicates that the value provided as the current password is not correct. |
| 0xC000006B<br>STATUS\_ILL\_FORMED\_PASSWORD | When trying to update a password, this return status indicates that the value provided for the new password contains values that are not allowed in passwords. |
| 0xC000006C<br>STATUS\_PASSWORD\_RESTRICTION | When trying to update a password, this status indicates that some password update rule has been violated. For example, the password might not meet length criteria. |
| 0xC000006D<br>STATUS\_LOGON\_FAILURE | The attempted logon is invalid. This is either due to a bad username or authentication information. |
| 0xC000006E<br>STATUS\_ACCOUNT\_RESTRICTION | Indicates a referenced user name and authentication information are valid, but some user account restriction has prevented successful authentication (such as time-of-day restrictions). |
| 0xC000006F<br>STATUS\_INVALID\_LOGON\_HOURS | The user account has time restrictions and cannot be logged onto at this time. |
| 0xC0000070<br>STATUS\_INVALID\_WORKSTATION | The user account is restricted so that it cannot be used to log on from the source workstation. |
| 0xC0000071<br>STATUS\_PASSWORD\_EXPIRED | The user account password has expired. |
| 0xC0000072<br>STATUS\_ACCOUNT\_DISABLED | The referenced account is currently disabled and cannot be logged on to. |
| 0xC0000073<br>STATUS\_NONE\_MAPPED | None of the information to be translated has been translated. |
| 0xC0000074<br>STATUS\_TOO\_MANY\_LUIDS\_REQUESTED | The number of LUIDs requested cannot be allocated with a single allocation. |
| 0xC0000075<br>STATUS\_LUIDS\_EXHAUSTED | Indicates there are no more LUIDs to allocate. |
| 0xC0000076<br>STATUS\_INVALID\_SUB\_AUTHORITY | Indicates the sub-authority value is invalid for the particular use. |
| 0xC0000077<br>STATUS\_INVALID\_ACL | Indicates the ACL structure is not valid. |
| 0xC0000078<br>STATUS\_INVALID\_SID | Indicates the SID structure is not valid. |
| 0xC0000079<br>STATUS\_INVALID\_SECURITY\_DESCR | Indicates the SECURITY\_DESCRIPTOR structure is not valid. |
| 0xC000007A<br>STATUS\_PROCEDURE\_NOT\_FOUND | Indicates the specified procedure address cannot be found in the DLL. |
| 0xC000007B<br>STATUS\_INVALID\_IMAGE\_FORMAT | {Bad Image} %hs is either not designed to run on Windows or it contains an error. Try installing the program again using the original installation media or contact your system administrator or the software vendor for support. |
| 0xC000007C<br>STATUS\_NO\_TOKEN | An attempt was made to reference a token that does not exist. This is typically done by referencing the token that is associated with a thread when the thread is not impersonating a client. |
| 0xC000007D<br>STATUS\_BAD\_INHERITANCE\_ACL | Indicates that an attempt to build either an inherited ACL or ACE was not successful. This can be caused by a number of things. One of the more probable causes is the replacement of a CreatorId with a SID that did not fit into the ACE or ACL. |
| 0xC000007E<br>STATUS\_RANGE\_NOT\_LOCKED | The range specified in NtUnlockFile was not locked. |
| 0xC000007F<br>STATUS\_DISK\_FULL | An operation failed because the disk was full. |
| 0xC0000080<br>STATUS\_SERVER\_DISABLED | The GUID allocation server is disabled at the moment. |
| 0xC0000081<br>STATUS\_SERVER\_NOT\_DISABLED | The GUID allocation server is enabled at the moment. |
| 0xC0000082<br>STATUS\_TOO\_MANY\_GUIDS\_REQUESTED | Too many GUIDs were requested from the allocation server at once. |
| 0xC0000083<br>STATUS\_GUIDS\_EXHAUSTED | The GUIDs could not be allocated because the Authority Agent was exhausted. |
| 0xC0000084<br>STATUS\_INVALID\_ID\_AUTHORITY | The value provided was an invalid value for an identifier authority. |
| 0xC0000085<br>STATUS\_AGENTS\_EXHAUSTED | No more authority agent values are available for the particular identifier authority value. |
| 0xC0000086<br>STATUS\_INVALID\_VOLUME\_LABEL | An invalid volume label has been specified. |
| 0xC0000087<br>STATUS\_SECTION\_NOT\_EXTENDED | A mapped section could not be extended. |
| 0xC0000088<br>STATUS\_NOT\_MAPPED\_DATA | Specified section to flush does not map a data file. |
| 0xC0000089<br>STATUS\_RESOURCE\_DATA\_NOT\_FOUND | Indicates the specified image file did not contain a resource section. |
| 0xC000008A<br>STATUS\_RESOURCE\_TYPE\_NOT\_FOUND | Indicates the specified resource type cannot be found in the image file. |
| 0xC000008B<br>STATUS\_RESOURCE\_NAME\_NOT\_FOUND | Indicates the specified resource name cannot be found in the image file. |
| 0xC000008C<br>STATUS\_ARRAY\_BOUNDS\_EXCEEDED | {EXCEPTION} Array bounds exceeded. |
| 0xC000008D<br>STATUS\_FLOAT\_DENORMAL\_OPERAND | {EXCEPTION} Floating-point denormal operand. |
| 0xC000008E<br>STATUS\_FLOAT\_DIVIDE\_BY\_ZERO | {EXCEPTION} Floating-point division by zero. |
| 0xC000008F<br>STATUS\_FLOAT\_INEXACT\_RESULT | {EXCEPTION} Floating-point inexact result. |
| 0xC0000090<br>STATUS\_FLOAT\_INVALID\_OPERATION | {EXCEPTION} Floating-point invalid operation. |
| 0xC0000091<br>STATUS\_FLOAT\_OVERFLOW | {EXCEPTION} Floating-point overflow. |
| 0xC0000092<br>STATUS\_FLOAT\_STACK\_CHECK | {EXCEPTION} Floating-point stack check. |
| 0xC0000093<br>STATUS\_FLOAT\_UNDERFLOW | {EXCEPTION} Floating-point underflow. |
| 0xC0000094<br>STATUS\_INTEGER\_DIVIDE\_BY\_ZERO | {EXCEPTION} Integer division by zero. |
| 0xC0000095<br>STATUS\_INTEGER\_OVERFLOW | {EXCEPTION} Integer overflow. |
| 0xC0000096<br>STATUS\_PRIVILEGED\_INSTRUCTION | {EXCEPTION} Privileged instruction. |
| 0xC0000097<br>STATUS\_TOO\_MANY\_PAGING\_FILES | An attempt was made to install more paging files than the system supports. |
| 0xC0000098<br>STATUS\_FILE\_INVALID | The volume for a file has been externally altered such that the opened file is no longer valid. |
| 0xC0000099<br>STATUS\_ALLOTTED\_SPACE\_EXCEEDED | When a block of memory is allotted for future updates, such as the memory allocated to hold discretionary access control and primary group information, successive updates might exceed the amount of memory originally allotted. Because a quota might already have been charged to several processes that have handles to the object, it is not reasonable to alter the size of the allocated memory. Instead, a request that requires more memory than has been allotted must fail and the STATUS\_ALLOTTED\_SPACE\_EXCEEDED error returned. |
| 0xC000009A<br>STATUS\_INSUFFICIENT\_RESOURCES | Insufficient system resources exist to complete the API. |
| 0xC000009B<br>STATUS\_DFS\_EXIT\_PATH\_FOUND | An attempt has been made to open a DFS exit path control file. |
| 0xC000009C<br>STATUS\_DEVICE\_DATA\_ERROR | There are bad blocks (sectors) on the hard disk. |
| 0xC000009D<br>STATUS\_DEVICE\_NOT\_CONNECTED | There is bad cabling, non-termination, or the controller is not able to obtain access to the hard disk. |
| 0xC000009F<br>STATUS\_FREE\_VM\_NOT\_AT\_BASE | Virtual memory cannot be freed because the base address is not the base of the region and a region size of zero was specified. |
| 0xC00000A0<br>STATUS\_MEMORY\_NOT\_ALLOCATED | An attempt was made to free virtual memory that is not allocated. |
| 0xC00000A1<br>STATUS\_WORKING\_SET\_QUOTA | The working set is not big enough to allow the requested pages to be locked. |
| 0xC00000A2<br>STATUS\_MEDIA\_WRITE\_PROTECTED | {Write Protect Error} The disk cannot be written to because it is write-protected. Remove the write protection from the volume %hs in drive %hs. |
| 0xC00000A3<br>STATUS\_DEVICE\_NOT\_READY | {Drive Not Ready} The drive is not ready for use; its door might be open. Check drive %hs and make sure that a disk is inserted and that the drive door is closed. |
| 0xC00000A4<br>STATUS\_INVALID\_GROUP\_ATTRIBUTES | The specified attributes are invalid or are incompatible with the attributes for the group as a whole. |
| 0xC00000A5<br>STATUS\_BAD\_IMPERSONATION\_LEVEL | A specified impersonation level is invalid. Also used to indicate that a required impersonation level was not provided. |
| 0xC00000A6<br>STATUS\_CANT\_OPEN\_ANONYMOUS | An attempt was made to open an anonymous-level token. Anonymous tokens cannot be opened. |
| 0xC00000A7<br>STATUS\_BAD\_VALIDATION\_CLASS | The validation information class requested was invalid. |
| 0xC00000A8<br>STATUS\_BAD\_TOKEN\_TYPE | The type of a token object is inappropriate for its attempted use. |
| 0xC00000A9<br>STATUS\_BAD\_MASTER\_BOOT\_RECORD | The type of a token object is inappropriate for its attempted use. |
| 0xC00000AA<br>STATUS\_INSTRUCTION\_MISALIGNMENT | An attempt was made to execute an instruction at an unaligned address and the host system does not support unaligned instruction references. |
| 0xC00000AB<br>STATUS\_INSTANCE\_NOT\_AVAILABLE | The maximum named pipe instance count has been reached. |
| 0xC00000AC<br>STATUS\_PIPE\_NOT\_AVAILABLE | An instance of a named pipe cannot be found in the listening state. |
| 0xC00000AD<br>STATUS\_INVALID\_PIPE\_STATE | The named pipe is not in the connected or closing state. |
| 0xC00000AE<br>STATUS\_PIPE\_BUSY | The specified pipe is set to complete operations and there are current I/O operations queued so that it cannot be changed to queue operations. |
| 0xC00000AF<br>STATUS\_ILLEGAL\_FUNCTION | The specified handle is not open to the server end of the named pipe. |
| 0xC00000B0<br>STATUS\_PIPE\_DISCONNECTED | The specified named pipe is in the disconnected state. |
| 0xC00000B1<br>STATUS\_PIPE\_CLOSING | The specified named pipe is in the closing state. |
| 0xC00000B2<br>STATUS\_PIPE\_CONNECTED | The specified named pipe is in the connected state. |
| 0xC00000B3<br>STATUS\_PIPE\_LISTENING | The specified named pipe is in the listening state. |
| 0xC00000B4<br>STATUS\_INVALID\_READ\_MODE | The specified named pipe is not in message mode. |
| 0xC00000B5<br>STATUS\_IO\_TIMEOUT | {Device Timeout} The specified I/O operation on %hs was not completed before the time-out period expired. |
| 0xC00000B6<br>STATUS\_FILE\_FORCED\_CLOSED | The specified file has been closed by another process. |
| 0xC00000B7<br>STATUS\_PROFILING\_NOT\_STARTED | Profiling is not started. |
| 0xC00000B8<br>STATUS\_PROFILING\_NOT\_STOPPED | Profiling is not stopped. |
| 0xC00000B9<br>STATUS\_COULD\_NOT\_INTERPRET | The passed ACL did not contain the minimum required information. |
| 0xC00000BA<br>STATUS\_FILE\_IS\_A\_DIRECTORY | The file that was specified as a target is a directory, and the caller specified that it could be anything but a directory. |
| 0xC00000BB<br>STATUS\_NOT\_SUPPORTED | The request is not supported. |
| 0xC00000BC<br>STATUS\_REMOTE\_NOT\_LISTENING | This remote computer is not listening. |
| 0xC00000BD<br>STATUS\_DUPLICATE\_NAME | A duplicate name exists on the network. |
| 0xC00000BE<br>STATUS\_BAD\_NETWORK\_PATH | The network path cannot be located. |
| 0xC00000BF<br>STATUS\_NETWORK\_BUSY | The network is busy. |
| 0xC00000C0<br>STATUS\_DEVICE\_DOES\_NOT\_EXIST | This device does not exist. |
| 0xC00000C1<br>STATUS\_TOO\_MANY\_COMMANDS | The network BIOS command limit has been reached. |
| 0xC00000C2<br>STATUS\_ADAPTER\_HARDWARE\_ERROR | An I/O adapter hardware error has occurred. |
| 0xC00000C3<br>STATUS\_INVALID\_NETWORK\_RESPONSE | The network responded incorrectly. |
| 0xC00000C4<br>STATUS\_UNEXPECTED\_NETWORK\_ERROR | An unexpected network error occurred. |
| 0xC00000C5<br>STATUS\_BAD\_REMOTE\_ADAPTER | The remote adapter is not compatible. |
| 0xC00000C6<br>STATUS\_PRINT\_QUEUE\_FULL | The print queue is full. |
| 0xC00000C7<br>STATUS\_NO\_SPOOL\_SPACE | Space to store the file that is waiting to be printed is not available on the server. |
| 0xC00000C8<br>STATUS\_PRINT\_CANCELLED | The requested print file has been canceled. |
| 0xC00000C9<br>STATUS\_NETWORK\_NAME\_DELETED | The network name was deleted. |
| 0xC00000CA<br>STATUS\_NETWORK\_ACCESS\_DENIED | Network access is denied. |
| 0xC00000CB<br>STATUS\_BAD\_DEVICE\_TYPE | {Incorrect Network Resource Type} The specified device type (LPT, for example) conflicts with the actual device type on the remote resource. |
| 0xC00000CC<br>STATUS\_BAD\_NETWORK\_NAME | {Network Name Not Found} The specified share name cannot be found on the remote server. |
| 0xC00000CD<br>STATUS\_TOO\_MANY\_NAMES | The name limit for the network adapter card of the local computer was exceeded. |
| 0xC00000CE<br>STATUS\_TOO\_MANY\_SESSIONS | The network BIOS session limit was exceeded. |
| 0xC00000CF<br>STATUS\_SHARING\_PAUSED | File sharing has been temporarily paused. |
| 0xC00000D0<br>STATUS\_REQUEST\_NOT\_ACCEPTED | No more connections can be made to this remote computer at this time because the computer has already accepted the maximum number of connections. |
| 0xC00000D1<br>STATUS\_REDIRECTOR\_PAUSED | Print or disk redirection is temporarily paused. |
| 0xC00000D2<br>STATUS\_NET\_WRITE\_FAULT | A network data fault occurred. |
| 0xC00000D3<br>STATUS\_PROFILING\_AT\_LIMIT | The number of active profiling objects is at the maximum and no more can be started. |
| 0xC00000D4<br>STATUS\_NOT\_SAME\_DEVICE | {Incorrect Volume} The destination file of a rename request is located on a different device than the source of the rename request. |
| 0xC00000D5<br>STATUS\_FILE\_RENAMED | The specified file has been renamed and thus cannot be modified. |
| 0xC00000D6<br>STATUS\_VIRTUAL\_CIRCUIT\_CLOSED | {Network Request Timeout} The session with a remote server has been disconnected because the time-out interval for a request has expired. |
| 0xC00000D7<br>STATUS\_NO\_SECURITY\_ON\_OBJECT | Indicates an attempt was made to operate on the security of an object that does not have security associated with it. |
| 0xC00000D8<br>STATUS\_CANT\_WAIT | Used to indicate that an operation cannot continue without blocking for I/O. |
| 0xC00000D9<br>STATUS\_PIPE\_EMPTY | Used to indicate that a read operation was done on an empty pipe. |
| 0xC00000DA<br>STATUS\_CANT\_ACCESS\_DOMAIN\_INFO | Configuration information could not be read from the domain controller, either because the machine is unavailable or access has been denied. |
| 0xC00000DB<br>STATUS\_CANT\_TERMINATE\_SELF | Indicates that a thread attempted to terminate itself by default (called NtTerminateThread with NULL) and it was the last thread in the current process. |
| 0xC00000DC<br>STATUS\_INVALID\_SERVER\_STATE | Indicates the Sam Server was in the wrong state to perform the desired operation. |
| 0xC00000DD<br>STATUS\_INVALID\_DOMAIN\_STATE | Indicates the domain was in the wrong state to perform the desired operation. |
| 0xC00000DE<br>STATUS\_INVALID\_DOMAIN\_ROLE | This operation is only allowed for the primary domain controller of the domain. |
| 0xC00000DF<br>STATUS\_NO\_SUCH\_DOMAIN | The specified domain did not exist. |
| 0xC00000E0<br>STATUS\_DOMAIN\_EXISTS | The specified domain already exists. |
| 0xC00000E1<br>STATUS\_DOMAIN\_LIMIT\_EXCEEDED | An attempt was made to exceed the limit on the number of domains per server for this release. |
| 0xC00000E2<br>STATUS\_OPLOCK\_NOT\_GRANTED | An error status returned when the opportunistic lock (oplock) request is denied. |
| 0xC00000E3<br>STATUS\_INVALID\_OPLOCK\_PROTOCOL | An error status returned when an invalid opportunistic lock (oplock) acknowledgment is received by a file system. |
| 0xC00000E4<br>STATUS\_INTERNAL\_DB\_CORRUPTION | This error indicates that the requested operation cannot be completed due to a catastrophic media failure or an on-disk data structure corruption. |
| 0xC00000E5<br>STATUS\_INTERNAL\_ERROR | An internal error occurred. |
| 0xC00000E6<br>STATUS\_GENERIC\_NOT\_MAPPED | Indicates generic access types were contained in an access mask which should already be mapped to non-generic access types. |
| 0xC00000E7<br>STATUS\_BAD\_DESCRIPTOR\_FORMAT | Indicates a security descriptor is not in the necessary format (absolute or self-relative). |
| 0xC00000E8<br>STATUS\_INVALID\_USER\_BUFFER | An access to a user buffer failed at an expected point in time. This code is defined because the caller does not want to accept STATUS\_ACCESS\_VIOLATION in its filter. |
| 0xC00000E9<br>STATUS\_UNEXPECTED\_IO\_ERROR | If an I/O error that is not defined in the standard FsRtl filter is returned, it is converted to the following error, which is guaranteed to be in the filter. In this case, information is lost; however, the filter correctly handles the exception. |
| 0xC00000EA<br>STATUS\_UNEXPECTED\_MM\_CREATE\_ERR | If an MM error that is not defined in the standard FsRtl filter is returned, it is converted to one of the following errors, which are guaranteed to be in the filter. In this case, information is lost; however, the filter correctly handles the exception. |
| 0xC00000EB<br>STATUS\_UNEXPECTED\_MM\_MAP\_ERROR | If an MM error that is not defined in the standard FsRtl filter is returned, it is converted to one of the following errors, which are guaranteed to be in the filter. In this case, information is lost; however, the filter correctly handles the exception. |
| 0xC00000EC<br>STATUS\_UNEXPECTED\_MM\_EXTEND\_ERR | If an MM error that is not defined in the standard FsRtl filter is returned, it is converted to one of the following errors, which are guaranteed to be in the filter. In this case, information is lost; however, the filter correctly handles the exception. |
| 0xC00000ED<br>STATUS\_NOT\_LOGON\_PROCESS | The requested action is restricted for use by logon processes only. The calling process has not registered as a logon process. |
| 0xC00000EE<br>STATUS\_LOGON\_SESSION\_EXISTS | An attempt has been made to start a new session manager or LSA logon session by using an ID that is already in use. |
| 0xC00000EF<br>STATUS\_INVALID\_PARAMETER\_1 | An invalid parameter was passed to a service or function as the first argument. |
| 0xC00000F0<br>STATUS\_INVALID\_PARAMETER\_2 | An invalid parameter was passed to a service or function as the second argument. |
| 0xC00000F1<br>STATUS\_INVALID\_PARAMETER\_3 | An invalid parameter was passed to a service or function as the third argument. |
| 0xC00000F2<br>STATUS\_INVALID\_PARAMETER\_4 | An invalid parameter was passed to a service or function as the fourth argument. |
| 0xC00000F3<br>STATUS\_INVALID\_PARAMETER\_5 | An invalid parameter was passed to a service or function as the fifth argument. |
| 0xC00000F4<br>STATUS\_INVALID\_PARAMETER\_6 | An invalid parameter was passed to a service or function as the sixth argument. |
| 0xC00000F5<br>STATUS\_INVALID\_PARAMETER\_7 | An invalid parameter was passed to a service or function as the seventh argument. |
| 0xC00000F6<br>STATUS\_INVALID\_PARAMETER\_8 | An invalid parameter was passed to a service or function as the eighth argument. |
| 0xC00000F7<br>STATUS\_INVALID\_PARAMETER\_9 | An invalid parameter was passed to a service or function as the ninth argument. |
| 0xC00000F8<br>STATUS\_INVALID\_PARAMETER\_10 | An invalid parameter was passed to a service or function as the tenth argument. |
| 0xC00000F9<br>STATUS\_INVALID\_PARAMETER\_11 | An invalid parameter was passed to a service or function as the eleventh argument. |
| 0xC00000FA<br>STATUS\_INVALID\_PARAMETER\_12 | An invalid parameter was passed to a service or function as the twelfth argument. |
| 0xC00000FB<br>STATUS\_REDIRECTOR\_NOT\_STARTED | An attempt was made to access a network file, but the network software was not yet started. |
| 0xC00000FC<br>STATUS\_REDIRECTOR\_STARTED | An attempt was made to start the redirector, but the redirector has already been started. |
| 0xC00000FD<br>STATUS\_STACK\_OVERFLOW | A new guard page for the stack cannot be created. |
| 0xC00000FE<br>STATUS\_NO\_SUCH\_PACKAGE | A specified authentication package is unknown. |
| 0xC00000FF<br>STATUS\_BAD\_FUNCTION\_TABLE | A malformed function table was encountered during an unwind operation. |
| 0xC0000100<br>STATUS\_VARIABLE\_NOT\_FOUND | Indicates the specified environment variable name was not found in the specified environment block. |
| 0xC0000101<br>STATUS\_DIRECTORY\_NOT\_EMPTY | Indicates that the directory trying to be deleted is not empty. |
| 0xC0000102<br>STATUS\_FILE\_CORRUPT\_ERROR | {Corrupt File} The file or directory %hs is corrupt and unreadable. Run the Chkdsk utility. |
| 0xC0000103<br>STATUS\_NOT\_A\_DIRECTORY | A requested opened file is not a directory. |
| 0xC0000104<br>STATUS\_BAD\_LOGON\_SESSION\_STATE | The logon session is not in a state that is consistent with the requested operation. |
| 0xC0000105<br>STATUS\_LOGON\_SESSION\_COLLISION | An internal LSA error has occurred. An authentication package has requested the creation of a logon session but the ID of an already existing logon session has been specified. |
| 0xC0000106<br>STATUS\_NAME\_TOO\_LONG | A specified name string is too long for its intended use. |
| 0xC0000107<br>STATUS\_FILES\_OPEN | The user attempted to force close the files on a redirected drive, but there were opened files on the drive, and the user did not specify a sufficient level of force. |
| 0xC0000108<br>STATUS\_CONNECTION\_IN\_USE | The user attempted to force close the files on a redirected drive, but there were opened directories on the drive, and the user did not specify a sufficient level of force. |
| 0xC0000109<br>STATUS\_MESSAGE\_NOT\_FOUND | RtlFindMessage could not locate the requested message ID in the message table resource. |
| 0xC000010A<br>STATUS\_PROCESS\_IS\_TERMINATING | An attempt was made to duplicate an object handle into or out of an exiting process. |
| 0xC000010B<br>STATUS\_INVALID\_LOGON\_TYPE | Indicates an invalid value has been provided for the LogonType requested. |
| 0xC000010C<br>STATUS\_NO\_GUID\_TRANSLATION | Indicates that an attempt was made to assign protection to a file system file or directory and one of the SIDs in the security descriptor could not be translated into a GUID that could be stored by the file system. This causes the protection attempt to fail, which might cause a file creation attempt to fail. |
| 0xC000010D<br>STATUS\_CANNOT\_IMPERSONATE | Indicates that an attempt has been made to impersonate via a named pipe that has not yet been read from. |
| 0xC000010E<br>STATUS\_IMAGE\_ALREADY\_LOADED | Indicates that the specified image is already loaded. |
| 0xC0000117<br>STATUS\_NO\_LDT | Indicates that an attempt was made to change the size of the LDT for a process that has no LDT. |
| 0xC0000118<br>STATUS\_INVALID\_LDT\_SIZE | Indicates that an attempt was made to grow an LDT by setting its size, or that the size was not an even number of selectors. |
| 0xC0000119<br>STATUS\_INVALID\_LDT\_OFFSET | Indicates that the starting value for the LDT information was not an integral multiple of the selector size. |
| 0xC000011A<br>STATUS\_INVALID\_LDT\_DESCRIPTOR | Indicates that the user supplied an invalid descriptor when trying to set up LDT descriptors. |
| 0xC000011B<br>STATUS\_INVALID\_IMAGE\_NE\_FORMAT | The specified image file did not have the correct format. It appears to be NE format. |
| 0xC000011C<br>STATUS\_RXACT\_INVALID\_STATE | Indicates that the transaction state of a registry subtree is incompatible with the requested operation. For example, a request has been made to start a new transaction with one already in progress, or a request has been made to apply a transaction when one is not currently in progress. |
| 0xC000011D<br>STATUS\_RXACT\_COMMIT\_FAILURE | Indicates an error has occurred during a registry transaction commit. The database has been left in an unknown, but probably inconsistent, state. The state of the registry transaction is left as COMMITTING. |
| 0xC000011E<br>STATUS\_MAPPED\_FILE\_SIZE\_ZERO | An attempt was made to map a file of size zero with the maximum size specified as zero. |
| 0xC000011F<br>STATUS\_TOO\_MANY\_OPENED\_FILES | Too many files are opened on a remote server. This error should only be returned by the Windows redirector on a remote drive. |
| 0xC0000120<br>STATUS\_CANCELLED | The I/O request was canceled. |
| 0xC0000121<br>STATUS\_CANNOT\_DELETE | An attempt has been made to remove a file or directory that cannot be deleted. |
| 0xC0000122<br>STATUS\_INVALID\_COMPUTER\_NAME | Indicates a name that was specified as a remote computer name is syntactically invalid. |
| 0xC0000123<br>STATUS\_FILE\_DELETED | An I/O request other than close was performed on a file after it was deleted, which can only happen to a request that did not complete before the last handle was closed via NtClose. |
| 0xC0000124<br>STATUS\_SPECIAL\_ACCOUNT | Indicates an operation that is incompatible with built-in accounts has been attempted on a built-in (special) SAM account. For example, built-in accounts cannot be deleted. |
| 0xC0000125<br>STATUS\_SPECIAL\_GROUP | The operation requested cannot be performed on the specified group because it is a built-in special group. |
| 0xC0000126<br>STATUS\_SPECIAL\_USER | The operation requested cannot be performed on the specified user because it is a built-in special user. |
| 0xC0000127<br>STATUS\_MEMBERS\_PRIMARY\_GROUP | Indicates a member cannot be removed from a group because the group is currently the member's primary group. |
| 0xC0000128<br>STATUS\_FILE\_CLOSED | An I/O request other than close and several other special case operations was attempted using a file object that had already been closed. |
| 0xC0000129<br>STATUS\_TOO\_MANY\_THREADS | Indicates a process has too many threads to perform the requested action. For example, assignment of a primary token can be performed only when a process has zero or one threads. |
| 0xC000012A<br>STATUS\_THREAD\_NOT\_IN\_PROCESS | An attempt was made to operate on a thread within a specific process, but the specified thread is not in the specified process. |
| 0xC000012B<br>STATUS\_TOKEN\_ALREADY\_IN\_USE | An attempt was made to establish a token for use as a primary token but the token is already in use. A token can only be the primary token of one process at a time. |
| 0xC000012C<br>STATUS\_PAGEFILE\_QUOTA\_EXCEEDED | The page file quota was exceeded. |
| 0xC000012D<br>STATUS\_COMMITMENT\_LIMIT | {Out of Virtual Memory} Your system is low on virtual memory. To ensure that Windows runs correctly, increase the size of your virtual memory paging file. For more information, see Help. |
| 0xC000012E<br>STATUS\_INVALID\_IMAGE\_LE\_FORMAT | The specified image file did not have the correct format: it appears to be LE format. |
| 0xC000012F<br>STATUS\_INVALID\_IMAGE\_NOT\_MZ | The specified image file did not have the correct format: it did not have an initial MZ. |
| 0xC0000130<br>STATUS\_INVALID\_IMAGE\_PROTECT | The specified image file did not have the correct format: it did not have a proper e\_lfarlc in the MZ header. |
| 0xC0000131<br>STATUS\_INVALID\_IMAGE\_WIN\_16 | The specified image file did not have the correct format: it appears to be a 16-bit Windows image. |
| 0xC0000132<br>STATUS\_LOGON\_SERVER\_CONFLICT | The Netlogon service cannot start because another Netlogon service running in the domain conflicts with the specified role. |
| 0xC0000133<br>STATUS\_TIME\_DIFFERENCE\_AT\_DC | The time at the primary domain controller is different from the time at the backup domain controller or member server by too large an amount. |
| 0xC0000134<br>STATUS\_SYNCHRONIZATION\_REQUIRED | On applicable Windows Server releases, the SAM database is significantly out of synchronization with the copy on the domain controller. A complete synchronization is required. |
| 0xC0000135<br>STATUS\_DLL\_NOT\_FOUND | {Unable To Locate Component} This application has failed to start because %hs was not found. Reinstalling the application might fix this problem. |
| 0xC0000136<br>STATUS\_OPEN\_FAILED | The NtCreateFile API failed. This error should never be returned to an application; it is a place holder for the Windows LAN Manager Redirector to use in its internal error-mapping routines. |
| 0xC0000137<br>STATUS\_IO\_PRIVILEGE\_FAILED | {Privilege Failed} The I/O permissions for the process could not be changed. |
| 0xC0000138<br>STATUS\_ORDINAL\_NOT\_FOUND | {Ordinal Not Found} The ordinal %ld could not be located in the dynamic link library %hs. |
| 0xC0000139<br>STATUS\_ENTRYPOINT\_NOT\_FOUND | {Entry Point Not Found} The procedure entry point %hs could not be located in the dynamic link library %hs. |
| 0xC000013A<br>STATUS\_CONTROL\_C\_EXIT | {Application Exit by CTRL+C} The application terminated as a result of a CTRL+C. |
| 0xC000013B<br>STATUS\_LOCAL\_DISCONNECT | {Virtual Circuit Closed} The network transport on your computer has closed a network connection. There might or might not be I/O requests outstanding. |
| 0xC000013C<br>STATUS\_REMOTE\_DISCONNECT | {Virtual Circuit Closed} The network transport on a remote computer has closed a network connection. There might or might not be I/O requests outstanding. |
| 0xC000013D<br>STATUS\_REMOTE\_RESOURCES | {Insufficient Resources on Remote Computer} The remote computer has insufficient resources to complete the network request. For example, the remote computer might not have enough available memory to carry out the request at this time. |
| 0xC000013E<br>STATUS\_LINK\_FAILED | {Virtual Circuit Closed} An existing connection (virtual circuit) has been broken at the remote computer. There is probably something wrong with the network software protocol or the network hardware on the remote computer. |
| 0xC000013F<br>STATUS\_LINK\_TIMEOUT | {Virtual Circuit Closed} The network transport on your computer has closed a network connection because it had to wait too long for a response from the remote computer. |
| 0xC0000140<br>STATUS\_INVALID\_CONNECTION | The connection handle that was given to the transport was invalid. |
| 0xC0000141<br>STATUS\_INVALID\_ADDRESS | The address handle that was given to the transport was invalid. |
| 0xC0000142<br>STATUS\_DLL\_INIT\_FAILED | {DLL Initialization Failed} Initialization of the dynamic link library %hs failed. The process is terminating abnormally. |
| 0xC0000143<br>STATUS\_MISSING\_SYSTEMFILE | {Missing System File} The required system file %hs is bad or missing. |
| 0xC0000144<br>STATUS\_UNHANDLED\_EXCEPTION | {Application Error} The exception %s (0x%08lx) occurred in the application at location 0x%08lx. |
| 0xC0000145<br>STATUS\_APP\_INIT\_FAILURE | {Application Error} The application failed to initialize properly (0x%lx). Click OK to terminate the application. |
| 0xC0000146<br>STATUS\_PAGEFILE\_CREATE\_FAILED | {Unable to Create Paging File} The creation of the paging file %hs failed (%lx). The requested size was %ld. |
| 0xC0000147<br>STATUS\_NO\_PAGEFILE | {No Paging File Specified} No paging file was specified in the system configuration. |
| 0xC0000148<br>STATUS\_INVALID\_LEVEL | {Incorrect System Call Level} An invalid level was passed into the specified system call. |
| 0xC0000149<br>STATUS\_WRONG\_PASSWORD\_CORE | {Incorrect Password to LAN Manager Server} You specified an incorrect password to a LAN Manager 2.x or MS-NET server. |
| 0xC000014A<br>STATUS\_ILLEGAL\_FLOAT\_CONTEXT | {EXCEPTION} A real-mode application issued a floating-point instruction and floating-point hardware is not present. |
| 0xC000014B<br>STATUS\_PIPE\_BROKEN | The pipe operation has failed because the other end of the pipe has been closed. |
| 0xC000014C<br>STATUS\_REGISTRY\_CORRUPT | {The Registry Is Corrupt} The structure of one of the files that contains registry data is corrupt; the image of the file in memory is corrupt; or the file could not be recovered because the alternate copy or log was absent or corrupt. |
| 0xC000014D<br>STATUS\_REGISTRY\_IO\_FAILED | An I/O operation initiated by the Registry failed and cannot be recovered. The registry could not read in, write out, or flush one of the files that contain the system's image of the registry. |
| 0xC000014E<br>STATUS\_NO\_EVENT\_PAIR | An event pair synchronization operation was performed using the thread-specific client/server event pair object, but no event pair object was associated with the thread. |
| 0xC000014F<br>STATUS\_UNRECOGNIZED\_VOLUME | The volume does not contain a recognized file system. Be sure that all required file system drivers are loaded and that the volume is not corrupt. |
| 0xC0000150<br>STATUS\_SERIAL\_NO\_DEVICE\_INITED | No serial device was successfully initialized. The serial driver will unload. |
| 0xC0000151<br>STATUS\_NO\_SUCH\_ALIAS | The specified local group does not exist. |
| 0xC0000152<br>STATUS\_MEMBER\_NOT\_IN\_ALIAS | The specified account name is not a member of the group. |
| 0xC0000153<br>STATUS\_MEMBER\_IN\_ALIAS | The specified account name is already a member of the group. |
| 0xC0000154<br>STATUS\_ALIAS\_EXISTS | The specified local group already exists. |
| 0xC0000155<br>STATUS\_LOGON\_NOT\_GRANTED | A requested type of logon (for example, interactive, network, and service) is not granted by the local security policy of the target system. Ask the system administrator to grant the necessary form of logon. |
| 0xC0000156<br>STATUS\_TOO\_MANY\_SECRETS | The maximum number of secrets that can be stored in a single system was exceeded. The length and number of secrets is limited to satisfy U.S. State Department export restrictions. |
| 0xC0000157<br>STATUS\_SECRET\_TOO\_LONG | The length of a secret exceeds the maximum allowable length. The length and number of secrets is limited to satisfy U.S. State Department export restrictions. |
| 0xC0000158<br>STATUS\_INTERNAL\_DB\_ERROR | The local security authority (LSA) database contains an internal inconsistency. |
| 0xC0000159<br>STATUS\_FULLSCREEN\_MODE | The requested operation cannot be performed in full-screen mode. |
| 0xC000015A<br>STATUS\_TOO\_MANY\_CONTEXT\_IDS | During a logon attempt, the user's security context accumulated too many security IDs. This is a very unusual situation. Remove the user from some global or local groups to reduce the number of security IDs to incorporate into the security context. |
| 0xC000015B<br>STATUS\_LOGON\_TYPE\_NOT\_GRANTED | A user has requested a type of logon (for example, interactive or network) that has not been granted. An administrator has control over who can logon interactively and through the network. |
| 0xC000015C<br>STATUS\_NOT\_REGISTRY\_FILE | The system has attempted to load or restore a file into the registry, and the specified file is not in the format of a registry file. |
| 0xC000015D<br>STATUS\_NT\_CROSS\_ENCRYPTION\_REQUIRED | An attempt was made to change a user password in the security account manager without providing the necessary Windows cross-encrypted password. |
| 0xC000015E<br>STATUS\_DOMAIN\_CTRLR\_CONFIG\_ERROR | A domain server has an incorrect configuration. |
| 0xC000015F<br>STATUS\_FT\_MISSING\_MEMBER | An attempt was made to explicitly access the secondary copy of information via a device control to the fault tolerance driver and the secondary copy is not present in the system. |
| 0xC0000160<br>STATUS\_ILL\_FORMED\_SERVICE\_ENTRY | A configuration registry node that represents a driver service entry was ill-formed and did not contain the required value entries. |
| 0xC0000161<br>STATUS\_ILLEGAL\_CHARACTER | An illegal character was encountered. For a multibyte character set, this includes a lead byte without a succeeding trail byte. For the Unicode character set this includes the characters 0xFFFF and 0xFFFE. |
| 0xC0000162<br>STATUS\_UNMAPPABLE\_CHARACTER | No mapping for the Unicode character exists in the target multibyte code page. |
| 0xC0000163<br>STATUS\_UNDEFINED\_CHARACTER | The Unicode character is not defined in the Unicode character set that is installed on the system. |
| 0xC0000164<br>STATUS\_FLOPPY\_VOLUME | The paging file cannot be created on a floppy disk. |
| 0xC0000165<br>STATUS\_FLOPPY\_ID\_MARK\_NOT\_FOUND | {Floppy Disk Error} While accessing a floppy disk, an ID address mark was not found. |
| 0xC0000166<br>STATUS\_FLOPPY\_WRONG\_CYLINDER | {Floppy Disk Error} While accessing a floppy disk, the track address from the sector ID field was found to be different from the track address that is maintained by the controller. |
| 0xC0000167<br>STATUS\_FLOPPY\_UNKNOWN\_ERROR | {Floppy Disk Error} The floppy disk controller reported an error that is not recognized by the floppy disk driver. |
| 0xC0000168<br>STATUS\_FLOPPY\_BAD\_REGISTERS | {Floppy Disk Error} While accessing a floppy-disk, the controller returned inconsistent results via its registers. |
| 0xC0000169<br>STATUS\_DISK\_RECALIBRATE\_FAILED | {Hard Disk Error} While accessing the hard disk, a recalibrate operation failed, even after retries. |
| 0xC000016A<br>STATUS\_DISK\_OPERATION\_FAILED | {Hard Disk Error} While accessing the hard disk, a disk operation failed even after retries. |
| 0xC000016B<br>STATUS\_DISK\_RESET\_FAILED | {Hard Disk Error} While accessing the hard disk, a disk controller reset was needed, but even that failed. |
| 0xC000016C<br>STATUS\_SHARED\_IRQ\_BUSY | An attempt was made to open a device that was sharing an interrupt request (IRQ) with other devices. At least one other device that uses that IRQ was already opened. Two concurrent opens of devices that share an IRQ and only work via interrupts is not supported for the particular bus type that the devices use. |
| 0xC000016D<br>STATUS\_FT\_ORPHANING | {FT Orphaning} A disk that is part of a fault-tolerant volume can no longer be accessed. |
| 0xC000016E<br>STATUS\_BIOS\_FAILED\_TO\_CONNECT\_INTERRUPT | The basic input/output system (BIOS) failed to connect a system interrupt to the device or bus for which the device is connected. |
| 0xC0000172<br>STATUS\_PARTITION\_FAILURE | The tape could not be partitioned. |
| 0xC0000173<br>STATUS\_INVALID\_BLOCK\_LENGTH | When accessing a new tape of a multi-volume partition, the current blocksize is incorrect. |
| 0xC0000174<br>STATUS\_DEVICE\_NOT\_PARTITIONED | The tape partition information could not be found when loading a tape. |
| 0xC0000175<br>STATUS\_UNABLE\_TO\_LOCK\_MEDIA | An attempt to lock the eject media mechanism failed. |
| 0xC0000176<br>STATUS\_UNABLE\_TO\_UNLOAD\_MEDIA | An attempt to unload media failed. |
| 0xC0000177<br>STATUS\_EOM\_OVERFLOW | The physical end of tape was detected. |
| 0xC0000178<br>STATUS\_NO\_MEDIA | {No Media} There is no media in the drive. Insert media into drive %hs. |
| 0xC000017A<br>STATUS\_NO\_SUCH\_MEMBER | A member could not be added to or removed from the local group because the member does not exist. |
| 0xC000017B<br>STATUS\_INVALID\_MEMBER | A new member could not be added to a local group because the member has the wrong account type. |
| 0xC000017C<br>STATUS\_KEY\_DELETED | An illegal operation was attempted on a registry key that has been marked for deletion. |
| 0xC000017D<br>STATUS\_NO\_LOG\_SPACE | The system could not allocate the required space in a registry log. |
| 0xC000017E<br>STATUS\_TOO\_MANY\_SIDS | Too many SIDs have been specified. |
| 0xC000017F<br>STATUS\_LM\_CROSS\_ENCRYPTION\_REQUIRED | An attempt was made to change a user password in the security account manager without providing the necessary LM cross-encrypted password. |
| 0xC0000180<br>STATUS\_KEY\_HAS\_CHILDREN | An attempt was made to create a symbolic link in a registry key that already has subkeys or values. |
| 0xC0000181<br>STATUS\_CHILD\_MUST\_BE\_VOLATILE | An attempt was made to create a stable subkey under a volatile parent key. |
| 0xC0000182<br>STATUS\_DEVICE\_CONFIGURATION\_ERROR | The I/O device is configured incorrectly or the configuration parameters to the driver are incorrect. |
| 0xC0000183<br>STATUS\_DRIVER\_INTERNAL\_ERROR | An error was detected between two drivers or within an I/O driver. |
| 0xC0000184<br>STATUS\_INVALID\_DEVICE\_STATE | The device is not in a valid state to perform this request. |
| 0xC0000185<br>STATUS\_IO\_DEVICE\_ERROR | The I/O device reported an I/O error. |
| 0xC0000186<br>STATUS\_DEVICE\_PROTOCOL\_ERROR | A protocol error was detected between the driver and the device. |
| 0xC0000187<br>STATUS\_BACKUP\_CONTROLLER | This operation is only allowed for the primary domain controller of the domain. |
| 0xC0000188<br>STATUS\_LOG\_FILE\_FULL | The log file space is insufficient to support this operation. |
| 0xC0000189<br>STATUS\_TOO\_LATE | A write operation was attempted to a volume after it was dismounted. |
| 0xC000018A<br>STATUS\_NO\_TRUST\_LSA\_SECRET | The workstation does not have a trust secret for the primary domain in the local LSA database. |
| 0xC000018B<br>STATUS\_NO\_TRUST\_SAM\_ACCOUNT | On applicable Windows Server releases, the SAM database does not have a computer account for this workstation trust relationship. |
| 0xC000018C<br>STATUS\_TRUSTED\_DOMAIN\_FAILURE | The logon request failed because the trust relationship between the primary domain and the trusted domain failed. |
| 0xC000018D<br>STATUS\_TRUSTED\_RELATIONSHIP\_FAILURE | The logon request failed because the trust relationship between this workstation and the primary domain failed. |
| 0xC000018E<br>STATUS\_EVENTLOG\_FILE\_CORRUPT | The Eventlog log file is corrupt. |
| 0xC000018F<br>STATUS\_EVENTLOG\_CANT\_START | No Eventlog log file could be opened. The Eventlog service did not start. |
| 0xC0000190<br>STATUS\_TRUST\_FAILURE | The network logon failed. This might be because the validation authority cannot be reached. |
| 0xC0000191<br>STATUS\_MUTANT\_LIMIT\_EXCEEDED | An attempt was made to acquire a mutant such that its maximum count would have been exceeded. |
| 0xC0000192<br>STATUS\_NETLOGON\_NOT\_STARTED | An attempt was made to logon, but the NetLogon service was not started. |
| 0xC0000193<br>STATUS\_ACCOUNT\_EXPIRED | The user account has expired. |
| 0xC0000194<br>STATUS\_POSSIBLE\_DEADLOCK | {EXCEPTION} Possible deadlock condition. |
| 0xC0000195<br>STATUS\_NETWORK\_CREDENTIAL\_CONFLICT | Multiple connections to a server or shared resource by the same user, using more than one user name, are not allowed. Disconnect all previous connections to the server or shared resource and try again. |
| 0xC0000196<br>STATUS\_REMOTE\_SESSION\_LIMIT | An attempt was made to establish a session to a network server, but there are already too many sessions established to that server. |
| 0xC0000197<br>STATUS\_EVENTLOG\_FILE\_CHANGED | The log file has changed between reads. |
| 0xC0000198<br>STATUS\_NOLOGON\_INTERDOMAIN\_TRUST\_ACCOUNT | The account used is an interdomain trust account. Use your global user account or local user account to access this server. |
| 0xC0000199<br>STATUS\_NOLOGON\_WORKSTATION\_TRUST\_ACCOUNT | The account used is a computer account. Use your global user account or local user account to access this server. |
| 0xC000019A<br>STATUS\_NOLOGON\_SERVER\_TRUST\_ACCOUNT | The account used is a server trust account. Use your global user account or local user account to access this server. |
| 0xC000019B<br>STATUS\_DOMAIN\_TRUST\_INCONSISTENT | The name or SID of the specified domain is inconsistent with the trust information for that domain. |
| 0xC000019C<br>STATUS\_FS\_DRIVER\_REQUIRED | A volume has been accessed for which a file system driver is required that has not yet been loaded. |
| 0xC000019D<br>STATUS\_IMAGE\_ALREADY\_LOADED\_AS\_DLL | Indicates that the specified image is already loaded as a DLL. |
| 0xC000019E<br>STATUS\_INCOMPATIBLE\_WITH\_GLOBAL\_SHORT\_NAME\_REGISTRY\_SETTING | Short name settings cannot be changed on this volume due to the global registry setting. |
| 0xC000019F<br>STATUS\_SHORT\_NAMES\_NOT\_ENABLED\_ON\_VOLUME | Short names are not enabled on this volume. |
| 0xC00001A0<br>STATUS\_SECURITY\_STREAM\_IS\_INCONSISTENT | The security stream for the given volume is in an inconsistent state. Please run CHKDSK on the volume. |
| 0xC00001A1<br>STATUS\_INVALID\_LOCK\_RANGE | A requested file lock operation cannot be processed due to an invalid byte range. |
| 0xC00001A2<br>STATUS\_INVALID\_ACE\_CONDITION | The specified access control entry (ACE) contains an invalid condition. |
| 0xC00001A3<br>STATUS\_IMAGE\_SUBSYSTEM\_NOT\_PRESENT | The subsystem needed to support the image type is not present. |
| 0xC00001A4<br>STATUS\_NOTIFICATION\_GUID\_ALREADY\_DEFINED | The specified file already has a notification GUID associated with it. |
| 0xC0000201<br>STATUS\_NETWORK\_OPEN\_RESTRICTION | A remote open failed because the network open restrictions were not satisfied. |
| 0xC0000202<br>STATUS\_NO\_USER\_SESSION\_KEY | There is no user session key for the specified logon session. |
| 0xC0000203<br>STATUS\_USER\_SESSION\_DELETED | The remote user session has been deleted. |
| 0xC0000204<br>STATUS\_RESOURCE\_LANG\_NOT\_FOUND | Indicates the specified resource language ID cannot be found in the image file. |
| 0xC0000205<br>STATUS\_INSUFF\_SERVER\_RESOURCES | Insufficient server resources exist to complete the request. |
| 0xC0000206<br>STATUS\_INVALID\_BUFFER\_SIZE | The size of the buffer is invalid for the specified operation. |
| 0xC0000207<br>STATUS\_INVALID\_ADDRESS\_COMPONENT | The transport rejected the specified network address as invalid. |
| 0xC0000208<br>STATUS\_INVALID\_ADDRESS\_WILDCARD | The transport rejected the specified network address due to invalid use of a wildcard. |
| 0xC0000209<br>STATUS\_TOO\_MANY\_ADDRESSES | The transport address could not be opened because all the available addresses are in use. |
| 0xC000020A<br>STATUS\_ADDRESS\_ALREADY\_EXISTS | The transport address could not be opened because it already exists. |
| 0xC000020B<br>STATUS\_ADDRESS\_CLOSED | The transport address is now closed. |
| 0xC000020C<br>STATUS\_CONNECTION\_DISCONNECTED | The transport connection is now disconnected. |
| 0xC000020D<br>STATUS\_CONNECTION\_RESET | The transport connection has been reset. |
| 0xC000020E<br>STATUS\_TOO\_MANY\_NODES | The transport cannot dynamically acquire any more nodes. |
| 0xC000020F<br>STATUS\_TRANSACTION\_ABORTED | The transport aborted a pending transaction. |
| 0xC0000210<br>STATUS\_TRANSACTION\_TIMED\_OUT | The transport timed out a request that is waiting for a response. |
| 0xC0000211<br>STATUS\_TRANSACTION\_NO\_RELEASE | The transport did not receive a release for a pending response. |
| 0xC0000212<br>STATUS\_TRANSACTION\_NO\_MATCH | The transport did not find a transaction that matches the specific token. |
| 0xC0000213<br>STATUS\_TRANSACTION\_RESPONDED | The transport had previously responded to a transaction request. |
| 0xC0000214<br>STATUS\_TRANSACTION\_INVALID\_ID | The transport does not recognize the specified transaction request ID. |
| 0xC0000215<br>STATUS\_TRANSACTION\_INVALID\_TYPE | The transport does not recognize the specified transaction request type. |
| 0xC0000216<br>STATUS\_NOT\_SERVER\_SESSION | The transport can only process the specified request on the server side of a session. |
| 0xC0000217<br>STATUS\_NOT\_CLIENT\_SESSION | The transport can only process the specified request on the client side of a session. |
| 0xC0000218<br>STATUS\_CANNOT\_LOAD\_REGISTRY\_FILE | {Registry File Failure} The registry cannot load the hive (file): %hs or its log or alternate. It is corrupt, absent, or not writable. |
| 0xC0000219<br>STATUS\_DEBUG\_ATTACH\_FAILED | {Unexpected Failure in DebugActiveProcess} An unexpected failure occurred while processing a DebugActiveProcess API request. Choosing OK will terminate the process, and choosing Cancel will ignore the error. |
| 0xC000021A<br>STATUS\_SYSTEM\_PROCESS\_TERMINATED | {Fatal System Error} The %hs system process terminated unexpectedly with a status of 0x%08x (0x%08x 0x%08x). The system has been shut down. |
| 0xC000021B<br>STATUS\_DATA\_NOT\_ACCEPTED | {Data Not Accepted} The TDI client could not handle the data received during an indication. |
| 0xC000021C<br>STATUS\_NO\_BROWSER\_SERVERS\_FOUND | {Unable to Retrieve Browser Server List} The list of servers for this workgroup is not currently available. |
| 0xC000021D<br>STATUS\_VDM\_HARD\_ERROR | NTVDM encountered a hard error. |
| 0xC000021E<br>STATUS\_DRIVER\_CANCEL\_TIMEOUT | {Cancel Timeout} The driver %hs failed to complete a canceled I/O request in the allotted time. |
| 0xC000021F<br>STATUS\_REPLY\_MESSAGE\_MISMATCH | {Reply Message Mismatch} An attempt was made to reply to an LPC message, but the thread specified by the client ID in the message was not waiting on that message. |
| 0xC0000220<br>STATUS\_MAPPED\_ALIGNMENT | {Mapped View Alignment Incorrect} An attempt was made to map a view of a file, but either the specified base address or the offset into the file were not aligned on the proper allocation granularity. |
| 0xC0000221<br>STATUS\_IMAGE\_CHECKSUM\_MISMATCH | {Bad Image Checksum} The image %hs is possibly corrupt. The header checksum does not match the computed checksum. |
| 0xC0000222<br>STATUS\_LOST\_WRITEBEHIND\_DATA | {Delayed Write Failed} Windows was unable to save all the data for the file %hs. The data has been lost. This error might be caused by a failure of your computer hardware or network connection. Try to save this file elsewhere. |
| 0xC0000223<br>STATUS\_CLIENT\_SERVER\_PARAMETERS\_INVALID | The parameters passed to the server in the client/server shared memory window were invalid. Too much data might have been put in the shared memory window. |
| 0xC0000224<br>STATUS\_PASSWORD\_MUST\_CHANGE | The user password must be changed before logging on the first time. |
| 0xC0000225<br>STATUS\_NOT\_FOUND | The object was not found. |
| 0xC0000226<br>STATUS\_NOT\_TINY\_STREAM | The stream is not a tiny stream. |
| 0xC0000227<br>STATUS\_RECOVERY\_FAILURE | A transaction recovery failed. |
| 0xC0000228<br>STATUS\_STACK\_OVERFLOW\_READ | The request must be handled by the stack overflow code. |
| 0xC0000229<br>STATUS\_FAIL\_CHECK | A consistency check failed. |
| 0xC000022A<br>STATUS\_DUPLICATE\_OBJECTID | The attempt to insert the ID in the index failed because the ID is already in the index. |
| 0xC000022B<br>STATUS\_OBJECTID\_EXISTS | The attempt to set the object ID failed because the object already has an ID. |
| 0xC000022C<br>STATUS\_CONVERT\_TO\_LARGE | Internal OFS status codes indicating how an allocation operation is handled. Either it is retried after the containing oNode is moved or the extent stream is converted to a large stream. |
| 0xC000022D<br>STATUS\_RETRY | The request needs to be retried. |
| 0xC000022E<br>STATUS\_FOUND\_OUT\_OF\_SCOPE | The attempt to find the object found an object on the volume that matches by ID; however, it is out of the scope of the handle that is used for the operation. |
| 0xC000022F<br>STATUS\_ALLOCATE\_BUCKET | The bucket array must be grown. Retry the transaction after doing so. |
| 0xC0000230<br>STATUS\_PROPSET\_NOT\_FOUND | The specified property set does not exist on the object. |
| 0xC0000231<br>STATUS\_MARSHALL\_OVERFLOW | The user/kernel marshaling buffer has overflowed. |
| 0xC0000232<br>STATUS\_INVALID\_VARIANT | The supplied variant structure contains invalid data. |
| 0xC0000233<br>STATUS\_DOMAIN\_CONTROLLER\_NOT\_FOUND | A domain controller for this domain was not found. |
| 0xC0000234<br>STATUS\_ACCOUNT\_LOCKED\_OUT | The user account has been automatically locked because too many invalid logon attempts or password change attempts have been requested. |
| 0xC0000235<br>STATUS\_HANDLE\_NOT\_CLOSABLE | NtClose was called on a handle that was protected from close via NtSetInformationObject. |
| 0xC0000236<br>STATUS\_CONNECTION\_REFUSED | The transport-connection attempt was refused by the remote system. |
| 0xC0000237<br>STATUS\_GRACEFUL\_DISCONNECT | The transport connection was gracefully closed. |
| 0xC0000238<br>STATUS\_ADDRESS\_ALREADY\_ASSOCIATED | The transport endpoint already has an address associated with it. |
| 0xC0000239<br>STATUS\_ADDRESS\_NOT\_ASSOCIATED | An address has not yet been associated with the transport endpoint. |
| 0xC000023A<br>STATUS\_CONNECTION\_INVALID | An operation was attempted on a nonexistent transport connection. |
| 0xC000023B<br>STATUS\_CONNECTION\_ACTIVE | An invalid operation was attempted on an active transport connection. |
| 0xC000023C<br>STATUS\_NETWORK\_UNREACHABLE | The remote network is not reachable by the transport. |
| 0xC000023D<br>STATUS\_HOST\_UNREACHABLE | The remote system is not reachable by the transport. |
| 0xC000023E<br>STATUS\_PROTOCOL\_UNREACHABLE | The remote system does not support the transport protocol. |
| 0xC000023F<br>STATUS\_PORT\_UNREACHABLE | No service is operating at the destination port of the transport on the remote system. |
| 0xC0000240<br>STATUS\_REQUEST\_ABORTED | The request was aborted. |
| 0xC0000241<br>STATUS\_CONNECTION\_ABORTED | The transport connection was aborted by the local system. |
| 0xC0000242<br>STATUS\_BAD\_COMPRESSION\_BUFFER | The specified buffer contains ill-formed data. |
| 0xC0000243<br>STATUS\_USER\_MAPPED\_FILE | The requested operation cannot be performed on a file with a user mapped section open. |
| 0xC0000244<br>STATUS\_AUDIT\_FAILED | {Audit Failed} An attempt to generate a security audit failed. |
| 0xC0000245<br>STATUS\_TIMER\_RESOLUTION\_NOT\_SET | The timer resolution was not previously set by the current process. |
| 0xC0000246<br>STATUS\_CONNECTION\_COUNT\_LIMIT | A connection to the server could not be made because the limit on the number of concurrent connections for this account has been reached. |
| 0xC0000247<br>STATUS\_LOGIN\_TIME\_RESTRICTION | Attempting to log on during an unauthorized time of day for this account. |
| 0xC0000248<br>STATUS\_LOGIN\_WKSTA\_RESTRICTION | The account is not authorized to log on from this station. |
| 0xC0000249<br>STATUS\_IMAGE\_MP\_UP\_MISMATCH | {UP/MP Image Mismatch} The image %hs has been modified for use on a uniprocessor system, but you are running it on a multiprocessor machine. Reinstall the image file. |
| 0xC0000250<br>STATUS\_INSUFFICIENT\_LOGON\_INFO | There is insufficient account information to log you on. |
| 0xC0000251<br>STATUS\_BAD\_DLL\_ENTRYPOINT | {Invalid DLL Entrypoint} The dynamic link library %hs is not written correctly. The stack pointer has been left in an inconsistent state. The entry point should be declared as WINAPI or STDCALL. Select YES to fail the DLL load. Select NO to continue execution. Selecting NO might cause the application to operate incorrectly. |
| 0xC0000252<br>STATUS\_BAD\_SERVICE\_ENTRYPOINT | {Invalid Service Callback Entrypoint} The %hs service is not written correctly. The stack pointer has been left in an inconsistent state. The callback entry point should be declared as WINAPI or STDCALL. Selecting OK will cause the service to continue operation. However, the service process might operate incorrectly. |
| 0xC0000253<br>STATUS\_LPC\_REPLY\_LOST | The server received the messages but did not send a reply. |
| 0xC0000254<br>STATUS\_IP\_ADDRESS\_CONFLICT1 | There is an IP address conflict with another system on the network. |
| 0xC0000255<br>STATUS\_IP\_ADDRESS\_CONFLICT2 | There is an IP address conflict with another system on the network. |
| 0xC0000256<br>STATUS\_REGISTRY\_QUOTA\_LIMIT | {Low On Registry Space} The system has reached the maximum size that is allowed for the system part of the registry. Additional storage requests will be ignored. |
| 0xC0000257<br>STATUS\_PATH\_NOT\_COVERED | The contacted server does not support the indicated part of the DFS namespace. |
| 0xC0000258<br>STATUS\_NO\_CALLBACK\_ACTIVE | A callback return system service cannot be executed when no callback is active. |
| 0xC0000259<br>STATUS\_LICENSE\_QUOTA\_EXCEEDED | The service being accessed is licensed for a particular number of connections. No more connections can be made to the service at this time because the service has already accepted the maximum number of connections. |
| 0xC000025A<br>STATUS\_PWD\_TOO\_SHORT | The password provided is too short to meet the policy of your user account. Choose a longer password. |
| 0xC000025B<br>STATUS\_PWD\_TOO\_RECENT | The policy of your user account does not allow you to change passwords too frequently. This is done to prevent users from changing back to a familiar, but potentially discovered, password. If you feel your password has been compromised, contact your administrator immediately to have a new one assigned. |
| 0xC000025C<br>STATUS\_PWD\_HISTORY\_CONFLICT | You have attempted to change your password to one that you have used in the past. The policy of your user account does not allow this. Select a password that you have not previously used. |
| 0xC000025E<br>STATUS\_PLUGPLAY\_NO\_DEVICE | You have attempted to load a legacy device driver while its device instance had been disabled. |
| 0xC000025F<br>STATUS\_UNSUPPORTED\_COMPRESSION | The specified compression format is unsupported. |
| 0xC0000260<br>STATUS\_INVALID\_HW\_PROFILE | The specified hardware profile configuration is invalid. |
| 0xC0000261<br>STATUS\_INVALID\_PLUGPLAY\_DEVICE\_PATH | The specified Plug and Play registry device path is invalid. |
| 0xC0000262<br>STATUS\_DRIVER\_ORDINAL\_NOT\_FOUND | {Driver Entry Point Not Found} The %hs device driver could not locate the ordinal %ld in driver %hs. |
| 0xC0000263<br>STATUS\_DRIVER\_ENTRYPOINT\_NOT\_FOUND | {Driver Entry Point Not Found} The %hs device driver could not locate the entry point %hs in driver %hs. |
| 0xC0000264<br>STATUS\_RESOURCE\_NOT\_OWNED | {Application Error} The application attempted to release a resource it did not own. Click OK to terminate the application. |
| 0xC0000265<br>STATUS\_TOO\_MANY\_LINKS | An attempt was made to create more links on a file than the file system supports. |
| 0xC0000266<br>STATUS\_QUOTA\_LIST\_INCONSISTENT | The specified quota list is internally inconsistent with its descriptor. |
| 0xC0000267<br>STATUS\_FILE\_IS\_OFFLINE | The specified file has been relocated to offline storage. |
| 0xC0000268<br>STATUS\_EVALUATION\_EXPIRATION | {Windows Evaluation Notification} The evaluation period for this installation of Windows has expired. This system will shutdown in 1 hour. To restore access to this installation of Windows, upgrade this installation by using a licensed distribution of this product. |
| 0xC0000269<br>STATUS\_ILLEGAL\_DLL\_RELOCATION | {Illegal System DLL Relocation} The system DLL %hs was relocated in memory. The application will not run properly. The relocation occurred because the DLL %hs occupied an address range that is reserved for Windows system DLLs. The vendor supplying the DLL should be contacted for a new DLL. |
| 0xC000026A<br>STATUS\_LICENSE\_VIOLATION | {License Violation} The system has detected tampering with your registered product type. This is a violation of your software license. Tampering with the product type is not permitted. |
| 0xC000026B<br>STATUS\_DLL\_INIT\_FAILED\_LOGOFF | {DLL Initialization Failed} The application failed to initialize because the window station is shutting down. |
| 0xC000026C<br>STATUS\_DRIVER\_UNABLE\_TO\_LOAD | {Unable to Load Device Driver} %hs device driver could not be loaded. Error Status was 0x%x. |
| 0xC000026D<br>STATUS\_DFS\_UNAVAILABLE | DFS is unavailable on the contacted server. |
| 0xC000026E<br>STATUS\_VOLUME\_DISMOUNTED | An operation was attempted to a volume after it was dismounted. |
| 0xC000026F<br>STATUS\_WX86\_INTERNAL\_ERROR | An internal error occurred in the Win32 x86 emulation subsystem. |
| 0xC0000270<br>STATUS\_WX86\_FLOAT\_STACK\_CHECK | Win32 x86 emulation subsystem floating-point stack check. |
| 0xC0000271<br>STATUS\_VALIDATE\_CONTINUE | The validation process needs to continue on to the next step. |
| 0xC0000272<br>STATUS\_NO\_MATCH | There was no match for the specified key in the index. |
| 0xC0000273<br>STATUS\_NO\_MORE\_MATCHES | There are no more matches for the current index enumeration. |
| 0xC0000275<br>STATUS\_NOT\_A\_REPARSE\_POINT | The NTFS file or directory is not a reparse point. |
| 0xC0000276<br>STATUS\_IO\_REPARSE\_TAG\_INVALID | The Windows I/O reparse tag passed for the NTFS reparse point is invalid. |
| 0xC0000277<br>STATUS\_IO\_REPARSE\_TAG\_MISMATCH | The Windows I/O reparse tag does not match the one that is in the NTFS reparse point. |
| 0xC0000278<br>STATUS\_IO\_REPARSE\_DATA\_INVALID | The user data passed for the NTFS reparse point is invalid. |
| 0xC0000279<br>STATUS\_IO\_REPARSE\_TAG\_NOT\_HANDLED | The layered file system driver for this I/O tag did not handle it when needed. |
| 0xC0000280<br>STATUS\_REPARSE\_POINT\_NOT\_RESOLVED | The NTFS symbolic link could not be resolved even though the initial file name is valid. |
| 0xC0000281<br>STATUS\_DIRECTORY\_IS\_A\_REPARSE\_POINT | The NTFS directory is a reparse point. |
| 0xC0000282<br>STATUS\_RANGE\_LIST\_CONFLICT | The range could not be added to the range list because of a conflict. |
| 0xC0000283<br>STATUS\_SOURCE\_ELEMENT\_EMPTY | The specified medium changer source element contains no media. |
| 0xC0000284<br>STATUS\_DESTINATION\_ELEMENT\_FULL | The specified medium changer destination element already contains media. |
| 0xC0000285<br>STATUS\_ILLEGAL\_ELEMENT\_ADDRESS | The specified medium changer element does not exist. |
| 0xC0000286<br>STATUS\_MAGAZINE\_NOT\_PRESENT | The specified element is contained in a magazine that is no longer present. |
| 0xC0000287<br>STATUS\_REINITIALIZATION\_NEEDED | The device requires re-initialization due to hardware errors. |
| 0xC000028A<br>STATUS\_ENCRYPTION\_FAILED | The file encryption attempt failed. |
| 0xC000028B<br>STATUS\_DECRYPTION\_FAILED | The file decryption attempt failed. |
| 0xC000028C<br>STATUS\_RANGE\_NOT\_FOUND | The specified range could not be found in the range list. |
| 0xC000028D<br>STATUS\_NO\_RECOVERY\_POLICY | There is no encryption recovery policy configured for this system. |
| 0xC000028E<br>STATUS\_NO\_EFS | The required encryption driver is not loaded for this system. |
| 0xC000028F<br>STATUS\_WRONG\_EFS | The file was encrypted with a different encryption driver than is currently loaded. |
| 0xC0000290<br>STATUS\_NO\_USER\_KEYS | There are no EFS keys defined for the user. |
| 0xC0000291<br>STATUS\_FILE\_NOT\_ENCRYPTED | The specified file is not encrypted. |
| 0xC0000292<br>STATUS\_NOT\_EXPORT\_FORMAT | The specified file is not in the defined EFS export format. |
| 0xC0000293<br>STATUS\_FILE\_ENCRYPTED | The specified file is encrypted and the user does not have the ability to decrypt it. |
| 0xC0000295<br>STATUS\_WMI\_GUID\_NOT\_FOUND | The GUID passed was not recognized as valid by a WMI data provider. |
| 0xC0000296<br>STATUS\_WMI\_INSTANCE\_NOT\_FOUND | The instance name passed was not recognized as valid by a WMI data provider. |
| 0xC0000297<br>STATUS\_WMI\_ITEMID\_NOT\_FOUND | The data item ID passed was not recognized as valid by a WMI data provider. |
| 0xC0000298<br>STATUS\_WMI\_TRY\_AGAIN | The WMI request could not be completed and should be retried. |
| 0xC0000299<br>STATUS\_SHARED\_POLICY | The policy object is shared and can only be modified at the root. |
| 0xC000029A<br>STATUS\_POLICY\_OBJECT\_NOT\_FOUND | The policy object does not exist when it should. |
| 0xC000029B<br>STATUS\_POLICY\_ONLY\_IN\_DS | The requested policy information only lives in the Ds. |
| 0xC000029C<br>STATUS\_VOLUME\_NOT\_UPGRADED | The volume must be upgraded to enable this feature. |
| 0xC000029D<br>STATUS\_REMOTE\_STORAGE\_NOT\_ACTIVE | The remote storage service is not operational at this time. |
| 0xC000029E<br>STATUS\_REMOTE\_STORAGE\_MEDIA\_ERROR | The remote storage service encountered a media error. |
| 0xC000029F<br>STATUS\_NO\_TRACKING\_SERVICE | The tracking (workstation) service is not running. |
| 0xC00002A0<br>STATUS\_SERVER\_SID\_MISMATCH | The server process is running under a SID that is different from the SID that is required by client. |
| 0xC00002A1<br>STATUS\_DS\_NO\_ATTRIBUTE\_OR\_VALUE | The specified directory service attribute or value does not exist. |
| 0xC00002A2<br>STATUS\_DS\_INVALID\_ATTRIBUTE\_SYNTAX | The attribute syntax specified to the directory service is invalid. |
| 0xC00002A3<br>STATUS\_DS\_ATTRIBUTE\_TYPE\_UNDEFINED | The attribute type specified to the directory service is not defined. |
| 0xC00002A4<br>STATUS\_DS\_ATTRIBUTE\_OR\_VALUE\_EXISTS | The specified directory service attribute or value already exists. |
| 0xC00002A5<br>STATUS\_DS\_BUSY | The directory service is busy. |
| 0xC00002A6<br>STATUS\_DS\_UNAVAILABLE | The directory service is unavailable. |
| 0xC00002A7<br>STATUS\_DS\_NO\_RIDS\_ALLOCATED | The directory service was unable to allocate a relative identifier. |
| 0xC00002A8<br>STATUS\_DS\_NO\_MORE\_RIDS | The directory service has exhausted the pool of relative identifiers. |
| 0xC00002A9<br>STATUS\_DS\_INCORRECT\_ROLE\_OWNER | The requested operation could not be performed because the directory service is not the master for that type of operation. |
| 0xC00002AA<br>STATUS\_DS\_RIDMGR\_INIT\_ERROR | The directory service was unable to initialize the subsystem that allocates relative identifiers. |
| 0xC00002AB<br>STATUS\_DS\_OBJ\_CLASS\_VIOLATION | The requested operation did not satisfy one or more constraints that are associated with the class of the object. |
| 0xC00002AC<br>STATUS\_DS\_CANT\_ON\_NON\_LEAF | The directory service can perform the requested operation only on a leaf object. |
| 0xC00002AD<br>STATUS\_DS\_CANT\_ON\_RDN | The directory service cannot perform the requested operation on the Relatively Defined Name (RDN) attribute of an object. |
| 0xC00002AE<br>STATUS\_DS\_CANT\_MOD\_OBJ\_CLASS | The directory service detected an attempt to modify the object class of an object. |
| 0xC00002AF<br>STATUS\_DS\_CROSS\_DOM\_MOVE\_FAILED | An error occurred while performing a cross domain move operation. |
| 0xC00002B0<br>STATUS\_DS\_GC\_NOT\_AVAILABLE | Unable to contact the global catalog server. |
| 0xC00002B1<br>STATUS\_DIRECTORY\_SERVICE\_REQUIRED | The requested operation requires a directory service, and none was available. |
| 0xC00002B2<br>STATUS\_REPARSE\_ATTRIBUTE\_CONFLICT | The reparse attribute cannot be set because it is incompatible with an existing attribute. |
| 0xC00002B3<br>STATUS\_CANT\_ENABLE\_DENY\_ONLY | A group marked "use for deny only" cannot be enabled. |
| 0xC00002B4<br>STATUS\_FLOAT\_MULTIPLE\_FAULTS | {EXCEPTION} Multiple floating-point faults. |
| 0xC00002B5<br>STATUS\_FLOAT\_MULTIPLE\_TRAPS | {EXCEPTION} Multiple floating-point traps. |
| 0xC00002B6<br>STATUS\_DEVICE\_REMOVED | The device has been removed. |
| 0xC00002B7<br>STATUS\_JOURNAL\_DELETE\_IN\_PROGRESS | The volume change journal is being deleted. |
| 0xC00002B8<br>STATUS\_JOURNAL\_NOT\_ACTIVE | The volume change journal is not active. |
| 0xC00002B9<br>STATUS\_NOINTERFACE | The requested interface is not supported. |
| 0xC00002C1<br>STATUS\_DS\_ADMIN\_LIMIT\_EXCEEDED | A directory service resource limit has been exceeded. |
| 0xC00002C2<br>STATUS\_DRIVER\_FAILED\_SLEEP | {System Standby Failed} The driver %hs does not support standby mode. Updating this driver allows the system to go to standby mode. |
| 0xC00002C3<br>STATUS\_MUTUAL\_AUTHENTICATION\_FAILED | Mutual Authentication failed. The server password is out of date at the domain controller. |
| 0xC00002C4<br>STATUS\_CORRUPT\_SYSTEM\_FILE | The system file %1 has become corrupt and has been replaced. |
| 0xC00002C5<br>STATUS\_DATATYPE\_MISALIGNMENT\_ERROR | {EXCEPTION} Alignment Error A data type misalignment error was detected in a load or store instruction. |
| 0xC00002C6<br>STATUS\_WMI\_READ\_ONLY | The WMI data item or data block is read-only. |
| 0xC00002C7<br>STATUS\_WMI\_SET\_FAILURE | The WMI data item or data block could not be changed. |
| 0xC00002C8<br>STATUS\_COMMITMENT\_MINIMUM | {Virtual Memory Minimum Too Low} Your system is low on virtual memory. Windows is increasing the size of your virtual memory paging file. During this process, memory requests for some applications might be denied. For more information, see Help. |
| 0xC00002C9<br>STATUS\_REG\_NAT\_CONSUMPTION | {EXCEPTION} Register NaT consumption faults. A NaT value is consumed on a non-speculative instruction. |
| 0xC00002CA<br>STATUS\_TRANSPORT\_FULL | The transport element of the medium changer contains media, which is causing the operation to fail. |
| 0xC00002CB<br>STATUS\_DS\_SAM\_INIT\_FAILURE | Security Accounts Manager initialization failed because of the following error: %hs Error Status: 0x%x. Click OK to shut down this system and restart in Directory Services Restore Mode. Check the event log for more detailed information. |
| 0xC00002CC<br>STATUS\_ONLY\_IF\_CONNECTED | This operation is supported only when you are connected to the server. |
| 0xC00002CD<br>STATUS\_DS\_SENSITIVE\_GROUP\_VIOLATION | Only an administrator can modify the membership list of an administrative group. |
| 0xC00002CE<br>STATUS\_PNP\_RESTART\_ENUMERATION | A device was removed so enumeration must be restarted. |
| 0xC00002CF<br>STATUS\_JOURNAL\_ENTRY\_DELETED | The journal entry has been deleted from the journal. |
| 0xC00002D0<br>STATUS\_DS\_CANT\_MOD\_PRIMARYGROUPID | Cannot change the primary group ID of a domain controller account. |
| 0xC00002D1<br>STATUS\_SYSTEM\_IMAGE\_BAD\_SIGNATURE | {Fatal System Error} The system image %s is not properly signed. The file has been replaced with the signed file. The system has been shut down. |
| 0xC00002D2<br>STATUS\_PNP\_REBOOT\_REQUIRED | The device will not start without a reboot. |
| 0xC00002D3<br>STATUS\_POWER\_STATE\_INVALID | The power state of the current device cannot support this request. |
| 0xC00002D4<br>STATUS\_DS\_INVALID\_GROUP\_TYPE | The specified group type is invalid. |
| 0xC00002D5<br>STATUS\_DS\_NO\_NEST\_GLOBALGROUP\_IN\_MIXEDDOMAIN | In a mixed domain, no nesting of a global group if the group is security enabled. |
| 0xC00002D6<br>STATUS\_DS\_NO\_NEST\_LOCALGROUP\_IN\_MIXEDDOMAIN | In a mixed domain, cannot nest local groups with other local groups, if the group is security enabled. |
| 0xC00002D7<br>STATUS\_DS\_GLOBAL\_CANT\_HAVE\_LOCAL\_MEMBER | A global group cannot have a local group as a member. |
| 0xC00002D8<br>STATUS\_DS\_GLOBAL\_CANT\_HAVE\_UNIVERSAL\_MEMBER | A global group cannot have a universal group as a member. |
| 0xC00002D9<br>STATUS\_DS\_UNIVERSAL\_CANT\_HAVE\_LOCAL\_MEMBER | A universal group cannot have a local group as a member. |
| 0xC00002DA<br>STATUS\_DS\_GLOBAL\_CANT\_HAVE\_CROSSDOMAIN\_MEMBER | A global group cannot have a cross-domain member. |
| 0xC00002DB<br>STATUS\_DS\_LOCAL\_CANT\_HAVE\_CROSSDOMAIN\_LOCAL\_MEMBER | A local group cannot have another cross-domain local group as a member. |
| 0xC00002DC<br>STATUS\_DS\_HAVE\_PRIMARY\_MEMBERS | Cannot change to a security-disabled group because primary members are in this group. |
| 0xC00002DD<br>STATUS\_WMI\_NOT\_SUPPORTED | The WMI operation is not supported by the data block or method. |
| 0xC00002DE<br>STATUS\_INSUFFICIENT\_POWER | There is not enough power to complete the requested operation. |
| 0xC00002DF<br>STATUS\_SAM\_NEED\_BOOTKEY\_PASSWORD | The Security Accounts Manager needs to get the boot password. |
| 0xC00002E0<br>STATUS\_SAM\_NEED\_BOOTKEY\_FLOPPY | The Security Accounts Manager needs to get the boot key from the floppy disk. |
| 0xC00002E1<br>STATUS\_DS\_CANT\_START | The directory service cannot start. |
| 0xC00002E2<br>STATUS\_DS\_INIT\_FAILURE | The directory service could not start because of the following error: %hs Error Status: 0x%x. Click OK to shut down this system and restart in Directory Services Restore Mode. Check the event log for more detailed information. |
| 0xC00002E3<br>STATUS\_SAM\_INIT\_FAILURE | The Security Accounts Manager initialization failed because of the following error: %hs Error Status: 0x%x. Click OK to shut down this system and restart in Safe Mode. Check the event log for more detailed information. |
| 0xC00002E4<br>STATUS\_DS\_GC\_REQUIRED | The requested operation can be performed only on a global catalog server. |
| 0xC00002E5<br>STATUS\_DS\_LOCAL\_MEMBER\_OF\_LOCAL\_ONLY | A local group can only be a member of other local groups in the same domain. |
| 0xC00002E6<br>STATUS\_DS\_NO\_FPO\_IN\_UNIVERSAL\_GROUPS | Foreign security principals cannot be members of universal groups. |
| 0xC00002E7<br>STATUS\_DS\_MACHINE\_ACCOUNT\_QUOTA\_EXCEEDED | Your computer could not be joined to the domain. You have exceeded the maximum number of computer accounts you are allowed to create in this domain. Contact your system administrator to have this limit reset or increased. |
| 0xC00002E9<br>STATUS\_CURRENT\_DOMAIN\_NOT\_ALLOWED | This operation cannot be performed on the current domain. |
| 0xC00002EA<br>STATUS\_CANNOT\_MAKE | The directory or file cannot be created. |
| 0xC00002EB<br>STATUS\_SYSTEM\_SHUTDOWN | The system is in the process of shutting down. |
| 0xC00002EC<br>STATUS\_DS\_INIT\_FAILURE\_CONSOLE | Directory Services could not start because of the following error: %hs Error Status: 0x%x. Click OK to shut down the system. You can use the recovery console to diagnose the system further. |
| 0xC00002ED<br>STATUS\_DS\_SAM\_INIT\_FAILURE\_CONSOLE | Security Accounts Manager initialization failed because of the following error: %hs Error Status: 0x%x. Click OK to shut down the system. You can use the recovery console to diagnose the system further. |
| 0xC00002EE<br>STATUS\_UNFINISHED\_CONTEXT\_DELETED | A security context was deleted before the context was completed. This is considered a logon failure. |
| 0xC00002EF<br>STATUS\_NO\_TGT\_REPLY | The client is trying to negotiate a context and the server requires user-to-user but did not send a TGT reply. |
| 0xC00002F0<br>STATUS\_OBJECTID\_NOT\_FOUND | An object ID was not found in the file. |
| 0xC00002F1<br>STATUS\_NO\_IP\_ADDRESSES | Unable to accomplish the requested task because the local machine does not have any IP addresses. |
| 0xC00002F2<br>STATUS\_WRONG\_CREDENTIAL\_HANDLE | The supplied credential handle does not match the credential that is associated with the security context. |
| 0xC00002F3<br>STATUS\_CRYPTO\_SYSTEM\_INVALID | The crypto system or checksum function is invalid because a required function is unavailable. |
| 0xC00002F4<br>STATUS\_MAX\_REFERRALS\_EXCEEDED | The number of maximum ticket referrals has been exceeded. |
| 0xC00002F5<br>STATUS\_MUST\_BE\_KDC | The local machine must be a Kerberos KDC (domain controller) and it is not. |
| 0xC00002F6<br>STATUS\_STRONG\_CRYPTO\_NOT\_SUPPORTED | The other end of the security negotiation requires strong crypto but it is not supported on the local machine. |
| 0xC00002F7<br>STATUS\_TOO\_MANY\_PRINCIPALS | The KDC reply contained more than one principal name. |
| 0xC00002F8<br>STATUS\_NO\_PA\_DATA | Expected to find PA data for a hint of what etype to use, but it was not found. |
| 0xC00002F9<br>STATUS\_PKINIT\_NAME\_MISMATCH | The client certificate does not contain a valid UPN, or does not match the client name in the logon request. Contact your administrator. |
| 0xC00002FA<br>STATUS\_SMARTCARD\_LOGON\_REQUIRED | Smart card logon is required and was not used. |
| 0xC00002FB<br>STATUS\_KDC\_INVALID\_REQUEST | An invalid request was sent to the KDC. |
| 0xC00002FC<br>STATUS\_KDC\_UNABLE\_TO\_REFER | The KDC was unable to generate a referral for the service requested. |
| 0xC00002FD<br>STATUS\_KDC\_UNKNOWN\_ETYPE | The encryption type requested is not supported by the KDC. |
| 0xC00002FE<br>STATUS\_SHUTDOWN\_IN\_PROGRESS | A system shutdown is in progress. |
| 0xC00002FF<br>STATUS\_SERVER\_SHUTDOWN\_IN\_PROGRESS | The server machine is shutting down. |
| 0xC0000300<br>STATUS\_NOT\_SUPPORTED\_ON\_SBS | This operation is not supported on a computer running Windows Server 2003 operating system for Small Business Server. |
| 0xC0000301<br>STATUS\_WMI\_GUID\_DISCONNECTED | The WMI GUID is no longer available. |
| 0xC0000302<br>STATUS\_WMI\_ALREADY\_DISABLED | Collection or events for the WMI GUID is already disabled. |
| 0xC0000303<br>STATUS\_WMI\_ALREADY\_ENABLED | Collection or events for the WMI GUID is already enabled. |
| 0xC0000304<br>STATUS\_MFT\_TOO\_FRAGMENTED | The master file table on the volume is too fragmented to complete this operation. |
| 0xC0000305<br>STATUS\_COPY\_PROTECTION\_FAILURE | Copy protection failure. |
| 0xC0000306<br>STATUS\_CSS\_AUTHENTICATION\_FAILURE | Copy protection error—DVD CSS Authentication failed. |
| 0xC0000307<br>STATUS\_CSS\_KEY\_NOT\_PRESENT | Copy protection error—The specified sector does not contain a valid key. |
| 0xC0000308<br>STATUS\_CSS\_KEY\_NOT\_ESTABLISHED | Copy protection error—DVD session key not established. |
| 0xC0000309<br>STATUS\_CSS\_SCRAMBLED\_SECTOR | Copy protection error—The read failed because the sector is encrypted. |
| 0xC000030A<br>STATUS\_CSS\_REGION\_MISMATCH | Copy protection error—The region of the specified DVD does not correspond to the region setting of the drive. |
| 0xC000030B<br>STATUS\_CSS\_RESETS\_EXHAUSTED | Copy protection error—The region setting of the drive might be permanent. |
| 0xC0000320<br>STATUS\_PKINIT\_FAILURE | The Kerberos protocol encountered an error while validating the KDC certificate during smart card logon. There is more information in the system event log. |
| 0xC0000321<br>STATUS\_SMARTCARD\_SUBSYSTEM\_FAILURE | The Kerberos protocol encountered an error while attempting to use the smart card subsystem. |
| 0xC0000322<br>STATUS\_NO\_KERB\_KEY | The target server does not have acceptable Kerberos credentials. |
| 0xC0000350<br>STATUS\_HOST\_DOWN | The transport determined that the remote system is down. |
| 0xC0000351<br>STATUS\_UNSUPPORTED\_PREAUTH | An unsupported pre-authentication mechanism was presented to the Kerberos package. |
| 0xC0000352<br>STATUS\_EFS\_ALG\_BLOB\_TOO\_BIG | The encryption algorithm that is used on the source file needs a bigger key buffer than the one that is used on the destination file. |
| 0xC0000353<br>STATUS\_PORT\_NOT\_SET | An attempt to remove a processes DebugPort was made, but a port was not already associated with the process. |
| 0xC0000354<br>STATUS\_DEBUGGER\_INACTIVE | An attempt to do an operation on a debug port failed because the port is in the process of being deleted. |
| 0xC0000355<br>STATUS\_DS\_VERSION\_CHECK\_FAILURE | This version of Windows is not compatible with the behavior version of the directory forest, domain, or domain controller. |
| 0xC0000356<br>STATUS\_AUDITING\_DISABLED | The specified event is currently not being audited. |
| 0xC0000357<br>STATUS\_PRENT4\_MACHINE\_ACCOUNT | The machine account was created prior to Windows NT 4.0 operating system. The account needs to be recreated. |
| 0xC0000358<br>STATUS\_DS\_AG\_CANT\_HAVE\_UNIVERSAL\_MEMBER | An account group cannot have a universal group as a member. |
| 0xC0000359<br>STATUS\_INVALID\_IMAGE\_WIN\_32 | The specified image file did not have the correct format; it appears to be a 32-bit Windows image. |
| 0xC000035A<br>STATUS\_INVALID\_IMAGE\_WIN\_64 | The specified image file did not have the correct format; it appears to be a 64-bit Windows image. |
| 0xC000035B<br>STATUS\_BAD\_BINDINGS | The client's supplied SSPI channel bindings were incorrect. |
| 0xC000035C<br>STATUS\_NETWORK\_SESSION\_EXPIRED | The client session has expired; so the client must re-authenticate to continue accessing the remote resources. |
| 0xC000035D<br>STATUS\_APPHELP\_BLOCK | The AppHelp dialog box canceled; thus preventing the application from starting. |
| 0xC000035E<br>STATUS\_ALL\_SIDS\_FILTERED | The SID filtering operation removed all SIDs. |
| 0xC000035F<br>STATUS\_NOT\_SAFE\_MODE\_DRIVER | The driver was not loaded because the system is starting in safe mode. |
| 0xC0000361<br>STATUS\_ACCESS\_DISABLED\_BY\_POLICY\_DEFAULT | Access to %1 has been restricted by your Administrator by the default software restriction policy level. |
| 0xC0000362<br>STATUS\_ACCESS\_DISABLED\_BY\_POLICY\_PATH | Access to %1 has been restricted by your Administrator by location with policy rule %2 placed on path %3. |
| 0xC0000363<br>STATUS\_ACCESS\_DISABLED\_BY\_POLICY\_PUBLISHER | Access to %1 has been restricted by your Administrator by software publisher policy. |
| 0xC0000364<br>STATUS\_ACCESS\_DISABLED\_BY\_POLICY\_OTHER | Access to %1 has been restricted by your Administrator by policy rule %2. |
| 0xC0000365<br>STATUS\_FAILED\_DRIVER\_ENTRY | The driver was not loaded because it failed its initialization call. |
| 0xC0000366<br>STATUS\_DEVICE\_ENUMERATION\_ERROR | The device encountered an error while applying power or reading the device configuration. This might be caused by a failure of your hardware or by a poor connection. |
| 0xC0000368<br>STATUS\_MOUNT\_POINT\_NOT\_RESOLVED | The create operation failed because the name contained at least one mount point that resolves to a volume to which the specified device object is not attached. |
| 0xC0000369<br>STATUS\_INVALID\_DEVICE\_OBJECT\_PARAMETER | The device object parameter is either not a valid device object or is not attached to the volume that is specified by the file name. |
| 0xC000036A<br>STATUS\_MCA\_OCCURED | A machine check error has occurred. Check the system event log for additional information. |
| 0xC000036B<br>STATUS\_DRIVER\_BLOCKED\_CRITICAL | Driver %2 has been blocked from loading. |
| 0xC000036C<br>STATUS\_DRIVER\_BLOCKED | Driver %2 has been blocked from loading. |
| 0xC000036D<br>STATUS\_DRIVER\_DATABASE\_ERROR | There was error [%2] processing the driver database. |
| 0xC000036E<br>STATUS\_SYSTEM\_HIVE\_TOO\_LARGE | System hive size has exceeded its limit. |
| 0xC000036F<br>STATUS\_INVALID\_IMPORT\_OF\_NON\_DLL | A dynamic link library (DLL) referenced a module that was neither a DLL nor the process's executable image. |
| 0xC0000371<br>STATUS\_NO\_SECRETS | The local account store does not contain secret material for the specified account. |
| 0xC0000372<br>STATUS\_ACCESS\_DISABLED\_NO\_SAFER\_UI\_BY\_POLICY | Access to %1 has been restricted by your Administrator by policy rule %2. |
| 0xC0000373<br>STATUS\_FAILED\_STACK\_SWITCH | The system was not able to allocate enough memory to perform a stack switch. |
| 0xC0000374<br>STATUS\_HEAP\_CORRUPTION | A heap has been corrupted. |
| 0xC0000380<br>STATUS\_SMARTCARD\_WRONG\_PIN | An incorrect PIN was presented to the smart card. |
| 0xC0000381<br>STATUS\_SMARTCARD\_CARD\_BLOCKED | The smart card is blocked. |
| 0xC0000382<br>STATUS\_SMARTCARD\_CARD\_NOT\_AUTHENTICATED | No PIN was presented to the smart card. |
| 0xC0000383<br>STATUS\_SMARTCARD\_NO\_CARD | No smart card is available. |
| 0xC0000384<br>STATUS\_SMARTCARD\_NO\_KEY\_CONTAINER | The requested key container does not exist on the smart card. |
| 0xC0000385<br>STATUS\_SMARTCARD\_NO\_CERTIFICATE | The requested certificate does not exist on the smart card. |
| 0xC0000386<br>STATUS\_SMARTCARD\_NO\_KEYSET | The requested keyset does not exist. |
| 0xC0000387<br>STATUS\_SMARTCARD\_IO\_ERROR | A communication error with the smart card has been detected. |
| 0xC0000388<br>STATUS\_DOWNGRADE\_DETECTED | The system detected a possible attempt to compromise security. Ensure that you can contact the server that authenticated you. |
| 0xC0000389<br>STATUS\_SMARTCARD\_CERT\_REVOKED | The smart card certificate used for authentication has been revoked. Contact your system administrator. There might be additional information in the event log. |
| 0xC000038A<br>STATUS\_ISSUING\_CA\_UNTRUSTED | An untrusted certificate authority was detected while processing the smart card certificate that is used for authentication. Contact your system administrator. |
| 0xC000038B<br>STATUS\_REVOCATION\_OFFLINE\_C | The revocation status of the smart card certificate that is used for authentication could not be determined. Contact your system administrator. |
| 0xC000038C<br>STATUS\_PKINIT\_CLIENT\_FAILURE | The smart card certificate used for authentication was not trusted. Contact your system administrator. |
| 0xC000038D<br>STATUS\_SMARTCARD\_CERT\_EXPIRED | The smart card certificate used for authentication has expired. Contact your system administrator. |
| 0xC000038E<br>STATUS\_DRIVER\_FAILED\_PRIOR\_UNLOAD | The driver could not be loaded because a previous version of the driver is still in memory. |
| 0xC000038F<br>STATUS\_SMARTCARD\_SILENT\_CONTEXT | The smart card provider could not perform the action because the context was acquired as silent. |
| 0xC0000401<br>STATUS\_PER\_USER\_TRUST\_QUOTA\_EXCEEDED | The delegated trust creation quota of the current user has been exceeded. |
| 0xC0000402<br>STATUS\_ALL\_USER\_TRUST\_QUOTA\_EXCEEDED | The total delegated trust creation quota has been exceeded. |
| 0xC0000403<br>STATUS\_USER\_DELETE\_TRUST\_QUOTA\_EXCEEDED | The delegated trust deletion quota of the current user has been exceeded. |
| 0xC0000404<br>STATUS\_DS\_NAME\_NOT\_UNIQUE | The requested name already exists as a unique identifier. |
| 0xC0000405<br>STATUS\_DS\_DUPLICATE\_ID\_FOUND | The requested object has a non-unique identifier and cannot be retrieved. |
| 0xC0000406<br>STATUS\_DS\_GROUP\_CONVERSION\_ERROR | The group cannot be converted due to attribute restrictions on the requested group type. |
| 0xC0000407<br>STATUS\_VOLSNAP\_PREPARE\_HIBERNATE | {Volume Shadow Copy Service} Wait while the Volume Shadow Copy Service prepares volume %hs for hibernation. |
| 0xC0000408<br>STATUS\_USER2USER\_REQUIRED | Kerberos sub-protocol User2User is required. |
| 0xC0000409<br>STATUS\_STACK\_BUFFER\_OVERRUN | The system detected an overrun of a stack-based buffer in this application. This overrun could potentially allow a malicious user to gain control of this application. |
| 0xC000040A<br>STATUS\_NO\_S4U\_PROT\_SUPPORT | The Kerberos subsystem encountered an error. A service for user protocol request was made against a domain controller which does not support service for user. |
| 0xC000040B<br>STATUS\_CROSSREALM\_DELEGATION\_FAILURE | An attempt was made by this server to make a Kerberos constrained delegation request for a target that is outside the server realm. This action is not supported and the resulting error indicates a misconfiguration on the allowed-to-delegate-to list for this server. Contact your administrator. |
| 0xC000040C<br>STATUS\_REVOCATION\_OFFLINE\_KDC | The revocation status of the domain controller certificate used for smart card authentication could not be determined. There is additional information in the system event log. Contact your system administrator. |
| 0xC000040D<br>STATUS\_ISSUING\_CA\_UNTRUSTED\_KDC | An untrusted certificate authority was detected while processing the domain controller certificate used for authentication. There is additional information in the system event log. Contact your system administrator. |
| 0xC000040E<br>STATUS\_KDC\_CERT\_EXPIRED | The domain controller certificate used for smart card logon has expired. Contact your system administrator with the contents of your system event log. |
| 0xC000040F<br>STATUS\_KDC\_CERT\_REVOKED | The domain controller certificate used for smart card logon has been revoked. Contact your system administrator with the contents of your system event log. |
| 0xC0000410<br>STATUS\_PARAMETER\_QUOTA\_EXCEEDED | Data present in one of the parameters is more than the function can operate on. |
| 0xC0000411<br>STATUS\_HIBERNATION\_FAILURE | The system has failed to hibernate (The error code is %hs). Hibernation will be disabled until the system is restarted. |
| 0xC0000412<br>STATUS\_DELAY\_LOAD\_FAILED | An attempt to delay-load a .dll or get a function address in a delay-loaded .dll failed. |
| 0xC0000413<br>STATUS\_AUTHENTICATION\_FIREWALL\_FAILED | Logon Failure: The machine you are logging onto is protected by an authentication firewall. The specified account is not allowed to authenticate to the machine. |
| 0xC0000414<br>STATUS\_VDM\_DISALLOWED | %hs is a 16-bit application. You do not have permissions to execute 16-bit applications. Check your permissions with your system administrator. |
| 0xC0000415<br>STATUS\_HUNG\_DISPLAY\_DRIVER\_THREAD | {Display Driver Stopped Responding} The %hs display driver has stopped working normally. Save your work and reboot the system to restore full display functionality. The next time you reboot the machine a dialog will be displayed giving you a chance to report this failure to Microsoft. |
| 0xC0000416<br>STATUS\_INSUFFICIENT\_RESOURCE\_FOR\_SPECIFIED\_SHARED\_SECTION\_SIZE | The Desktop heap encountered an error while allocating session memory. There is more information in the system event log. |
| 0xC0000417<br>STATUS\_INVALID\_CRUNTIME\_PARAMETER | An invalid parameter was passed to a C runtime function. |
| 0xC0000418<br>STATUS\_NTLM\_BLOCKED | The authentication failed because NTLM was blocked. |
| 0xC0000419<br>STATUS\_DS\_SRC\_SID\_EXISTS\_IN\_FOREST | The source object's SID already exists in destination forest. |
| 0xC000041A<br>STATUS\_DS\_DOMAIN\_NAME\_EXISTS\_IN\_FOREST | The domain name of the trusted domain already exists in the forest. |
| 0xC000041B<br>STATUS\_DS\_FLAT\_NAME\_EXISTS\_IN\_FOREST | The flat name of the trusted domain already exists in the forest. |
| 0xC000041C<br>STATUS\_INVALID\_USER\_PRINCIPAL\_NAME | The User Principal Name (UPN) is invalid. |
| 0xC0000420<br>STATUS\_ASSERTION\_FAILURE | There has been an assertion failure. |
| 0xC0000421<br>STATUS\_VERIFIER\_STOP | Application verifier has found an error in the current process. |
| 0xC0000423<br>STATUS\_CALLBACK\_POP\_STACK | A user mode unwind is in progress. |
| 0xC0000424<br>STATUS\_INCOMPATIBLE\_DRIVER\_BLOCKED | %2 has been blocked from loading due to incompatibility with this system. Contact your software vendor for a compatible version of the driver. |
| 0xC0000425<br>STATUS\_HIVE\_UNLOADED | Illegal operation attempted on a registry key which has already been unloaded. |
| 0xC0000426<br>STATUS\_COMPRESSION\_DISABLED | Compression is disabled for this volume. |
| 0xC0000427<br>STATUS\_FILE\_SYSTEM\_LIMITATION | The requested operation could not be completed due to a file system limitation. |
| 0xC0000428<br>STATUS\_INVALID\_IMAGE\_HASH | The hash for image %hs cannot be found in the system catalogs. The image is likely corrupt or the victim of tampering. |
| 0xC0000429<br>STATUS\_NOT\_CAPABLE | The implementation is not capable of performing the request. |
| 0xC000042A<br>STATUS\_REQUEST\_OUT\_OF\_SEQUENCE | The requested operation is out of order with respect to other operations. |
| 0xC000042B<br>STATUS\_IMPLEMENTATION\_LIMIT | An operation attempted to exceed an implementation-defined limit. |
| 0xC000042C<br>STATUS\_ELEVATION\_REQUIRED | The requested operation requires elevation. |
| 0xC000042D<br>STATUS\_NO\_SECURITY\_CONTEXT | The required security context does not exist. |
| 0xC000042E<br>STATUS\_PKU2U\_CERT\_FAILURE | The PKU2U protocol encountered an error while attempting to utilize the associated certificates. |
| 0xC0000432<br>STATUS\_BEYOND\_VDL | The operation was attempted beyond the valid data length of the file. |
| 0xC0000433<br>STATUS\_ENCOUNTERED\_WRITE\_IN\_PROGRESS | The attempted write operation encountered a write already in progress for some portion of the range. |
| 0xC0000434<br>STATUS\_PTE\_CHANGED | The page fault mappings changed in the middle of processing a fault so the operation must be retried. |
| 0xC0000435<br>STATUS\_PURGE\_FAILED | The attempt to purge this file from memory failed to purge some or all the data from memory. |
| 0xC0000440<br>STATUS\_CRED\_REQUIRES\_CONFIRMATION | The requested credential requires confirmation. |
| 0xC0000441<br>STATUS\_CS\_ENCRYPTION\_INVALID\_SERVER\_RESPONSE | The remote server sent an invalid response for a file being opened with Client Side Encryption. |
| 0xC0000442<br>STATUS\_CS\_ENCRYPTION\_UNSUPPORTED\_SERVER | Client Side Encryption is not supported by the remote server even though it claims to support it. |
| 0xC0000443<br>STATUS\_CS\_ENCRYPTION\_EXISTING\_ENCRYPTED\_FILE | File is encrypted and should be opened in Client Side Encryption mode. |
| 0xC0000444<br>STATUS\_CS\_ENCRYPTION\_NEW\_ENCRYPTED\_FILE | A new encrypted file is being created and a $EFS needs to be provided. |
| 0xC0000445<br>STATUS\_CS\_ENCRYPTION\_FILE\_NOT\_CSE | The SMB client requested a CSE FSCTL on a non-CSE file. |
| 0xC0000446<br>STATUS\_INVALID\_LABEL | Indicates a particular Security ID cannot be assigned as the label of an object. |
| 0xC0000450<br>STATUS\_DRIVER\_PROCESS\_TERMINATED | The process hosting the driver for this device has terminated. |
| 0xC0000451<br>STATUS\_AMBIGUOUS\_SYSTEM\_DEVICE | The requested system device cannot be identified due to multiple indistinguishable devices potentially matching the identification criteria. |
| 0xC0000452<br>STATUS\_SYSTEM\_DEVICE\_NOT\_FOUND | The requested system device cannot be found. |
| 0xC0000453<br>STATUS\_RESTART\_BOOT\_APPLICATION | This boot application must be restarted. |
| 0xC0000454<br>STATUS\_INSUFFICIENT\_NVRAM\_RESOURCES | Insufficient NVRAM resources exist to complete the API. A reboot might be required. |
| 0xC0000460<br>STATUS\_NO\_RANGES\_PROCESSED | No ranges for the specified operation were able to be processed. |
| 0xC0000463<br>STATUS\_DEVICE\_FEATURE\_NOT\_SUPPORTED | The storage device does not support Offload Write. |
| 0xC0000464<br>STATUS\_DEVICE\_UNREACHABLE | Data cannot be moved because the source device cannot communicate with the destination device. |
| 0xC0000465<br>STATUS\_INVALID\_TOKEN | The token representing the data is invalid or expired. |
| 0xC0000466<br>STATUS\_SERVER\_UNAVAILABLE | The file server is temporarily unavailable. |
| 0xC0000500<br>STATUS\_INVALID\_TASK\_NAME | The specified task name is invalid. |
| 0xC0000501<br>STATUS\_INVALID\_TASK\_INDEX | The specified task index is invalid. |
| 0xC0000502<br>STATUS\_THREAD\_ALREADY\_IN\_TASK | The specified thread is already joining a task. |
| 0xC0000503<br>STATUS\_CALLBACK\_BYPASS | A callback has requested to bypass native code. |
| 0xC0000602<br>STATUS\_FAIL\_FAST\_EXCEPTION | A fail fast exception occurred. Exception handlers will not be invoked and the process will be terminated immediately. |
| 0xC0000603<br>STATUS\_IMAGE\_CERT\_REVOKED | Windows cannot verify the digital signature for this file. The signing certificate for this file has been revoked. |
| 0xC0000700<br>STATUS\_PORT\_CLOSED | The ALPC port is closed. |
| 0xC0000701<br>STATUS\_MESSAGE\_LOST | The ALPC message requested is no longer available. |
| 0xC0000702<br>STATUS\_INVALID\_MESSAGE | The ALPC message supplied is invalid. |
| 0xC0000703<br>STATUS\_REQUEST\_CANCELED | The ALPC message has been canceled. |
| 0xC0000704<br>STATUS\_RECURSIVE\_DISPATCH | Invalid recursive dispatch attempt. |
| 0xC0000705<br>STATUS\_LPC\_RECEIVE\_BUFFER\_EXPECTED | No receive buffer has been supplied in a synchronous request. |
| 0xC0000706<br>STATUS\_LPC\_INVALID\_CONNECTION\_USAGE | The connection port is used in an invalid context. |
| 0xC0000707<br>STATUS\_LPC\_REQUESTS\_NOT\_ALLOWED | The ALPC port does not accept new request messages. |
| 0xC0000708<br>STATUS\_RESOURCE\_IN\_USE | The resource requested is already in use. |
| 0xC0000709<br>STATUS\_HARDWARE\_MEMORY\_ERROR | The hardware has reported an uncorrectable memory error. |
| 0xC000070A<br>STATUS\_THREADPOOL\_HANDLE\_EXCEPTION | Status 0x%08x was returned, waiting on handle 0x%x for wait 0x%p, in waiter 0x%p. |
| 0xC000070B<br>STATUS\_THREADPOOL\_SET\_EVENT\_ON\_COMPLETION\_FAILED | After a callback to 0x%p(0x%p), a completion call to Set event(0x%p) failed with status 0x%08x. |
| 0xC000070C<br>STATUS\_THREADPOOL\_RELEASE\_SEMAPHORE\_ON\_COMPLETION\_FAILED | After a callback to 0x%p(0x%p), a completion call to ReleaseSemaphore(0x%p, %d) failed with status 0x%08x. |
| 0xC000070D<br>STATUS\_THREADPOOL\_RELEASE\_MUTEX\_ON\_COMPLETION\_FAILED | After a callback to 0x%p(0x%p), a completion call to ReleaseMutex(%p) failed with status 0x%08x. |
| 0xC000070E<br>STATUS\_THREADPOOL\_FREE\_LIBRARY\_ON\_COMPLETION\_FAILED | After a callback to 0x%p(0x%p), a completion call to FreeLibrary(%p) failed with status 0x%08x. |
| 0xC000070F<br>STATUS\_THREADPOOL\_RELEASED\_DURING\_OPERATION | The thread pool 0x%p was released while a thread was posting a callback to 0x%p(0x%p) to it. |
| 0xC0000710<br>STATUS\_CALLBACK\_RETURNED\_WHILE\_IMPERSONATING | A thread pool worker thread is impersonating a client, after a callback to 0x%p(0x%p). This is unexpected, indicating that the callback is missing a call to revert the impersonation. |
| 0xC0000711<br>STATUS\_APC\_RETURNED\_WHILE\_IMPERSONATING | A thread pool worker thread is impersonating a client, after executing an APC. This is unexpected, indicating that the APC is missing a call to revert the impersonation. |
| 0xC0000712<br>STATUS\_PROCESS\_IS\_PROTECTED | Either the target process, or the target thread's containing process, is a protected process. |
| 0xC0000713<br>STATUS\_MCA\_EXCEPTION | A thread is getting dispatched with MCA EXCEPTION because of MCA. |
| 0xC0000714<br>STATUS\_CERTIFICATE\_MAPPING\_NOT\_UNIQUE | The client certificate account mapping is not unique. |
| 0xC0000715<br>STATUS\_SYMLINK\_CLASS\_DISABLED | The symbolic link cannot be followed because its type is disabled. |
| 0xC0000716<br>STATUS\_INVALID\_IDN\_NORMALIZATION | Indicates that the specified string is not valid for IDN normalization. |
| 0xC0000717<br>STATUS\_NO\_UNICODE\_TRANSLATION | No mapping for the Unicode character exists in the target multi-byte code page. |
| 0xC0000718<br>STATUS\_ALREADY\_REGISTERED | The provided callback is already registered. |
| 0xC0000719<br>STATUS\_CONTEXT\_MISMATCH | The provided context did not match the target. |
| 0xC000071A<br>STATUS\_PORT\_ALREADY\_HAS\_COMPLETION\_LIST | The specified port already has a completion list. |
| 0xC000071B<br>STATUS\_CALLBACK\_RETURNED\_THREAD\_PRIORITY | A threadpool worker thread entered a callback at thread base priority 0x%x and exited at priority 0x%x.<br>This is unexpected, indicating that the callback missed restoring the priority. |
| 0xC000071C<br>STATUS\_INVALID\_THREAD | An invalid thread, handle %p, is specified for this operation. Possibly, a threadpool worker thread was specified. |
| 0xC000071D<br>STATUS\_CALLBACK\_RETURNED\_TRANSACTION | A threadpool worker thread entered a callback, which left transaction state.<br>This is unexpected, indicating that the callback missed clearing the transaction. |
| 0xC000071E<br>STATUS\_CALLBACK\_RETURNED\_LDR\_LOCK | A threadpool worker thread entered a callback, which left the loader lock held.<br>This is unexpected, indicating that the callback missed releasing the lock. |
| 0xC000071F<br>STATUS\_CALLBACK\_RETURNED\_LANG | A threadpool worker thread entered a callback, which left with preferred languages set.<br>This is unexpected, indicating that the callback missed clearing them. |
| 0xC0000720<br>STATUS\_CALLBACK\_RETURNED\_PRI\_BACK | A threadpool worker thread entered a callback, which left with background priorities set.<br>This is unexpected, indicating that the callback missed restoring the original priorities. |
| 0xC0000800<br>STATUS\_DISK\_REPAIR\_DISABLED | The attempted operation required self healing to be enabled. |
| 0xC0000801<br>STATUS\_DS\_DOMAIN\_RENAME\_IN\_PROGRESS | The directory service cannot perform the requested operation because a domain rename operation is in progress. |
| 0xC0000802<br>STATUS\_DISK\_QUOTA\_EXCEEDED | An operation failed because the storage quota was exceeded. |
| 0xC0000804<br>STATUS\_CONTENT\_BLOCKED | An operation failed because the content was blocked. |
| 0xC0000805<br>STATUS\_BAD\_CLUSTERS | The operation could not be completed due to bad clusters on disk. |
| 0xC0000806<br>STATUS\_VOLUME\_DIRTY | The operation could not be completed because the volume is dirty. Please run the Chkdsk utility and try again. |
| 0xC0000901<br>STATUS\_FILE\_CHECKED\_OUT | This file is checked out or locked for editing by another user. |
| 0xC0000902<br>STATUS\_CHECKOUT\_REQUIRED | The file must be checked out before saving changes. |
| 0xC0000903<br>STATUS\_BAD\_FILE\_TYPE | The file type being saved or retrieved has been blocked. |
| 0xC0000904<br>STATUS\_FILE\_TOO\_LARGE | The file size exceeds the limit allowed and cannot be saved. |
| 0xC0000905<br>STATUS\_FORMS\_AUTH\_REQUIRED | Access Denied. Before opening files in this location, you must first browse to the e.g. site and select the option to log on automatically. |
| 0xC0000906<br>STATUS\_VIRUS\_INFECTED | The operation did not complete successfully because the file contains a virus. |
| 0xC0000907<br>STATUS\_VIRUS\_DELETED | This file contains a virus and cannot be opened. Due to the nature of this virus, the file has been removed from this location. |
| 0xC0000908<br>STATUS\_BAD\_MCFG\_TABLE | The resources required for this device conflict with the MCFG table. |
| 0xC000090B<br>STATUS\_BAD\_DATA | Bad data. |
| 0xC0000909<br>STATUS\_CANNOT\_BREAK\_OPLOCK | The operation did not complete successfully because it would cause an oplock to be broken. The caller has requested that existing oplocks not be broken. |
| 0xC0009898<br>STATUS\_WOW\_ASSERTION | WOW Assertion Error. |
| 0xC000A000<br>STATUS\_INVALID\_SIGNATURE | The cryptographic signature is invalid. |
| 0xC000A001<br>STATUS\_HMAC\_NOT\_SUPPORTED | The cryptographic provider does not support HMAC. |
| 0xC000A002<br>STATUS\_AUTH\_TAG\_MISMATCH | The computed authentication tag did not match the input authentication tag. |
| 0xC000A010<br>STATUS\_IPSEC\_QUEUE\_OVERFLOW | The IPsec queue overflowed. |
| 0xC000A011<br>STATUS\_ND\_QUEUE\_OVERFLOW | The neighbor discovery queue overflowed. |
| 0xC000A012<br>STATUS\_HOPLIMIT\_EXCEEDED | An Internet Control Message Protocol (ICMP) hop limit exceeded error was received. |
| 0xC000A013<br>STATUS\_PROTOCOL\_NOT\_SUPPORTED | The protocol is not installed on the local machine. |
| 0xC000A080<br>STATUS\_LOST\_WRITEBEHIND\_DATA\_NETWORK\_DISCONNECTED | {Delayed Write Failed} Windows was unable to save all the data for the file %hs; the data has been lost. This error might be caused by network connectivity issues. Try to save this file elsewhere. |
| 0xC000A081<br>STATUS\_LOST\_WRITEBEHIND\_DATA\_NETWORK\_SERVER\_ERROR | {Delayed Write Failed} Windows was unable to save all the data for the file %hs; the data has been lost. This error was returned by the server on which the file exists. Try to save this file elsewhere. |
| 0xC000A082<br>STATUS\_LOST\_WRITEBEHIND\_DATA\_LOCAL\_DISK\_ERROR | {Delayed Write Failed} Windows was unable to save all the data for the file %hs; the data has been lost. This error might be caused if the device has been removed or the media is write-protected. |
| 0xC000A083<br>STATUS\_XML\_PARSE\_ERROR | Windows was unable to parse the requested XML data. |
| 0xC000A084<br>STATUS\_XMLDSIG\_ERROR | An error was encountered while processing an XML digital signature. |
| 0xC000A085<br>STATUS\_WRONG\_COMPARTMENT | This indicates that the caller made the connection request in the wrong routing compartment. |
| 0xC000A086<br>STATUS\_AUTHIP\_FAILURE | This indicates that there was an AuthIP failure when attempting to connect to the remote host. |
| 0xC000A087<br>STATUS\_DS\_OID\_MAPPED\_GROUP\_CANT\_HAVE\_MEMBERS | OID mapped groups cannot have members. |
| 0xC000A088<br>STATUS\_DS\_OID\_NOT\_FOUND | The specified OID cannot be found. |
| 0xC000A100<br>STATUS\_HASH\_NOT\_SUPPORTED | Hash generation for the specified version and hash type is not enabled on server. |
| 0xC000A101<br>STATUS\_HASH\_NOT\_PRESENT | The hash requests is not present or not up to date with the current file contents. |
| 0xC000A2A1<br>STATUS\_OFFLOAD\_READ\_FLT\_NOT\_SUPPORTED | A file system filter on the server has not opted in for Offload Read support. |
| 0xC000A2A2<br>STATUS\_OFFLOAD\_WRITE\_FLT\_NOT\_SUPPORTED | A file system filter on the server has not opted in for Offload Write support. |
| 0xC000A2A3<br>STATUS\_OFFLOAD\_READ\_FILE\_NOT\_SUPPORTED | Offload read operations cannot be performed on:<br>- Compressed files<br>- Sparse files<br>- Encrypted files<br>- File system metadata files |
| 0xC000A2A4<br>STATUS\_OFFLOAD\_WRITE\_FILE\_NOT\_SUPPORTED | Offload write operations cannot be performed on:<br>- Compressed files<br>- Sparse files<br>- Encrypted files<br>- File system metadata files |
| 0xC0010001<br>DBG\_NO\_STATE\_CHANGE | The debugger did not perform a state change. |
| 0xC0010002<br>DBG\_APP\_NOT\_IDLE | The debugger found that the application is not idle. |
| 0xC0020001<br>RPC\_NT\_INVALID\_STRING\_BINDING | The string binding is invalid. |
| 0xC0020002<br>RPC\_NT\_WRONG\_KIND\_OF\_BINDING | The binding handle is not the correct type. |
| 0xC0020003<br>RPC\_NT\_INVALID\_BINDING | The binding handle is invalid. |
| 0xC0020004<br>RPC\_NT\_PROTSEQ\_NOT\_SUPPORTED | The RPC protocol sequence is not supported. |
| 0xC0020005<br>RPC\_NT\_INVALID\_RPC\_PROTSEQ | The RPC protocol sequence is invalid. |
| 0xC0020006<br>RPC\_NT\_INVALID\_STRING\_UUID | The string UUID is invalid. |
| 0xC0020007<br>RPC\_NT\_INVALID\_ENDPOINT\_FORMAT | The endpoint format is invalid. |
| 0xC0020008<br>RPC\_NT\_INVALID\_NET\_ADDR | The network address is invalid. |
| 0xC0020009<br>RPC\_NT\_NO\_ENDPOINT\_FOUND | No endpoint was found. |
| 0xC002000A<br>RPC\_NT\_INVALID\_TIMEOUT | The time-out value is invalid. |
| 0xC002000B<br>RPC\_NT\_OBJECT\_NOT\_FOUND | The object UUID was not found. |
| 0xC002000C<br>RPC\_NT\_ALREADY\_REGISTERED | The object UUID has already been registered. |
| 0xC002000D<br>RPC\_NT\_TYPE\_ALREADY\_REGISTERED | The type UUID has already been registered. |
| 0xC002000E<br>RPC\_NT\_ALREADY\_LISTENING | The RPC server is already listening. |
| 0xC002000F<br>RPC\_NT\_NO\_PROTSEQS\_REGISTERED | No protocol sequences have been registered. |
| 0xC0020010<br>RPC\_NT\_NOT\_LISTENING | The RPC server is not listening. |
| 0xC0020011<br>RPC\_NT\_UNKNOWN\_MGR\_TYPE | The manager type is unknown. |
| 0xC0020012<br>RPC\_NT\_UNKNOWN\_IF | The interface is unknown. |
| 0xC0020013<br>RPC\_NT\_NO\_BINDINGS | There are no bindings. |
| 0xC0020014<br>RPC\_NT\_NO\_PROTSEQS | There are no protocol sequences. |
| 0xC0020015<br>RPC\_NT\_CANT\_CREATE\_ENDPOINT | The endpoint cannot be created. |
| 0xC0020016<br>RPC\_NT\_OUT\_OF\_RESOURCES | Insufficient resources are available to complete this operation. |
| 0xC0020017<br>RPC\_NT\_SERVER\_UNAVAILABLE | The RPC server is unavailable. |
| 0xC0020018<br>RPC\_NT\_SERVER\_TOO\_BUSY | The RPC server is too busy to complete this operation. |
| 0xC0020019<br>RPC\_NT\_INVALID\_NETWORK\_OPTIONS | The network options are invalid. |
| 0xC002001A<br>RPC\_NT\_NO\_CALL\_ACTIVE | No RPCs are active on this thread. |
| 0xC002001B<br>RPC\_NT\_CALL\_FAILED | The RPC failed. |
| 0xC002001C<br>RPC\_NT\_CALL\_FAILED\_DNE | The RPC failed and did not execute. |
| 0xC002001D<br>RPC\_NT\_PROTOCOL\_ERROR | An RPC protocol error occurred. |
| 0xC002001F<br>RPC\_NT\_UNSUPPORTED\_TRANS\_SYN | The RPC server does not support the transfer syntax. |
| 0xC0020021<br>RPC\_NT\_UNSUPPORTED\_TYPE | The type UUID is not supported. |
| 0xC0020022<br>RPC\_NT\_INVALID\_TAG | The tag is invalid. |
| 0xC0020023<br>RPC\_NT\_INVALID\_BOUND | The array bounds are invalid. |
| 0xC0020024<br>RPC\_NT\_NO\_ENTRY\_NAME | The binding does not contain an entry name. |
| 0xC0020025<br>RPC\_NT\_INVALID\_NAME\_SYNTAX | The name syntax is invalid. |
| 0xC0020026<br>RPC\_NT\_UNSUPPORTED\_NAME\_SYNTAX | The name syntax is not supported. |
| 0xC0020028<br>RPC\_NT\_UUID\_NO\_ADDRESS | No network address is available to construct a UUID. |
| 0xC0020029<br>RPC\_NT\_DUPLICATE\_ENDPOINT | The endpoint is a duplicate. |
| 0xC002002A<br>RPC\_NT\_UNKNOWN\_AUTHN\_TYPE | The authentication type is unknown. |
| 0xC002002B<br>RPC\_NT\_MAX\_CALLS\_TOO\_SMALL | The maximum number of calls is too small. |
| 0xC002002C<br>RPC\_NT\_STRING\_TOO\_LONG | The string is too long. |
| 0xC002002D<br>RPC\_NT\_PROTSEQ\_NOT\_FOUND | The RPC protocol sequence was not found. |
| 0xC002002E<br>RPC\_NT\_PROCNUM\_OUT\_OF\_RANGE | The procedure number is out of range. |
| 0xC002002F<br>RPC\_NT\_BINDING\_HAS\_NO\_AUTH | The binding does not contain any authentication information. |
| 0xC0020030<br>RPC\_NT\_UNKNOWN\_AUTHN\_SERVICE | The authentication service is unknown. |
| 0xC0020031<br>RPC\_NT\_UNKNOWN\_AUTHN\_LEVEL | The authentication level is unknown. |
| 0xC0020032<br>RPC\_NT\_INVALID\_AUTH\_IDENTITY | The security context is invalid. |
| 0xC0020033<br>RPC\_NT\_UNKNOWN\_AUTHZ\_SERVICE | The authorization service is unknown. |
| 0xC0020034<br>EPT\_NT\_INVALID\_ENTRY | The entry is invalid. |
| 0xC0020035<br>EPT\_NT\_CANT\_PERFORM\_OP | The operation cannot be performed. |
| 0xC0020036<br>EPT\_NT\_NOT\_REGISTERED | No more endpoints are available from the endpoint mapper. |
| 0xC0020037<br>RPC\_NT\_NOTHING\_TO\_EXPORT | No interfaces have been exported. |
| 0xC0020038<br>RPC\_NT\_INCOMPLETE\_NAME | The entry name is incomplete. |
| 0xC0020039<br>RPC\_NT\_INVALID\_VERS\_OPTION | The version option is invalid. |
| 0xC002003A<br>RPC\_NT\_NO\_MORE\_MEMBERS | There are no more members. |
| 0xC002003B<br>RPC\_NT\_NOT\_ALL\_OBJS\_UNEXPORTED | There is nothing to unexport. |
| 0xC002003C<br>RPC\_NT\_INTERFACE\_NOT\_FOUND | The interface was not found. |
| 0xC002003D<br>RPC\_NT\_ENTRY\_ALREADY\_EXISTS | The entry already exists. |
| 0xC002003E<br>RPC\_NT\_ENTRY\_NOT\_FOUND | The entry was not found. |
| 0xC002003F<br>RPC\_NT\_NAME\_SERVICE\_UNAVAILABLE | The name service is unavailable. |
| 0xC0020040<br>RPC\_NT\_INVALID\_NAF\_ID | The network address family is invalid. |
| 0xC0020041<br>RPC\_NT\_CANNOT\_SUPPORT | The requested operation is not supported. |
| 0xC0020042<br>RPC\_NT\_NO\_CONTEXT\_AVAILABLE | No security context is available to allow impersonation. |
| 0xC0020043<br>RPC\_NT\_INTERNAL\_ERROR | An internal error occurred in the RPC. |
| 0xC0020044<br>RPC\_NT\_ZERO\_DIVIDE | The RPC server attempted to divide an integer by zero. |
| 0xC0020045<br>RPC\_NT\_ADDRESS\_ERROR | An addressing error occurred in the RPC server. |
| 0xC0020046<br>RPC\_NT\_FP\_DIV\_ZERO | A floating point operation at the RPC server caused a divide by zero. |
| 0xC0020047<br>RPC\_NT\_FP\_UNDERFLOW | A floating point underflow occurred at the RPC server. |
| 0xC0020048<br>RPC\_NT\_FP\_OVERFLOW | A floating point overflow occurred at the RPC server. |
| 0xC0020049<br>RPC\_NT\_CALL\_IN\_PROGRESS | An RPC is already in progress for this thread. |
| 0xC002004A<br>RPC\_NT\_NO\_MORE\_BINDINGS | There are no more bindings. |
| 0xC002004B<br>RPC\_NT\_GROUP\_MEMBER\_NOT\_FOUND | The group member was not found. |
| 0xC002004C<br>EPT\_NT\_CANT\_CREATE | The endpoint mapper database entry could not be created. |
| 0xC002004D<br>RPC\_NT\_INVALID\_OBJECT | The object UUID is the nil UUID. |
| 0xC002004F<br>RPC\_NT\_NO\_INTERFACES | No interfaces have been registered. |
| 0xC0020050<br>RPC\_NT\_CALL\_CANCELLED | The RPC was canceled. |
| 0xC0020051<br>RPC\_NT\_BINDING\_INCOMPLETE | The binding handle does not contain all the required information. |
| 0xC0020052<br>RPC\_NT\_COMM\_FAILURE | A communications failure occurred during an RPC. |
| 0xC0020053<br>RPC\_NT\_UNSUPPORTED\_AUTHN\_LEVEL | The requested authentication level is not supported. |
| 0xC0020054<br>RPC\_NT\_NO\_PRINC\_NAME | No principal name was registered. |
| 0xC0020055<br>RPC\_NT\_NOT\_RPC\_ERROR | The error specified is not a valid Windows RPC error code. |
| 0xC0020057<br>RPC\_NT\_SEC\_PKG\_ERROR | A security package-specific error occurred. |
| 0xC0020058<br>RPC\_NT\_NOT\_CANCELLED | The thread was not canceled. |
| 0xC0020062<br>RPC\_NT\_INVALID\_ASYNC\_HANDLE | Invalid asynchronous RPC handle. |
| 0xC0020063<br>RPC\_NT\_INVALID\_ASYNC\_CALL | Invalid asynchronous RPC call handle for this operation. |
| 0xC0020064<br>RPC\_NT\_PROXY\_ACCESS\_DENIED | Access to the HTTP proxy is denied. |
| 0xC0030001<br>RPC\_NT\_NO\_MORE\_ENTRIES | The list of RPC servers available for auto-handle binding has been exhausted. |
| 0xC0030002<br>RPC\_NT\_SS\_CHAR\_TRANS\_OPEN\_FAIL | The file designated by DCERPCCHARTRANS cannot be opened. |
| 0xC0030003<br>RPC\_NT\_SS\_CHAR\_TRANS\_SHORT\_FILE | The file containing the character translation table has fewer than 512 bytes. |
| 0xC0030004<br>RPC\_NT\_SS\_IN\_NULL\_CONTEXT | A null context handle is passed as an [in] parameter. |
| 0xC0030005<br>RPC\_NT\_SS\_CONTEXT\_MISMATCH | The context handle does not match any known context handles. |
| 0xC0030006<br>RPC\_NT\_SS\_CONTEXT\_DAMAGED | The context handle changed during a call. |
| 0xC0030007<br>RPC\_NT\_SS\_HANDLES\_MISMATCH | The binding handles passed to an RPC do not match. |
| 0xC0030008<br>RPC\_NT\_SS\_CANNOT\_GET\_CALL\_HANDLE | The stub is unable to get the call handle. |
| 0xC0030009<br>RPC\_NT\_NULL\_REF\_POINTER | A null reference pointer was passed to the stub. |
| 0xC003000A<br>RPC\_NT\_ENUM\_VALUE\_OUT\_OF\_RANGE | The enumeration value is out of range. |
| 0xC003000B<br>RPC\_NT\_BYTE\_COUNT\_TOO\_SMALL | The byte count is too small. |
| 0xC003000C<br>RPC\_NT\_BAD\_STUB\_DATA | The stub received bad data. |
| 0xC0030059<br>RPC\_NT\_INVALID\_ES\_ACTION | Invalid operation on the encoding/decoding handle. |
| 0xC003005A<br>RPC\_NT\_WRONG\_ES\_VERSION | Incompatible version of the serializing package. |
| 0xC003005B<br>RPC\_NT\_WRONG\_STUB\_VERSION | Incompatible version of the RPC stub. |
| 0xC003005C<br>RPC\_NT\_INVALID\_PIPE\_OBJECT | The RPC pipe object is invalid or corrupt. |
| 0xC003005D<br>RPC\_NT\_INVALID\_PIPE\_OPERATION | An invalid operation was attempted on an RPC pipe object. |
| 0xC003005E<br>RPC\_NT\_WRONG\_PIPE\_VERSION | Unsupported RPC pipe version. |
| 0xC003005F<br>RPC\_NT\_PIPE\_CLOSED | The RPC pipe object has already been closed. |
| 0xC0030060<br>RPC\_NT\_PIPE\_DISCIPLINE\_ERROR | The RPC call completed before all pipes were processed. |
| 0xC0030061<br>RPC\_NT\_PIPE\_EMPTY | No more data is available from the RPC pipe. |
| 0xC0040035<br>STATUS\_PNP\_BAD\_MPS\_TABLE | A device is missing in the system BIOS MPS table. This device will not be used. Contact your system vendor for a system BIOS update. |
| 0xC0040036<br>STATUS\_PNP\_TRANSLATION\_FAILED | A translator failed to translate resources. |
| 0xC0040037<br>STATUS\_PNP\_IRQ\_TRANSLATION\_FAILED | An IRQ translator failed to translate resources. |
| 0xC0040038<br>STATUS\_PNP\_INVALID\_ID | Driver %2 returned an invalid ID for a child device (%3). |
| 0xC0040039<br>STATUS\_IO\_REISSUE\_AS\_CACHED | Reissue the given operation as a cached I/O operation |
| 0xC00A0001<br>STATUS\_CTX\_WINSTATION\_NAME\_INVALID | Session name %1 is invalid. |
| 0xC00A0002<br>STATUS\_CTX\_INVALID\_PD | The protocol driver %1 is invalid. |
| 0xC00A0003<br>STATUS\_CTX\_PD\_NOT\_FOUND | The protocol driver %1 was not found in the system path. |
| 0xC00A0006<br>STATUS\_CTX\_CLOSE\_PENDING | A close operation is pending on the terminal connection. |
| 0xC00A0007<br>STATUS\_CTX\_NO\_OUTBUF | No free output buffers are available. |
| 0xC00A0008<br>STATUS\_CTX\_MODEM\_INF\_NOT\_FOUND | The MODEM.INF file was not found. |
| 0xC00A0009<br>STATUS\_CTX\_INVALID\_MODEMNAME | The modem (%1) was not found in the MODEM.INF file. |
| 0xC00A000A<br>STATUS\_CTX\_RESPONSE\_ERROR | The modem did not accept the command sent to it. Verify that the configured modem name matches the attached modem. |
| 0xC00A000B<br>STATUS\_CTX\_MODEM\_RESPONSE\_TIMEOUT | The modem did not respond to the command sent to it. Verify that the modem cable is properly attached and the modem is turned on. |
| 0xC00A000C<br>STATUS\_CTX\_MODEM\_RESPONSE\_NO\_CARRIER | Carrier detection has failed or the carrier has been dropped due to disconnection. |
| 0xC00A000D<br>STATUS\_CTX\_MODEM\_RESPONSE\_NO\_DIALTONE | A dial tone was not detected within the required time. Verify that the phone cable is properly attached and functional. |
| 0xC00A000E<br>STATUS\_CTX\_MODEM\_RESPONSE\_BUSY | A busy signal was detected at a remote site on callback. |
| 0xC00A000F<br>STATUS\_CTX\_MODEM\_RESPONSE\_VOICE | A voice was detected at a remote site on callback. |
| 0xC00A0010<br>STATUS\_CTX\_TD\_ERROR | Transport driver error. |
| 0xC00A0012<br>STATUS\_CTX\_LICENSE\_CLIENT\_INVALID | The client you are using is not licensed to use this system. Your logon request is denied. |
| 0xC00A0013<br>STATUS\_CTX\_LICENSE\_NOT\_AVAILABLE | The system has reached its licensed logon limit. Try again later. |
| 0xC00A0014<br>STATUS\_CTX\_LICENSE\_EXPIRED | The system license has expired. Your logon request is denied. |
| 0xC00A0015<br>STATUS\_CTX\_WINSTATION\_NOT\_FOUND | The specified session cannot be found. |
| 0xC00A0016<br>STATUS\_CTX\_WINSTATION\_NAME\_COLLISION | The specified session name is already in use. |
| 0xC00A0017<br>STATUS\_CTX\_WINSTATION\_BUSY | The requested operation cannot be completed because the terminal connection is currently processing a connect, disconnect, reset, or delete operation. |
| 0xC00A0018<br>STATUS\_CTX\_BAD\_VIDEO\_MODE | An attempt has been made to connect to a session whose video mode is not supported by the current client. |
| 0xC00A0022<br>STATUS\_CTX\_GRAPHICS\_INVALID | The application attempted to enable DOS graphics mode. DOS graphics mode is not supported. |
| 0xC00A0024<br>STATUS\_CTX\_NOT\_CONSOLE | The requested operation can be performed only on the system console. This is most often the result of a driver or system DLL requiring direct console access. |
| 0xC00A0026<br>STATUS\_CTX\_CLIENT\_QUERY\_TIMEOUT | The client failed to respond to the server connect message. |
| 0xC00A0027<br>STATUS\_CTX\_CONSOLE\_DISCONNECT | Disconnecting the console session is not supported. |
| 0xC00A0028<br>STATUS\_CTX\_CONSOLE\_CONNECT | Reconnecting a disconnected session to the console is not supported. |
| 0xC00A002A<br>STATUS\_CTX\_SHADOW\_DENIED | The request to control another session remotely was denied. |
| 0xC00A002B<br>STATUS\_CTX\_WINSTATION\_ACCESS\_DENIED | A process has requested access to a session, but has not been granted those access rights. |
| 0xC00A002E<br>STATUS\_CTX\_INVALID\_WD | The terminal connection driver %1 is invalid. |
| 0xC00A002F<br>STATUS\_CTX\_WD\_NOT\_FOUND | The terminal connection driver %1 was not found in the system path. |
| 0xC00A0030<br>STATUS\_CTX\_SHADOW\_INVALID | The requested session cannot be controlled remotely. You cannot control your own session, a session that is trying to control your session, a session that has no user logged on, or other sessions from the console. |
| 0xC00A0031<br>STATUS\_CTX\_SHADOW\_DISABLED | The requested session is not configured to allow remote control. |
| 0xC00A0032<br>STATUS\_RDP\_PROTOCOL\_ERROR | The RDP protocol component %2 detected an error in the protocol stream and has disconnected the client. |
| 0xC00A0033<br>STATUS\_CTX\_CLIENT\_LICENSE\_NOT\_SET | Your request to connect to this [terminal server](6b46e050-0761-44b1-858b-9b37a74ca32e#gt_b416f72e-cf04-4d80-bf93-f5753f3b0998) has been rejected. Your terminal server client license number has not been entered for this copy of the terminal client. Contact your system administrator for help in entering a valid, unique license number for this terminal server client. Click OK to continue. |
| 0xC00A0034<br>STATUS\_CTX\_CLIENT\_LICENSE\_IN\_USE | Your request to connect to this terminal server has been rejected. Your terminal server client license number is currently being used by another user. Contact your system administrator to obtain a new copy of the terminal server client with a valid, unique license number. Click OK to continue. |
| 0xC00A0035<br>STATUS\_CTX\_SHADOW\_ENDED\_BY\_MODE\_CHANGE | The remote control of the console was terminated because the display mode was changed. Changing the display mode in a remote control session is not supported. |
| 0xC00A0036<br>STATUS\_CTX\_SHADOW\_NOT\_RUNNING | Remote control could not be terminated because the specified session is not currently being remotely controlled. |
| 0xC00A0037<br>STATUS\_CTX\_LOGON\_DISABLED | Your interactive logon privilege has been disabled. Contact your system administrator. |
| 0xC00A0038<br>STATUS\_CTX\_SECURITY\_LAYER\_ERROR | The terminal server security layer detected an error in the protocol stream and has disconnected the client. |
| 0xC00A0039<br>STATUS\_TS\_INCOMPATIBLE\_SESSIONS | The target session is incompatible with the current session. |
| 0xC00B0001<br>STATUS\_MUI\_FILE\_NOT\_FOUND | The resource loader failed to find an MUI file. |
| 0xC00B0002<br>STATUS\_MUI\_INVALID\_FILE | The resource loader failed to load an MUI file because the file failed to pass validation. |
| 0xC00B0003<br>STATUS\_MUI\_INVALID\_RC\_CONFIG | The RC manifest is corrupted with garbage data, is an unsupported version, or is missing a required item. |
| 0xC00B0004<br>STATUS\_MUI\_INVALID\_LOCALE\_NAME | The RC manifest has an invalid culture name. |
| 0xC00B0005<br>STATUS\_MUI\_INVALID\_ULTIMATEFALLBACK\_NAME | The RC manifest has and invalid ultimate fallback name. |
| 0xC00B0006<br>STATUS\_MUI\_FILE\_NOT\_LOADED | The resource loader cache does not have a loaded MUI entry. |
| 0xC00B0007<br>STATUS\_RESOURCE\_ENUM\_USER\_STOP | The user stopped resource enumeration. |
| 0xC0130001<br>STATUS\_CLUSTER\_INVALID\_NODE | The cluster node is not valid. |
| 0xC0130002<br>STATUS\_CLUSTER\_NODE\_EXISTS | The cluster node already exists. |
| 0xC0130003<br>STATUS\_CLUSTER\_JOIN\_IN\_PROGRESS | A node is in the process of joining the cluster. |
| 0xC0130004<br>STATUS\_CLUSTER\_NODE\_NOT\_FOUND | The cluster node was not found. |
| 0xC0130005<br>STATUS\_CLUSTER\_LOCAL\_NODE\_NOT\_FOUND | The cluster local node information was not found. |
| 0xC0130006<br>STATUS\_CLUSTER\_NETWORK\_EXISTS | The cluster network already exists. |
| 0xC0130007<br>STATUS\_CLUSTER\_NETWORK\_NOT\_FOUND | The cluster network was not found. |
| 0xC0130008<br>STATUS\_CLUSTER\_NETINTERFACE\_EXISTS | The cluster network interface already exists. |
| 0xC0130009<br>STATUS\_CLUSTER\_NETINTERFACE\_NOT\_FOUND | The cluster network interface was not found. |
| 0xC013000A<br>STATUS\_CLUSTER\_INVALID\_REQUEST | The cluster request is not valid for this object. |
| 0xC013000B<br>STATUS\_CLUSTER\_INVALID\_NETWORK\_PROVIDER | The cluster network provider is not valid. |
| 0xC013000C<br>STATUS\_CLUSTER\_NODE\_DOWN | The cluster node is down. |
| 0xC013000D<br>STATUS\_CLUSTER\_NODE\_UNREACHABLE | The cluster node is not reachable. |
| 0xC013000E<br>STATUS\_CLUSTER\_NODE\_NOT\_MEMBER | The cluster node is not a member of the cluster. |
| 0xC013000F<br>STATUS\_CLUSTER\_JOIN\_NOT\_IN\_PROGRESS | A cluster join operation is not in progress. |
| 0xC0130010<br>STATUS\_CLUSTER\_INVALID\_NETWORK | The cluster network is not valid. |
| 0xC0130011<br>STATUS\_CLUSTER\_NO\_NET\_ADAPTERS | No network adapters are available. |
| 0xC0130012<br>STATUS\_CLUSTER\_NODE\_UP | The cluster node is up. |
| 0xC0130013<br>STATUS\_CLUSTER\_NODE\_PAUSED | The cluster node is paused. |
| 0xC0130014<br>STATUS\_CLUSTER\_NODE\_NOT\_PAUSED | The cluster node is not paused. |
| 0xC0130015<br>STATUS\_CLUSTER\_NO\_SECURITY\_CONTEXT | No cluster security context is available. |
| 0xC0130016<br>STATUS\_CLUSTER\_NETWORK\_NOT\_INTERNAL | The cluster network is not configured for internal cluster communication. |
| 0xC0130017<br>STATUS\_CLUSTER\_POISONED | The cluster node has been poisoned. |
| 0xC0140001<br>STATUS\_ACPI\_INVALID\_OPCODE | An attempt was made to run an invalid AML opcode. |
| 0xC0140002<br>STATUS\_ACPI\_STACK\_OVERFLOW | The AML interpreter stack has overflowed. |
| 0xC0140003<br>STATUS\_ACPI\_ASSERT\_FAILED | An inconsistent state has occurred. |
| 0xC0140004<br>STATUS\_ACPI\_INVALID\_INDEX | An attempt was made to access an array outside its bounds. |
| 0xC0140005<br>STATUS\_ACPI\_INVALID\_ARGUMENT | A required argument was not specified. |
| 0xC0140006<br>STATUS\_ACPI\_FATAL | A fatal error has occurred. |
| 0xC0140007<br>STATUS\_ACPI\_INVALID\_SUPERNAME | An invalid SuperName was specified. |
| 0xC0140008<br>STATUS\_ACPI\_INVALID\_ARGTYPE | An argument with an incorrect type was specified. |
| 0xC0140009<br>STATUS\_ACPI\_INVALID\_OBJTYPE | An object with an incorrect type was specified. |
| 0xC014000A<br>STATUS\_ACPI\_INVALID\_TARGETTYPE | A target with an incorrect type was specified. |
| 0xC014000B<br>STATUS\_ACPI\_INCORRECT\_ARGUMENT\_COUNT | An incorrect number of arguments was specified. |
| 0xC014000C<br>STATUS\_ACPI\_ADDRESS\_NOT\_MAPPED | An address failed to translate. |
| 0xC014000D<br>STATUS\_ACPI\_INVALID\_EVENTTYPE | An incorrect event type was specified. |
| 0xC014000E<br>STATUS\_ACPI\_HANDLER\_COLLISION | A handler for the target already exists. |
| 0xC014000F<br>STATUS\_ACPI\_INVALID\_DATA | Invalid data for the target was specified. |
| 0xC0140010<br>STATUS\_ACPI\_INVALID\_REGION | An invalid region for the target was specified. |
| 0xC0140011<br>STATUS\_ACPI\_INVALID\_ACCESS\_SIZE | An attempt was made to access a field outside the defined range. |
| 0xC0140012<br>STATUS\_ACPI\_ACQUIRE\_GLOBAL\_LOCK | The global system lock could not be acquired. |
| 0xC0140013<br>STATUS\_ACPI\_ALREADY\_INITIALIZED | An attempt was made to reinitialize the ACPI subsystem. |
| 0xC0140014<br>STATUS\_ACPI\_NOT\_INITIALIZED | The ACPI subsystem has not been initialized. |
| 0xC0140015<br>STATUS\_ACPI\_INVALID\_MUTEX\_LEVEL | An incorrect mutex was specified. |
| 0xC0140016<br>STATUS\_ACPI\_MUTEX\_NOT\_OWNED | The mutex is not currently owned. |
| 0xC0140017<br>STATUS\_ACPI\_MUTEX\_NOT\_OWNER | An attempt was made to access the mutex by a process that was not the owner. |
| 0xC0140018<br>STATUS\_ACPI\_RS\_ACCESS | An error occurred during an access to region space. |
| 0xC0140019<br>STATUS\_ACPI\_INVALID\_TABLE | An attempt was made to use an incorrect table. |
| 0xC0140020<br>STATUS\_ACPI\_REG\_HANDLER\_FAILED | The registration of an ACPI event failed. |
| 0xC0140021<br>STATUS\_ACPI\_POWER\_REQUEST\_FAILED | An ACPI power object failed to transition state. |
| 0xC0150001<br>STATUS\_SXS\_SECTION\_NOT\_FOUND | The requested section is not present in the activation context. |
| 0xC0150002<br>STATUS\_SXS\_CANT\_GEN\_ACTCTX | Windows was unble to process the application binding information. Refer to the system event log for further information. |
| 0xC0150003<br>STATUS\_SXS\_INVALID\_ACTCTXDATA\_FORMAT | The application binding data format is invalid. |
| 0xC0150004<br>STATUS\_SXS\_ASSEMBLY\_NOT\_FOUND | The referenced assembly is not installed on the system. |
| 0xC0150005<br>STATUS\_SXS\_MANIFEST\_FORMAT\_ERROR | The manifest file does not begin with the required tag and format information. |
| 0xC0150006<br>STATUS\_SXS\_MANIFEST\_PARSE\_ERROR | The manifest file contains one or more syntax errors. |
| 0xC0150007<br>STATUS\_SXS\_ACTIVATION\_CONTEXT\_DISABLED | The application attempted to activate a disabled activation context. |
| 0xC0150008<br>STATUS\_SXS\_KEY\_NOT\_FOUND | The requested lookup key was not found in any active activation context. |
| 0xC0150009<br>STATUS\_SXS\_VERSION\_CONFLICT | A component version required by the application conflicts with another component version that is already active. |
| 0xC015000A<br>STATUS\_SXS\_WRONG\_SECTION\_TYPE | The type requested activation context section does not match the query API used. |
| 0xC015000B<br>STATUS\_SXS\_THREAD\_QUERIES\_DISABLED | Lack of system resources has required isolated activation to be disabled for the current thread of execution. |
| 0xC015000C<br>STATUS\_SXS\_ASSEMBLY\_MISSING | The referenced assembly could not be found. |
| 0xC015000E<br>STATUS\_SXS\_PROCESS\_DEFAULT\_ALREADY\_SET | An attempt to set the process default activation context failed because the process default activation context was already set. |
| 0xC015000F<br>STATUS\_SXS\_EARLY\_DEACTIVATION | The activation context being deactivated is not the most recently activated one. |
| 0xC0150010<br>STATUS\_SXS\_INVALID\_DEACTIVATION | The activation context being deactivated is not active for the current thread of execution. |
| 0xC0150011<br>STATUS\_SXS\_MULTIPLE\_DEACTIVATION | The activation context being deactivated has already been deactivated. |
| 0xC0150012<br>STATUS\_SXS\_SYSTEM\_DEFAULT\_ACTIVATION\_CONTEXT\_EMPTY | The activation context of the system default assembly could not be generated. |
| 0xC0150013<br>STATUS\_SXS\_PROCESS\_TERMINATION\_REQUESTED | A component used by the isolation facility has requested that the process be terminated. |
| 0xC0150014<br>STATUS\_SXS\_CORRUPT\_ACTIVATION\_STACK | The activation context activation stack for the running thread of execution is corrupt. |
| 0xC0150015<br>STATUS\_SXS\_CORRUPTION | The application isolation metadata for this process or thread has become corrupt. |
| 0xC0150016<br>STATUS\_SXS\_INVALID\_IDENTITY\_ATTRIBUTE\_VALUE | The value of an attribute in an identity is not within the legal range. |
| 0xC0150017<br>STATUS\_SXS\_INVALID\_IDENTITY\_ATTRIBUTE\_NAME | The name of an attribute in an identity is not within the legal range. |
| 0xC0150018<br>STATUS\_SXS\_IDENTITY\_DUPLICATE\_ATTRIBUTE | An identity contains two definitions for the same attribute. |
| 0xC0150019<br>STATUS\_SXS\_IDENTITY\_PARSE\_ERROR | The identity string is malformed. This might be due to a trailing comma, more than two unnamed attributes, a missing attribute name, or a missing attribute value. |
| 0xC015001A<br>STATUS\_SXS\_COMPONENT\_STORE\_CORRUPT | The component store has become corrupted. |
| 0xC015001B<br>STATUS\_SXS\_FILE\_HASH\_MISMATCH | A component's file does not match the verification information present in the component manifest. |
| 0xC015001C<br>STATUS\_SXS\_MANIFEST\_IDENTITY\_SAME\_BUT\_CONTENTS\_DIFFERENT | The identities of the manifests are identical, but their contents are different. |
| 0xC015001D<br>STATUS\_SXS\_IDENTITIES\_DIFFERENT | The component identities are different. |
| 0xC015001E<br>STATUS\_SXS\_ASSEMBLY\_IS\_NOT\_A\_DEPLOYMENT | The assembly is not a deployment. |
| 0xC015001F<br>STATUS\_SXS\_FILE\_NOT\_PART\_OF\_ASSEMBLY | The file is not a part of the assembly. |
| 0xC0150020<br>STATUS\_ADVANCED\_INSTALLER\_FAILED | An advanced installer failed during setup or servicing. |
| 0xC0150021<br>STATUS\_XML\_ENCODING\_MISMATCH | The character encoding in the XML declaration did not match the encoding used in the document. |
| 0xC0150022<br>STATUS\_SXS\_MANIFEST\_TOO\_BIG | The size of the manifest exceeds the maximum allowed. |
| 0xC0150023<br>STATUS\_SXS\_SETTING\_NOT\_REGISTERED | The setting is not registered. |
| 0xC0150024<br>STATUS\_SXS\_TRANSACTION\_CLOSURE\_INCOMPLETE | One or more required transaction members are not present. |
| 0xC0150025<br>STATUS\_SMI\_PRIMITIVE\_INSTALLER\_FAILED | The SMI primitive installer failed during setup or servicing. |
| 0xC0150026<br>STATUS\_GENERIC\_COMMAND\_FAILED | A generic command executable returned a result that indicates failure. |
| 0xC0150027<br>STATUS\_SXS\_FILE\_HASH\_MISSING | A component is missing file verification information in its manifest. |
| 0xC0190001<br>STATUS\_TRANSACTIONAL\_CONFLICT | The function attempted to use a name that is reserved for use by another transaction. |
| 0xC0190002<br>STATUS\_INVALID\_TRANSACTION | The transaction handle associated with this operation is invalid. |
| 0xC0190003<br>STATUS\_TRANSACTION\_NOT\_ACTIVE | The requested operation was made in the context of a transaction that is no longer active. |
| 0xC0190004<br>STATUS\_TM\_INITIALIZATION\_FAILED | The transaction manager was unable to be successfully initialized. Transacted operations are not supported. |
| 0xC0190005<br>STATUS\_RM\_NOT\_ACTIVE | Transaction support within the specified file system resource manager was not started or was shut down due to an error. |
| 0xC0190006<br>STATUS\_RM\_METADATA\_CORRUPT | The metadata of the resource manager has been corrupted. The resource manager will not function. |
| 0xC0190007<br>STATUS\_TRANSACTION\_NOT\_JOINED | The resource manager attempted to prepare a transaction that it has not successfully joined. |
| 0xC0190008<br>STATUS\_DIRECTORY\_NOT\_RM | The specified directory does not contain a file system resource manager. |
| 0xC019000A<br>STATUS\_TRANSACTIONS\_UNSUPPORTED\_REMOTE | The remote server or share does not support transacted file operations. |
| 0xC019000B<br>STATUS\_LOG\_RESIZE\_INVALID\_SIZE | The requested log size for the file system resource manager is invalid. |
| 0xC019000C<br>STATUS\_REMOTE\_FILE\_VERSION\_MISMATCH | The remote server sent mismatching version number or Fid for a file opened with transactions. |
| 0xC019000F<br>STATUS\_CRM\_PROTOCOL\_ALREADY\_EXISTS | The resource manager tried to register a protocol that already exists. |
| 0xC0190010<br>STATUS\_TRANSACTION\_PROPAGATION\_FAILED | The attempt to propagate the transaction failed. |
| 0xC0190011<br>STATUS\_CRM\_PROTOCOL\_NOT\_FOUND | The requested propagation protocol was not registered as a CRM. |
| 0xC0190012<br>STATUS\_TRANSACTION\_SUPERIOR\_EXISTS | The transaction object already has a superior enlistment, and the caller attempted an operation that would have created a new superior. Only a single superior enlistment is allowed. |
| 0xC0190013<br>STATUS\_TRANSACTION\_REQUEST\_NOT\_VALID | The requested operation is not valid on the transaction object in its current state. |
| 0xC0190014<br>STATUS\_TRANSACTION\_NOT\_REQUESTED | The caller has called a response API, but the response is not expected because the transaction manager did not issue the corresponding request to the caller. |
| 0xC0190015<br>STATUS\_TRANSACTION\_ALREADY\_ABORTED | It is too late to perform the requested operation, because the transaction has already been aborted. |
| 0xC0190016<br>STATUS\_TRANSACTION\_ALREADY\_COMMITTED | It is too late to perform the requested operation, because the transaction has already been committed. |
| 0xC0190017<br>STATUS\_TRANSACTION\_INVALID\_MARSHALL\_BUFFER | The buffer passed in to NtPushTransaction or NtPullTransaction is not in a valid format. |
| 0xC0190018<br>STATUS\_CURRENT\_TRANSACTION\_NOT\_VALID | The current transaction context associated with the thread is not a valid handle to a transaction object. |
| 0xC0190019<br>STATUS\_LOG\_GROWTH\_FAILED | An attempt to create space in the transactional resource manager's log failed. The failure status has been recorded in the event log. |
| 0xC0190021<br>STATUS\_OBJECT\_NO\_LONGER\_EXISTS | The object (file, stream, or link) that corresponds to the handle has been deleted by a transaction savepoint rollback. |
| 0xC0190022<br>STATUS\_STREAM\_MINIVERSION\_NOT\_FOUND | The specified file miniversion was not found for this transacted file open. |
| 0xC0190023<br>STATUS\_STREAM\_MINIVERSION\_NOT\_VALID | The specified file miniversion was found but has been invalidated. The most likely cause is a transaction savepoint rollback. |
| 0xC0190024<br>STATUS\_MINIVERSION\_INACCESSIBLE\_FROM\_SPECIFIED\_TRANSACTION | A miniversion can be opened only in the context of the transaction that created it. |
| 0xC0190025<br>STATUS\_CANT\_OPEN\_MINIVERSION\_WITH\_MODIFY\_INTENT | It is not possible to open a miniversion with modify access. |
| 0xC0190026<br>STATUS\_CANT\_CREATE\_MORE\_STREAM\_MINIVERSIONS | It is not possible to create any more miniversions for this stream. |
| 0xC0190028<br>STATUS\_HANDLE\_NO\_LONGER\_VALID | The handle has been invalidated by a transaction. The most likely cause is the presence of memory mapping on a file or an open handle when the transaction ended or rolled back to savepoint. |
| 0xC0190030<br>STATUS\_LOG\_CORRUPTION\_DETECTED | The log data is corrupt. |
| 0xC0190032<br>STATUS\_RM\_DISCONNECTED | The transaction outcome is unavailable because the resource manager responsible for it is disconnected. |
| 0xC0190033<br>STATUS\_ENLISTMENT\_NOT\_SUPERIOR | The request was rejected because the enlistment in question is not a superior enlistment. |
| 0xC0190036<br>STATUS\_FILE\_IDENTITY\_NOT\_PERSISTENT | The file cannot be opened in a transaction because its identity depends on the outcome of an unresolved transaction. |
| 0xC0190037<br>STATUS\_CANT\_BREAK\_TRANSACTIONAL\_DEPENDENCY | The operation cannot be performed because another transaction is depending on this property not changing. |
| 0xC0190038<br>STATUS\_CANT\_CROSS\_RM\_BOUNDARY | The operation would involve a single file with two transactional resource managers and is, therefore, not allowed. |
| 0xC0190039<br>STATUS\_TXF\_DIR\_NOT\_EMPTY | The $Txf directory must be empty for this operation to succeed. |
| 0xC019003A<br>STATUS\_INDOUBT\_TRANSACTIONS\_EXIST | The operation would leave a transactional resource manager in an inconsistent state and is therefore not allowed. |
| 0xC019003B<br>STATUS\_TM\_VOLATILE | The operation could not be completed because the transaction manager does not have a log. |
| 0xC019003C<br>STATUS\_ROLLBACK\_TIMER\_EXPIRED | A rollback could not be scheduled because a previously scheduled rollback has already executed or been queued for execution. |
| 0xC019003D<br>STATUS\_TXF\_ATTRIBUTE\_CORRUPT | The transactional metadata attribute on the file or directory %hs is corrupt and unreadable. |
| 0xC019003E<br>STATUS\_EFS\_NOT\_ALLOWED\_IN\_TRANSACTION | The encryption operation could not be completed because a transaction is active. |
| 0xC019003F<br>STATUS\_TRANSACTIONAL\_OPEN\_NOT\_ALLOWED | This object is not allowed to be opened in a transaction. |
| 0xC0190040<br>STATUS\_TRANSACTED\_MAPPING\_UNSUPPORTED\_REMOTE | Memory mapping (creating a mapped section) a remote file under a transaction is not supported. |
| 0xC0190043<br>STATUS\_TRANSACTION\_REQUIRED\_PROMOTION | Promotion was required to allow the resource manager to enlist, but the transaction was set to disallow it. |
| 0xC0190044<br>STATUS\_CANNOT\_EXECUTE\_FILE\_IN\_TRANSACTION | This file is open for modification in an unresolved transaction and can be opened for execute only by a transacted reader. |
| 0xC0190045<br>STATUS\_TRANSACTIONS\_NOT\_FROZEN | The request to thaw frozen transactions was ignored because transactions were not previously frozen. |
| 0xC0190046<br>STATUS\_TRANSACTION\_FREEZE\_IN\_PROGRESS | Transactions cannot be frozen because a freeze is already in progress. |
| 0xC0190047<br>STATUS\_NOT\_SNAPSHOT\_VOLUME | The target volume is not a snapshot volume. This operation is valid only on a volume mounted as a snapshot. |
| 0xC0190048<br>STATUS\_NO\_SAVEPOINT\_WITH\_OPEN\_FILES | The savepoint operation failed because files are open on the transaction, which is not permitted. |
| 0xC0190049<br>STATUS\_SPARSE\_NOT\_ALLOWED\_IN\_TRANSACTION | The sparse operation could not be completed because a transaction is active on the file. |
| 0xC019004A<br>STATUS\_TM\_IDENTITY\_MISMATCH | The call to create a transaction manager object failed because the Tm Identity that is stored in the log file does not match the Tm Identity that was passed in as an argument. |
| 0xC019004B<br>STATUS\_FLOATED\_SECTION | I/O was attempted on a section object that has been floated as a result of a transaction ending. There is no valid data. |
| 0xC019004C<br>STATUS\_CANNOT\_ACCEPT\_TRANSACTED\_WORK | The transactional resource manager cannot currently accept transacted work due to a transient condition, such as low resources. |
| 0xC019004D<br>STATUS\_CANNOT\_ABORT\_TRANSACTIONS | The transactional resource manager had too many transactions outstanding that could not be aborted. The transactional resource manager has been shut down. |
| 0xC019004E<br>STATUS\_TRANSACTION\_NOT\_FOUND | The specified transaction was unable to be opened because it was not found. |
| 0xC019004F<br>STATUS\_RESOURCEMANAGER\_NOT\_FOUND | The specified resource manager was unable to be opened because it was not found. |
| 0xC0190050<br>STATUS\_ENLISTMENT\_NOT\_FOUND | The specified enlistment was unable to be opened because it was not found. |
| 0xC0190051<br>STATUS\_TRANSACTIONMANAGER\_NOT\_FOUND | The specified transaction manager was unable to be opened because it was not found. |
| 0xC0190052<br>STATUS\_TRANSACTIONMANAGER\_NOT\_ONLINE | The specified resource manager was unable to create an enlistment because its associated transaction manager is not online. |
| 0xC0190053<br>STATUS\_TRANSACTIONMANAGER\_RECOVERY\_NAME\_COLLISION | The specified transaction manager was unable to create the objects contained in its log file in the Ob namespace. Therefore, the transaction manager was unable to recover. |
| 0xC0190054<br>STATUS\_TRANSACTION\_NOT\_ROOT | The call to create a superior enlistment on this transaction object could not be completed because the transaction object specified for the enlistment is a subordinate branch of the transaction. Only the root of the transaction can be enlisted as a superior. |
| 0xC0190055<br>STATUS\_TRANSACTION\_OBJECT\_EXPIRED | Because the associated transaction manager or resource manager has been closed, the handle is no longer valid. |
| 0xC0190056<br>STATUS\_COMPRESSION\_NOT\_ALLOWED\_IN\_TRANSACTION | The compression operation could not be completed because a transaction is active on the file. |
| 0xC0190057<br>STATUS\_TRANSACTION\_RESPONSE\_NOT\_ENLISTED | The specified operation could not be performed on this superior enlistment because the enlistment was not created with the corresponding completion response in the NotificationMask. |
| 0xC0190058<br>STATUS\_TRANSACTION\_RECORD\_TOO\_LONG | The specified operation could not be performed because the record to be logged was too long. This can occur because either there are too many enlistments on this transaction or the combined RecoveryInformation being logged on behalf of those enlistments is too long. |
| 0xC0190059<br>STATUS\_NO\_LINK\_TRACKING\_IN\_TRANSACTION | The link-tracking operation could not be completed because a transaction is active. |
| 0xC019005A<br>STATUS\_OPERATION\_NOT\_SUPPORTED\_IN\_TRANSACTION | This operation cannot be performed in a transaction. |
| 0xC019005B<br>STATUS\_TRANSACTION\_INTEGRITY\_VIOLATED | The kernel transaction manager had to abort or forget the transaction because it blocked forward progress. |
| 0xC0190060<br>STATUS\_EXPIRED\_HANDLE | The handle is no longer properly associated with its transaction. It might have been opened in a transactional resource manager that was subsequently forced to restart. Please close the handle and open a new one. |
| 0xC0190061<br>STATUS\_TRANSACTION\_NOT\_ENLISTED | The specified operation could not be performed because the resource manager is not enlisted in the transaction. |
| 0xC01A0001<br>STATUS\_LOG\_SECTOR\_INVALID | The log service found an invalid log sector. |
| 0xC01A0002<br>STATUS\_LOG\_SECTOR\_PARITY\_INVALID | The log service encountered a log sector with invalid block parity. |
| 0xC01A0003<br>STATUS\_LOG\_SECTOR\_REMAPPED | The log service encountered a remapped log sector. |
| 0xC01A0004<br>STATUS\_LOG\_BLOCK\_INCOMPLETE | The log service encountered a partial or incomplete log block. |
| 0xC01A0005<br>STATUS\_LOG\_INVALID\_RANGE | The log service encountered an attempt to access data outside the active log range. |
| 0xC01A0006<br>STATUS\_LOG\_BLOCKS\_EXHAUSTED | The log service user-log marshaling buffers are exhausted. |
| 0xC01A0007<br>STATUS\_LOG\_READ\_CONTEXT\_INVALID | The log service encountered an attempt to read from a marshaling area with an invalid read context. |
| 0xC01A0008<br>STATUS\_LOG\_RESTART\_INVALID | The log service encountered an invalid log restart area. |
| 0xC01A0009<br>STATUS\_LOG\_BLOCK\_VERSION | The log service encountered an invalid log block version. |
| 0xC01A000A<br>STATUS\_LOG\_BLOCK\_INVALID | The log service encountered an invalid log block. |
| 0xC01A000B<br>STATUS\_LOG\_READ\_MODE\_INVALID | The log service encountered an attempt to read the log with an invalid read mode. |
| 0xC01A000D<br>STATUS\_LOG\_METADATA\_CORRUPT | The log service encountered a corrupted metadata file. |
| 0xC01A000E<br>STATUS\_LOG\_METADATA\_INVALID | The log service encountered a metadata file that could not be created by the log file system. |
| 0xC01A000F<br>STATUS\_LOG\_METADATA\_INCONSISTENT | The log service encountered a metadata file with inconsistent data. |
| 0xC01A0010<br>STATUS\_LOG\_RESERVATION\_INVALID | The log service encountered an attempt to erroneously allocate or dispose reservation space. |
| 0xC01A0011<br>STATUS\_LOG\_CANT\_DELETE | The log service cannot delete the log file or the file system container. |
| 0xC01A0012<br>STATUS\_LOG\_CONTAINER\_LIMIT\_EXCEEDED | The log service has reached the maximum allowable containers allocated to a log file. |
| 0xC01A0013<br>STATUS\_LOG\_START\_OF\_LOG | The log service has attempted to read or write backward past the start of the log. |
| 0xC01A0014<br>STATUS\_LOG\_POLICY\_ALREADY\_INSTALLED | The log policy could not be installed because a policy of the same type is already present. |
| 0xC01A0015<br>STATUS\_LOG\_POLICY\_NOT\_INSTALLED | The log policy in question was not installed at the time of the request. |
| 0xC01A0016<br>STATUS\_LOG\_POLICY\_INVALID | The installed set of policies on the log is invalid. |
| 0xC01A0017<br>STATUS\_LOG\_POLICY\_CONFLICT | A policy on the log in question prevented the operation from completing. |
| 0xC01A0018<br>STATUS\_LOG\_PINNED\_ARCHIVE\_TAIL | The log space cannot be reclaimed because the log is pinned by the archive tail. |
| 0xC01A0019<br>STATUS\_LOG\_RECORD\_NONEXISTENT | The log record is not a record in the log file. |
| 0xC01A001A<br>STATUS\_LOG\_RECORDS\_RESERVED\_INVALID | The number of reserved log records or the adjustment of the number of reserved log records is invalid. |
| 0xC01A001B<br>STATUS\_LOG\_SPACE\_RESERVED\_INVALID | The reserved log space or the adjustment of the log space is invalid. |
| 0xC01A001C<br>STATUS\_LOG\_TAIL\_INVALID | A new or existing archive tail or the base of the active log is invalid. |
| 0xC01A001D<br>STATUS\_LOG\_FULL | The log space is exhausted. |
| 0xC01A001E<br>STATUS\_LOG\_MULTIPLEXED | The log is multiplexed; no direct writes to the physical log are allowed. |
| 0xC01A001F<br>STATUS\_LOG\_DEDICATED | The operation failed because the log is dedicated. |
| 0xC01A0020<br>STATUS\_LOG\_ARCHIVE\_NOT\_IN\_PROGRESS | The operation requires an archive context. |
| 0xC01A0021<br>STATUS\_LOG\_ARCHIVE\_IN\_PROGRESS | Log archival is in progress. |
| 0xC01A0022<br>STATUS\_LOG\_EPHEMERAL | The operation requires a nonephemeral log, but the log is ephemeral. |
| 0xC01A0023<br>STATUS\_LOG\_NOT\_ENOUGH\_CONTAINERS | The log must have at least two containers before it can be read from or written to. |
| 0xC01A0024<br>STATUS\_LOG\_CLIENT\_ALREADY\_REGISTERED | A log client has already registered on the stream. |
| 0xC01A0025<br>STATUS\_LOG\_CLIENT\_NOT\_REGISTERED | A log client has not been registered on the stream. |
| 0xC01A0026<br>STATUS\_LOG\_FULL\_HANDLER\_IN\_PROGRESS | A request has already been made to handle the log full condition. |
| 0xC01A0027<br>STATUS\_LOG\_CONTAINER\_READ\_FAILED | The log service encountered an error when attempting to read from a log container. |
| 0xC01A0028<br>STATUS\_LOG\_CONTAINER\_WRITE\_FAILED | The log service encountered an error when attempting to write to a log container. |
| 0xC01A0029<br>STATUS\_LOG\_CONTAINER\_OPEN\_FAILED | The log service encountered an error when attempting to open a log container. |
| 0xC01A002A<br>STATUS\_LOG\_CONTAINER\_STATE\_INVALID | The log service encountered an invalid container state when attempting a requested action. |
| 0xC01A002B<br>STATUS\_LOG\_STATE\_INVALID | The log service is not in the correct state to perform a requested action. |
| 0xC01A002C<br>STATUS\_LOG\_PINNED | The log space cannot be reclaimed because the log is pinned. |
| 0xC01A002D<br>STATUS\_LOG\_METADATA\_FLUSH\_FAILED | The log metadata flush failed. |
| 0xC01A002E<br>STATUS\_LOG\_INCONSISTENT\_SECURITY | Security on the log and its containers is inconsistent. |
| 0xC01A002F<br>STATUS\_LOG\_APPENDED\_FLUSH\_FAILED | Records were appended to the log or reservation changes were made, but the log could not be flushed. |
| 0xC01A0030<br>STATUS\_LOG\_PINNED\_RESERVATION | The log is pinned due to reservation consuming most of the log space. Free some reserved records to make space available. |
| 0xC01B00EA<br>STATUS\_VIDEO\_HUNG\_DISPLAY\_DRIVER\_THREAD | {Display Driver Stopped Responding} The %hs display driver has stopped working normally. Save your work and reboot the system to restore full display functionality. The next time you reboot the computer, a dialog box will allow you to upload data about this failure to Microsoft. |
| 0xC01C0001<br>STATUS\_FLT\_NO\_HANDLER\_DEFINED | A handler was not defined by the filter for this operation. |
| 0xC01C0002<br>STATUS\_FLT\_CONTEXT\_ALREADY\_DEFINED | A context is already defined for this object. |
| 0xC01C0003<br>STATUS\_FLT\_INVALID\_ASYNCHRONOUS\_REQUEST | Asynchronous requests are not valid for this operation. |
| 0xC01C0004<br>STATUS\_FLT\_DISALLOW\_FAST\_IO | This is an internal error code used by the filter manager to determine if a fast I/O operation should be forced down the input/output request packet (IRP) path. Minifilters should never return this value. |
| 0xC01C0005<br>STATUS\_FLT\_INVALID\_NAME\_REQUEST | An invalid name request was made. The name requested cannot be retrieved at this time. |
| 0xC01C0006<br>STATUS\_FLT\_NOT\_SAFE\_TO\_POST\_OPERATION | Posting this operation to a worker thread for further processing is not safe at this time because it could lead to a system deadlock. |
| 0xC01C0007<br>STATUS\_FLT\_NOT\_INITIALIZED | The Filter Manager was not initialized when a filter tried to register. Make sure that the Filter Manager is loaded as a driver. |
| 0xC01C0008<br>STATUS\_FLT\_FILTER\_NOT\_READY | The filter is not ready for attachment to volumes because it has not finished initializing (FltStartFiltering has not been called). |
| 0xC01C0009<br>STATUS\_FLT\_POST\_OPERATION\_CLEANUP | The filter must clean up any operation-specific context at this time because it is being removed from the system before the operation is completed by the lower drivers. |
| 0xC01C000A<br>STATUS\_FLT\_INTERNAL\_ERROR | The Filter Manager had an internal error from which it cannot recover; therefore, the operation has failed. This is usually the result of a filter returning an invalid value from a pre-operation callback. |
| 0xC01C000B<br>STATUS\_FLT\_DELETING\_OBJECT | The object specified for this action is in the process of being deleted; therefore, the action requested cannot be completed at this time. |
| 0xC01C000C<br>STATUS\_FLT\_MUST\_BE\_NONPAGED\_POOL | A nonpaged pool must be used for this type of context. |
| 0xC01C000D<br>STATUS\_FLT\_DUPLICATE\_ENTRY | A duplicate handler definition has been provided for an operation. |
| 0xC01C000E<br>STATUS\_FLT\_CBDQ\_DISABLED | The callback data queue has been disabled. |
| 0xC01C000F<br>STATUS\_FLT\_DO\_NOT\_ATTACH | Do not attach the filter to the volume at this time. |
| 0xC01C0010<br>STATUS\_FLT\_DO\_NOT\_DETACH | Do not detach the filter from the volume at this time. |
| 0xC01C0011<br>STATUS\_FLT\_INSTANCE\_ALTITUDE\_COLLISION | An instance already exists at this altitude on the volume specified. |
| 0xC01C0012<br>STATUS\_FLT\_INSTANCE\_NAME\_COLLISION | An instance already exists with this name on the volume specified. |
| 0xC01C0013<br>STATUS\_FLT\_FILTER\_NOT\_FOUND | The system could not find the filter specified. |
| 0xC01C0014<br>STATUS\_FLT\_VOLUME\_NOT\_FOUND | The system could not find the volume specified. |
| 0xC01C0015<br>STATUS\_FLT\_INSTANCE\_NOT\_FOUND | The system could not find the instance specified. |
| 0xC01C0016<br>STATUS\_FLT\_CONTEXT\_ALLOCATION\_NOT\_FOUND | No registered context allocation definition was found for the given request. |
| 0xC01C0017<br>STATUS\_FLT\_INVALID\_CONTEXT\_REGISTRATION | An invalid parameter was specified during context registration. |
| 0xC01C0018<br>STATUS\_FLT\_NAME\_CACHE\_MISS | The name requested was not found in the Filter Manager name cache and could not be retrieved from the file system. |
| 0xC01C0019<br>STATUS\_FLT\_NO\_DEVICE\_OBJECT | The requested device object does not exist for the given volume. |
| 0xC01C001A<br>STATUS\_FLT\_VOLUME\_ALREADY\_MOUNTED | The specified volume is already mounted. |
| 0xC01C001B<br>STATUS\_FLT\_ALREADY\_ENLISTED | The specified transaction context is already enlisted in a transaction. |
| 0xC01C001C<br>STATUS\_FLT\_CONTEXT\_ALREADY\_LINKED | The specified context is already attached to another object. |
| 0xC01C0020<br>STATUS\_FLT\_NO\_WAITER\_FOR\_REPLY | No waiter is present for the filter's reply to this message. |
| 0xC01D0001<br>STATUS\_MONITOR\_NO\_DESCRIPTOR | A monitor descriptor could not be obtained. |
| 0xC01D0002<br>STATUS\_MONITOR\_UNKNOWN\_DESCRIPTOR\_FORMAT | This release does not support the format of the obtained monitor descriptor. |
| 0xC01D0003<br>STATUS\_MONITOR\_INVALID\_DESCRIPTOR\_CHECKSUM | The checksum of the obtained monitor descriptor is invalid. |
| 0xC01D0004<br>STATUS\_MONITOR\_INVALID\_STANDARD\_TIMING\_BLOCK | The monitor descriptor contains an invalid standard timing block. |
| 0xC01D0005<br>STATUS\_MONITOR\_WMI\_DATABLOCK\_REGISTRATION\_FAILED | WMI data-block registration failed for one of the MSMonitorClass WMI subclasses. |
| 0xC01D0006<br>STATUS\_MONITOR\_INVALID\_SERIAL\_NUMBER\_MONDSC\_BLOCK | The provided monitor descriptor block is either corrupted or does not contain the monitor's detailed serial number. |
| 0xC01D0007<br>STATUS\_MONITOR\_INVALID\_USER\_FRIENDLY\_MONDSC\_BLOCK | The provided monitor descriptor block is either corrupted or does not contain the monitor's user-friendly name. |
| 0xC01D0008<br>STATUS\_MONITOR\_NO\_MORE\_DESCRIPTOR\_DATA | There is no monitor descriptor data at the specified (offset or size) region. |
| 0xC01D0009<br>STATUS\_MONITOR\_INVALID\_DETAILED\_TIMING\_BLOCK | The monitor descriptor contains an invalid detailed timing block. |
| 0xC01D000A<br>STATUS\_MONITOR\_INVALID\_MANUFACTURE\_DATE | Monitor descriptor contains invalid manufacture date. |
| 0xC01E0000<br>STATUS\_GRAPHICS\_NOT\_EXCLUSIVE\_MODE\_OWNER | Exclusive mode ownership is needed to create an unmanaged primary allocation. |
| 0xC01E0001<br>STATUS\_GRAPHICS\_INSUFFICIENT\_DMA\_BUFFER | The driver needs more DMA buffer space to complete the requested operation. |
| 0xC01E0002<br>STATUS\_GRAPHICS\_INVALID\_DISPLAY\_ADAPTER | The specified display adapter handle is invalid. |
| 0xC01E0003<br>STATUS\_GRAPHICS\_ADAPTER\_WAS\_RESET | The specified display adapter and all of its state have been reset. |
| 0xC01E0004<br>STATUS\_GRAPHICS\_INVALID\_DRIVER\_MODEL | The driver stack does not match the expected driver model. |
| 0xC01E0005<br>STATUS\_GRAPHICS\_PRESENT\_MODE\_CHANGED | Present happened but ended up into the changed desktop mode. |
| 0xC01E0006<br>STATUS\_GRAPHICS\_PRESENT\_OCCLUDED | Nothing to present due to desktop occlusion. |
| 0xC01E0007<br>STATUS\_GRAPHICS\_PRESENT\_DENIED | Not able to present due to denial of desktop access. |
| 0xC01E0008<br>STATUS\_GRAPHICS\_CANNOTCOLORCONVERT | Not able to present with color conversion. |
| 0xC01E000B<br>STATUS\_GRAPHICS\_PRESENT\_REDIRECTION\_DISABLED | Present redirection is disabled (desktop windowing management subsystem is off). |
| 0xC01E000C<br>STATUS\_GRAPHICS\_PRESENT\_UNOCCLUDED | Previous exclusive VidPn source owner has released its ownership |
| 0xC01E0100<br>STATUS\_GRAPHICS\_NO\_VIDEO\_MEMORY | Not enough video memory is available to complete the operation. |
| 0xC01E0101<br>STATUS\_GRAPHICS\_CANT\_LOCK\_MEMORY | Could not probe and lock the underlying memory of an allocation. |
| 0xC01E0102<br>STATUS\_GRAPHICS\_ALLOCATION\_BUSY | The allocation is currently busy. |
| 0xC01E0103<br>STATUS\_GRAPHICS\_TOO\_MANY\_REFERENCES | An object being referenced has already reached the maximum reference count and cannot be referenced further. |
| 0xC01E0104<br>STATUS\_GRAPHICS\_TRY\_AGAIN\_LATER | A problem could not be solved due to an existing condition. Try again later. |
| 0xC01E0105<br>STATUS\_GRAPHICS\_TRY\_AGAIN\_NOW | A problem could not be solved due to an existing condition. Try again now. |
| 0xC01E0106<br>STATUS\_GRAPHICS\_ALLOCATION\_INVALID | The allocation is invalid. |
| 0xC01E0107<br>STATUS\_GRAPHICS\_UNSWIZZLING\_APERTURE\_UNAVAILABLE | No more unswizzling apertures are currently available. |
| 0xC01E0108<br>STATUS\_GRAPHICS\_UNSWIZZLING\_APERTURE\_UNSUPPORTED | The current allocation cannot be unswizzled by an aperture. |
| 0xC01E0109<br>STATUS\_GRAPHICS\_CANT\_EVICT\_PINNED\_ALLOCATION | The request failed because a pinned allocation cannot be evicted. |
| 0xC01E0110<br>STATUS\_GRAPHICS\_INVALID\_ALLOCATION\_USAGE | The allocation cannot be used from its current segment location for the specified operation. |
| 0xC01E0111<br>STATUS\_GRAPHICS\_CANT\_RENDER\_LOCKED\_ALLOCATION | A locked allocation cannot be used in the current command buffer. |
| 0xC01E0112<br>STATUS\_GRAPHICS\_ALLOCATION\_CLOSED | The allocation being referenced has been closed permanently. |
| 0xC01E0113<br>STATUS\_GRAPHICS\_INVALID\_ALLOCATION\_INSTANCE | An invalid allocation instance is being referenced. |
| 0xC01E0114<br>STATUS\_GRAPHICS\_INVALID\_ALLOCATION\_HANDLE | An invalid allocation handle is being referenced. |
| 0xC01E0115<br>STATUS\_GRAPHICS\_WRONG\_ALLOCATION\_DEVICE | The allocation being referenced does not belong to the current device. |
| 0xC01E0116<br>STATUS\_GRAPHICS\_ALLOCATION\_CONTENT\_LOST | The specified allocation lost its content. |
| 0xC01E0200<br>STATUS\_GRAPHICS\_GPU\_EXCEPTION\_ON\_DEVICE | A GPU exception was detected on the given device. The device cannot be scheduled. |
| 0xC01E0300<br>STATUS\_GRAPHICS\_INVALID\_VIDPN\_TOPOLOGY | The specified VidPN topology is invalid. |
| 0xC01E0301<br>STATUS\_GRAPHICS\_VIDPN\_TOPOLOGY\_NOT\_SUPPORTED | The specified VidPN topology is valid but is not supported by this model of the display adapter. |
| 0xC01E0302<br>STATUS\_GRAPHICS\_VIDPN\_TOPOLOGY\_CURRENTLY\_NOT\_SUPPORTED | The specified VidPN topology is valid but is not currently supported by the display adapter due to allocation of its resources. |
| 0xC01E0303<br>STATUS\_GRAPHICS\_INVALID\_VIDPN | The specified VidPN handle is invalid. |
| 0xC01E0304<br>STATUS\_GRAPHICS\_INVALID\_VIDEO\_PRESENT\_SOURCE | The specified video present source is invalid. |
| 0xC01E0305<br>STATUS\_GRAPHICS\_INVALID\_VIDEO\_PRESENT\_TARGET | The specified video present target is invalid. |
| 0xC01E0306<br>STATUS\_GRAPHICS\_VIDPN\_MODALITY\_NOT\_SUPPORTED | The specified VidPN modality is not supported (for example, at least two of the pinned modes are not co-functional). |
| 0xC01E0308<br>STATUS\_GRAPHICS\_INVALID\_VIDPN\_SOURCEMODESET | The specified VidPN source mode set is invalid. |
| 0xC01E0309<br>STATUS\_GRAPHICS\_INVALID\_VIDPN\_TARGETMODESET | The specified VidPN target mode set is invalid. |
| 0xC01E030A<br>STATUS\_GRAPHICS\_INVALID\_FREQUENCY | The specified video signal frequency is invalid. |
| 0xC01E030B<br>STATUS\_GRAPHICS\_INVALID\_ACTIVE\_REGION | The specified video signal active region is invalid. |
| 0xC01E030C<br>STATUS\_GRAPHICS\_INVALID\_TOTAL\_REGION | The specified video signal total region is invalid. |
| 0xC01E0310<br>STATUS\_GRAPHICS\_INVALID\_VIDEO\_PRESENT\_SOURCE\_MODE | The specified video present source mode is invalid. |
| 0xC01E0311<br>STATUS\_GRAPHICS\_INVALID\_VIDEO\_PRESENT\_TARGET\_MODE | The specified video present target mode is invalid. |
| 0xC01E0312<br>STATUS\_GRAPHICS\_PINNED\_MODE\_MUST\_REMAIN\_IN\_SET | The pinned mode must remain in the set on the VidPN's co-functional modality enumeration. |
| 0xC01E0313<br>STATUS\_GRAPHICS\_PATH\_ALREADY\_IN\_TOPOLOGY | The specified video present path is already in the VidPN's topology. |
| 0xC01E0314<br>STATUS\_GRAPHICS\_MODE\_ALREADY\_IN\_MODESET | The specified mode is already in the mode set. |
| 0xC01E0315<br>STATUS\_GRAPHICS\_INVALID\_VIDEOPRESENTSOURCESET | The specified video present source set is invalid. |
| 0xC01E0316<br>STATUS\_GRAPHICS\_INVALID\_VIDEOPRESENTTARGETSET | The specified video present target set is invalid. |
| 0xC01E0317<br>STATUS\_GRAPHICS\_SOURCE\_ALREADY\_IN\_SET | The specified video present source is already in the video present source set. |
| 0xC01E0318<br>STATUS\_GRAPHICS\_TARGET\_ALREADY\_IN\_SET | The specified video present target is already in the video present target set. |
| 0xC01E0319<br>STATUS\_GRAPHICS\_INVALID\_VIDPN\_PRESENT\_PATH | The specified VidPN present path is invalid. |
| 0xC01E031A<br>STATUS\_GRAPHICS\_NO\_RECOMMENDED\_VIDPN\_TOPOLOGY | The miniport has no recommendation for augmenting the specified VidPN's topology. |
| 0xC01E031B<br>STATUS\_GRAPHICS\_INVALID\_MONITOR\_FREQUENCYRANGESET | The specified monitor frequency range set is invalid. |
| 0xC01E031C<br>STATUS\_GRAPHICS\_INVALID\_MONITOR\_FREQUENCYRANGE | The specified monitor frequency range is invalid. |
| 0xC01E031D<br>STATUS\_GRAPHICS\_FREQUENCYRANGE\_NOT\_IN\_SET | The specified frequency range is not in the specified monitor frequency range set. |
| 0xC01E031F<br>STATUS\_GRAPHICS\_FREQUENCYRANGE\_ALREADY\_IN\_SET | The specified frequency range is already in the specified monitor frequency range set. |
| 0xC01E0320<br>STATUS\_GRAPHICS\_STALE\_MODESET | The specified mode set is stale. Reacquire the new mode set. |
| 0xC01E0321<br>STATUS\_GRAPHICS\_INVALID\_MONITOR\_SOURCEMODESET | The specified monitor source mode set is invalid. |
| 0xC01E0322<br>STATUS\_GRAPHICS\_INVALID\_MONITOR\_SOURCE\_MODE | The specified monitor source mode is invalid. |
| 0xC01E0323<br>STATUS\_GRAPHICS\_NO\_RECOMMENDED\_FUNCTIONAL\_VIDPN | The miniport does not have a recommendation regarding the request to provide a functional VidPN given the current display adapter configuration. |
| 0xC01E0324<br>STATUS\_GRAPHICS\_MODE\_ID\_MUST\_BE\_UNIQUE | The ID of the specified mode is being used by another mode in the set. |
| 0xC01E0325<br>STATUS\_GRAPHICS\_EMPTY\_ADAPTER\_MONITOR\_MODE\_SUPPORT\_INTERSECTION | The system failed to determine a mode that is supported by both the display adapter and the monitor connected to it. |
| 0xC01E0326<br>STATUS\_GRAPHICS\_VIDEO\_PRESENT\_TARGETS\_LESS\_THAN\_SOURCES | The number of video present targets must be greater than or equal to the number of video present sources. |
| 0xC01E0327<br>STATUS\_GRAPHICS\_PATH\_NOT\_IN\_TOPOLOGY | The specified present path is not in the VidPN's topology. |
| 0xC01E0328<br>STATUS\_GRAPHICS\_ADAPTER\_MUST\_HAVE\_AT\_LEAST\_ONE\_SOURCE | The display adapter must have at least one video present source. |
| 0xC01E0329<br>STATUS\_GRAPHICS\_ADAPTER\_MUST\_HAVE\_AT\_LEAST\_ONE\_TARGET | The display adapter must have at least one video present target. |
| 0xC01E032A<br>STATUS\_GRAPHICS\_INVALID\_MONITORDESCRIPTORSET | The specified monitor descriptor set is invalid. |
| 0xC01E032B<br>STATUS\_GRAPHICS\_INVALID\_MONITORDESCRIPTOR | The specified monitor descriptor is invalid. |
| 0xC01E032C<br>STATUS\_GRAPHICS\_MONITORDESCRIPTOR\_NOT\_IN\_SET | The specified descriptor is not in the specified monitor descriptor set. |
| 0xC01E032D<br>STATUS\_GRAPHICS\_MONITORDESCRIPTOR\_ALREADY\_IN\_SET | The specified descriptor is already in the specified monitor descriptor set. |
| 0xC01E032E<br>STATUS\_GRAPHICS\_MONITORDESCRIPTOR\_ID\_MUST\_BE\_UNIQUE | The ID of the specified monitor descriptor is being used by another descriptor in the set. |
| 0xC01E032F<br>STATUS\_GRAPHICS\_INVALID\_VIDPN\_TARGET\_SUBSET\_TYPE | The specified video present target subset type is invalid. |
| 0xC01E0330<br>STATUS\_GRAPHICS\_RESOURCES\_NOT\_RELATED | Two or more of the specified resources are not related to each other, as defined by the interface semantics. |
| 0xC01E0331<br>STATUS\_GRAPHICS\_SOURCE\_ID\_MUST\_BE\_UNIQUE | The ID of the specified video present source is being used by another source in the set. |
| 0xC01E0332<br>STATUS\_GRAPHICS\_TARGET\_ID\_MUST\_BE\_UNIQUE | The ID of the specified video present target is being used by another target in the set. |
| 0xC01E0333<br>STATUS\_GRAPHICS\_NO\_AVAILABLE\_VIDPN\_TARGET | The specified VidPN source cannot be used because there is no available VidPN target to connect it to. |
| 0xC01E0334<br>STATUS\_GRAPHICS\_MONITOR\_COULD\_NOT\_BE\_ASSOCIATED\_WITH\_ADAPTER | The newly arrived monitor could not be associated with a display adapter. |
| 0xC01E0335<br>STATUS\_GRAPHICS\_NO\_VIDPNMGR | The particular display adapter does not have an associated VidPN manager. |
| 0xC01E0336<br>STATUS\_GRAPHICS\_NO\_ACTIVE\_VIDPN | The VidPN manager of the particular display adapter does not have an active VidPN. |
| 0xC01E0337<br>STATUS\_GRAPHICS\_STALE\_VIDPN\_TOPOLOGY | The specified VidPN topology is stale; obtain the new topology. |
| 0xC01E0338<br>STATUS\_GRAPHICS\_MONITOR\_NOT\_CONNECTED | No monitor is connected on the specified video present target. |
| 0xC01E0339<br>STATUS\_GRAPHICS\_SOURCE\_NOT\_IN\_TOPOLOGY | The specified source is not part of the specified VidPN's topology. |
| 0xC01E033A<br>STATUS\_GRAPHICS\_INVALID\_PRIMARYSURFACE\_SIZE | The specified primary surface size is invalid. |
| 0xC01E033B<br>STATUS\_GRAPHICS\_INVALID\_VISIBLEREGION\_SIZE | The specified visible region size is invalid. |
| 0xC01E033C<br>STATUS\_GRAPHICS\_INVALID\_STRIDE | The specified stride is invalid. |
| 0xC01E033D<br>STATUS\_GRAPHICS\_INVALID\_PIXELFORMAT | The specified pixel format is invalid. |
| 0xC01E033E<br>STATUS\_GRAPHICS\_INVALID\_COLORBASIS | The specified color basis is invalid. |
| 0xC01E033F<br>STATUS\_GRAPHICS\_INVALID\_PIXELVALUEACCESSMODE | The specified pixel value access mode is invalid. |
| 0xC01E0340<br>STATUS\_GRAPHICS\_TARGET\_NOT\_IN\_TOPOLOGY | The specified target is not part of the specified VidPN's topology. |
| 0xC01E0341<br>STATUS\_GRAPHICS\_NO\_DISPLAY\_MODE\_MANAGEMENT\_SUPPORT | Failed to acquire the display mode management interface. |
| 0xC01E0342<br>STATUS\_GRAPHICS\_VIDPN\_SOURCE\_IN\_USE | The specified VidPN source is already owned by a DMM client and cannot be used until that client releases it. |
| 0xC01E0343<br>STATUS\_GRAPHICS\_CANT\_ACCESS\_ACTIVE\_VIDPN | The specified VidPN is active and cannot be accessed. |
| 0xC01E0344<br>STATUS\_GRAPHICS\_INVALID\_PATH\_IMPORTANCE\_ORDINAL | The specified VidPN's present path importance ordinal is invalid. |
| 0xC01E0345<br>STATUS\_GRAPHICS\_INVALID\_PATH\_CONTENT\_GEOMETRY\_TRANSFORMATION | The specified VidPN's present path content geometry transformation is invalid. |
| 0xC01E0346<br>STATUS\_GRAPHICS\_PATH\_CONTENT\_GEOMETRY\_TRANSFORMATION\_NOT\_SUPPORTED | The specified content geometry transformation is not supported on the respective VidPN present path. |
| 0xC01E0347<br>STATUS\_GRAPHICS\_INVALID\_GAMMA\_RAMP | The specified gamma ramp is invalid. |
| 0xC01E0348<br>STATUS\_GRAPHICS\_GAMMA\_RAMP\_NOT\_SUPPORTED | The specified gamma ramp is not supported on the respective VidPN present path. |
| 0xC01E0349<br>STATUS\_GRAPHICS\_MULTISAMPLING\_NOT\_SUPPORTED | Multisampling is not supported on the respective VidPN present path. |
| 0xC01E034A<br>STATUS\_GRAPHICS\_MODE\_NOT\_IN\_MODESET | The specified mode is not in the specified mode set. |
| 0xC01E034D<br>STATUS\_GRAPHICS\_INVALID\_VIDPN\_TOPOLOGY\_RECOMMENDATION\_REASON | The specified VidPN topology recommendation reason is invalid. |
| 0xC01E034E<br>STATUS\_GRAPHICS\_INVALID\_PATH\_CONTENT\_TYPE | The specified VidPN present path content type is invalid. |
| 0xC01E034F<br>STATUS\_GRAPHICS\_INVALID\_COPYPROTECTION\_TYPE | The specified VidPN present path copy protection type is invalid. |
| 0xC01E0350<br>STATUS\_GRAPHICS\_UNASSIGNED\_MODESET\_ALREADY\_EXISTS | Only one unassigned mode set can exist at any one time for a particular VidPN source or target. |
| 0xC01E0352<br>STATUS\_GRAPHICS\_INVALID\_SCANLINE\_ORDERING | The specified scan line ordering type is invalid. |
| 0xC01E0353<br>STATUS\_GRAPHICS\_TOPOLOGY\_CHANGES\_NOT\_ALLOWED | The topology changes are not allowed for the specified VidPN. |
| 0xC01E0354<br>STATUS\_GRAPHICS\_NO\_AVAILABLE\_IMPORTANCE\_ORDINALS | All available importance ordinals are being used in the specified topology. |
| 0xC01E0355<br>STATUS\_GRAPHICS\_INCOMPATIBLE\_PRIVATE\_FORMAT | The specified primary surface has a different private-format attribute than the current primary surface. |
| 0xC01E0356<br>STATUS\_GRAPHICS\_INVALID\_MODE\_PRUNING\_ALGORITHM | The specified mode-pruning algorithm is invalid. |
| 0xC01E0357<br>STATUS\_GRAPHICS\_INVALID\_MONITOR\_CAPABILITY\_ORIGIN | The specified monitor-capability origin is invalid. |
| 0xC01E0358<br>STATUS\_GRAPHICS\_INVALID\_MONITOR\_FREQUENCYRANGE\_CONSTRAINT | The specified monitor-frequency range constraint is invalid. |
| 0xC01E0359<br>STATUS\_GRAPHICS\_MAX\_NUM\_PATHS\_REACHED | The maximum supported number of present paths has been reached. |
| 0xC01E035A<br>STATUS\_GRAPHICS\_CANCEL\_VIDPN\_TOPOLOGY\_AUGMENTATION | The miniport requested that augmentation be canceled for the specified source of the specified VidPN's topology. |
| 0xC01E035B<br>STATUS\_GRAPHICS\_INVALID\_CLIENT\_TYPE | The specified client type was not recognized. |
| 0xC01E035C<br>STATUS\_GRAPHICS\_CLIENTVIDPN\_NOT\_SET | The client VidPN is not set on this adapter (for example, no user mode-initiated mode changes have taken place on this adapter). |
| 0xC01E0400<br>STATUS\_GRAPHICS\_SPECIFIED\_CHILD\_ALREADY\_CONNECTED | The specified display adapter child device already has an external device connected to it. |
| 0xC01E0401<br>STATUS\_GRAPHICS\_CHILD\_DESCRIPTOR\_NOT\_SUPPORTED | The display adapter child device does not support reporting a descriptor. |
| 0xC01E0430<br>STATUS\_GRAPHICS\_NOT\_A\_LINKED\_ADAPTER | The display adapter is not linked to any other adapters. |
| 0xC01E0431<br>STATUS\_GRAPHICS\_LEADLINK\_NOT\_ENUMERATED | The lead adapter in a linked configuration was not enumerated yet. |
| 0xC01E0432<br>STATUS\_GRAPHICS\_CHAINLINKS\_NOT\_ENUMERATED | Some chain adapters in a linked configuration have not yet been enumerated. |
| 0xC01E0433<br>STATUS\_GRAPHICS\_ADAPTER\_CHAIN\_NOT\_READY | The chain of linked adapters is not ready to start because of an unknown failure. |
| 0xC01E0434<br>STATUS\_GRAPHICS\_CHAINLINKS\_NOT\_STARTED | An attempt was made to start a lead link display adapter when the chain links had not yet started. |
| 0xC01E0435<br>STATUS\_GRAPHICS\_CHAINLINKS\_NOT\_POWERED\_ON | An attempt was made to turn on a lead link display adapter when the chain links were turned off. |
| 0xC01E0436<br>STATUS\_GRAPHICS\_INCONSISTENT\_DEVICE\_LINK\_STATE | The adapter link was found in an inconsistent state. Not all adapters are in an expected PNP/power state. |
| 0xC01E0438<br>STATUS\_GRAPHICS\_NOT\_POST\_DEVICE\_DRIVER | The driver trying to start is not the same as the driver for the posted display adapter. |
| 0xC01E043B<br>STATUS\_GRAPHICS\_ADAPTER\_ACCESS\_NOT\_EXCLUDED | An operation is being attempted that requires the display adapter to be in a quiescent state. |
| 0xC01E0500<br>STATUS\_GRAPHICS\_OPM\_NOT\_SUPPORTED | The driver does not support OPM. |
| 0xC01E0501<br>STATUS\_GRAPHICS\_COPP\_NOT\_SUPPORTED | The driver does not support COPP. |
| 0xC01E0502<br>STATUS\_GRAPHICS\_UAB\_NOT\_SUPPORTED | The driver does not support UAB. |
| 0xC01E0503<br>STATUS\_GRAPHICS\_OPM\_INVALID\_ENCRYPTED\_PARAMETERS | The specified encrypted parameters are invalid. |
| 0xC01E0504<br>STATUS\_GRAPHICS\_OPM\_PARAMETER\_ARRAY\_TOO\_SMALL | An array passed to a function cannot hold all of the data that the function wants to put in it. |
| 0xC01E0505<br>STATUS\_GRAPHICS\_OPM\_NO\_PROTECTED\_OUTPUTS\_EXIST | The GDI display device passed to this function does not have any active protected outputs. |
| 0xC01E0506<br>STATUS\_GRAPHICS\_PVP\_NO\_DISPLAY\_DEVICE\_CORRESPONDS\_TO\_NAME | The PVP cannot find an actual GDI display device that corresponds to the passed-in GDI display device name. |
| 0xC01E0507<br>STATUS\_GRAPHICS\_PVP\_DISPLAY\_DEVICE\_NOT\_ATTACHED\_TO\_DESKTOP | This function failed because the GDI display device passed to it was not attached to the Windows desktop. |
| 0xC01E0508<br>STATUS\_GRAPHICS\_PVP\_MIRRORING\_DEVICES\_NOT\_SUPPORTED | The PVP does not support mirroring display devices because they do not have any protected outputs. |
| 0xC01E050A<br>STATUS\_GRAPHICS\_OPM\_INVALID\_POINTER | The function failed because an invalid pointer parameter was passed to it. A pointer parameter is invalid if it is null, is not correctly aligned, or it points to an invalid address or a kernel mode address. |
| 0xC01E050B<br>STATUS\_GRAPHICS\_OPM\_INTERNAL\_ERROR | An internal error caused an operation to fail. |
| 0xC01E050C<br>STATUS\_GRAPHICS\_OPM\_INVALID\_HANDLE | The function failed because the caller passed in an invalid OPM user-mode handle. |
| 0xC01E050D<br>STATUS\_GRAPHICS\_PVP\_NO\_MONITORS\_CORRESPOND\_TO\_DISPLAY\_DEVICE | This function failed because the GDI device passed to it did not have any monitors associated with it. |
| 0xC01E050E<br>STATUS\_GRAPHICS\_PVP\_INVALID\_CERTIFICATE\_LENGTH | A certificate could not be returned because the certificate buffer passed to the function was too small. |
| 0xC01E050F<br>STATUS\_GRAPHICS\_OPM\_SPANNING\_MODE\_ENABLED | DxgkDdiOpmCreateProtectedOutput() could not create a protected output because the video present yarget is in spanning mode. |
| 0xC01E0510<br>STATUS\_GRAPHICS\_OPM\_THEATER\_MODE\_ENABLED | DxgkDdiOpmCreateProtectedOutput() could not create a protected output because the video present target is in theater mode. |
| 0xC01E0511<br>STATUS\_GRAPHICS\_PVP\_HFS\_FAILED | The function call failed because the display adapter's hardware functionality scan (HFS) failed to validate the graphics hardware. |
| 0xC01E0512<br>STATUS\_GRAPHICS\_OPM\_INVALID\_SRM | The HDCP SRM passed to this function did not comply with section 5 of the HDCP 1.1 specification. |
| 0xC01E0513<br>STATUS\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP | The protected output cannot enable the HDCP system because it does not support it. |
| 0xC01E0514<br>STATUS\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_ACP | The protected output cannot enable analog copy protection because it does not support it. |
| 0xC01E0515<br>STATUS\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_CGMSA | The protected output cannot enable the CGMS-A protection technology because it does not support it. |
| 0xC01E0516<br>STATUS\_GRAPHICS\_OPM\_HDCP\_SRM\_NEVER\_SET | DxgkDdiOPMGetInformation() cannot return the version of the SRM being used because the application never successfully passed an SRM to the protected output. |
| 0xC01E0517<br>STATUS\_GRAPHICS\_OPM\_RESOLUTION\_TOO\_HIGH | DxgkDdiOPMConfigureProtectedOutput() cannot enable the specified output protection technology because the output's screen resolution is too high. |
| 0xC01E0518<br>STATUS\_GRAPHICS\_OPM\_ALL\_HDCP\_HARDWARE\_ALREADY\_IN\_USE | DxgkDdiOPMConfigureProtectedOutput() cannot enable HDCP because other physical outputs are using the display adapter's HDCP hardware. |
| 0xC01E051A<br>STATUS\_GRAPHICS\_OPM\_PROTECTED\_OUTPUT\_NO\_LONGER\_EXISTS | The operating system asynchronously destroyed this OPM-protected output because the operating system state changed. This error typically occurs because the monitor PDO associated with this protected output was removed or stopped, the protected output's session became a nonconsole session, or the protected output's desktop became inactive. |
| 0xC01E051B<br>STATUS\_GRAPHICS\_OPM\_SESSION\_TYPE\_CHANGE\_IN\_PROGRESS | OPM functions cannot be called when a session is changing its type. Three types of sessions currently exist: console, disconnected, and remote (RDP or ICA). |
| 0xC01E051C<br>STATUS\_GRAPHICS\_OPM\_PROTECTED\_OUTPUT\_DOES\_NOT\_HAVE\_COPP\_SEMANTICS | The DxgkDdiOPMGetCOPPCompatibleInformation, DxgkDdiOPMGetInformation, or DxgkDdiOPMConfigureProtectedOutput function failed. This error is returned only if a protected output has OPM semantics. <br>DxgkDdiOPMGetCOPPCompatibleInformation always returns this error if a protected output has OPM semantics.<br>DxgkDdiOPMGetInformation returns this error code if the caller requested COPP-specific information.<br>DxgkDdiOPMConfigureProtectedOutput returns this error when the caller tries to use a COPP-specific command. |
| 0xC01E051D<br>STATUS\_GRAPHICS\_OPM\_INVALID\_INFORMATION\_REQUEST | The DxgkDdiOPMGetInformation and DxgkDdiOPMGetCOPPCompatibleInformation functions return this error code if the passed-in sequence number is not the expected sequence number or the passed-in OMAC value is invalid. |
| 0xC01E051E<br>STATUS\_GRAPHICS\_OPM\_DRIVER\_INTERNAL\_ERROR | The function failed because an unexpected error occurred inside a display driver. |
| 0xC01E051F<br>STATUS\_GRAPHICS\_OPM\_PROTECTED\_OUTPUT\_DOES\_NOT\_HAVE\_OPM\_SEMANTICS | The DxgkDdiOPMGetCOPPCompatibleInformation, DxgkDdiOPMGetInformation, or DxgkDdiOPMConfigureProtectedOutput function failed. This error is returned only if a protected output has COPP semantics. <br>DxgkDdiOPMGetCOPPCompatibleInformation returns this error code if the caller requested OPM-specific information.<br>DxgkDdiOPMGetInformation always returns this error if a protected output has COPP semantics.<br>DxgkDdiOPMConfigureProtectedOutput returns this error when the caller tries to use an OPM-specific command. |
| 0xC01E0520<br>STATUS\_GRAPHICS\_OPM\_SIGNALING\_NOT\_SUPPORTED | The DxgkDdiOPMGetCOPPCompatibleInformation and DxgkDdiOPMConfigureProtectedOutput functions return this error if the display driver does not support the DXGKMDT\_OPM\_GET\_ACP\_AND\_CGMSA\_SIGNALING and DXGKMDT\_OPM\_SET\_ACP\_AND\_CGMSA\_SIGNALING GUIDs. |
| 0xC01E0521<br>STATUS\_GRAPHICS\_OPM\_INVALID\_CONFIGURATION\_REQUEST | The DxgkDdiOPMConfigureProtectedOutput function returns this error code if the passed-in sequence number is not the expected sequence number or the passed-in OMAC value is invalid. |
| 0xC01E0580<br>STATUS\_GRAPHICS\_I2C\_NOT\_SUPPORTED | The monitor connected to the specified video output does not have an I2C bus. |
| 0xC01E0581<br>STATUS\_GRAPHICS\_I2C\_DEVICE\_DOES\_NOT\_EXIST | No device on the I2C bus has the specified address. |
| 0xC01E0582<br>STATUS\_GRAPHICS\_I2C\_ERROR\_TRANSMITTING\_DATA | An error occurred while transmitting data to the device on the I2C bus. |
| 0xC01E0583<br>STATUS\_GRAPHICS\_I2C\_ERROR\_RECEIVING\_DATA | An error occurred while receiving data from the device on the I2C bus. |
| 0xC01E0584<br>STATUS\_GRAPHICS\_DDCCI\_VCP\_NOT\_SUPPORTED | The monitor does not support the specified VCP code. |
| 0xC01E0585<br>STATUS\_GRAPHICS\_DDCCI\_INVALID\_DATA | The data received from the monitor is invalid. |
| 0xC01E0586<br>STATUS\_GRAPHICS\_DDCCI\_MONITOR\_RETURNED\_INVALID\_TIMING\_STATUS\_BYTE | A function call failed because a monitor returned an invalid timing status byte when the operating system used the DDC/CI get timing report and timing message command to get a timing report from a monitor. |
| 0xC01E0587<br>STATUS\_GRAPHICS\_DDCCI\_INVALID\_CAPABILITIES\_STRING | A monitor returned a DDC/CI capabilities string that did not comply with the ACCESS.bus 3.0, DDC/CI 1.1, or MCCS 2 Revision 1 specification. |
| 0xC01E0588<br>STATUS\_GRAPHICS\_MCA\_INTERNAL\_ERROR | An internal error caused an operation to fail. |
| 0xC01E0589<br>STATUS\_GRAPHICS\_DDCCI\_INVALID\_MESSAGE\_COMMAND | An operation failed because a DDC/CI message had an invalid value in its command field. |
| 0xC01E058A<br>STATUS\_GRAPHICS\_DDCCI\_INVALID\_MESSAGE\_LENGTH | This error occurred because a DDC/CI message had an invalid value in its length field. |
| 0xC01E058B<br>STATUS\_GRAPHICS\_DDCCI\_INVALID\_MESSAGE\_CHECKSUM | This error occurred because the value in a DDC/CI message's checksum field did not match the message's computed checksum value. This error implies that the data was corrupted while it was being transmitted from a monitor to a computer. |
| 0xC01E058C<br>STATUS\_GRAPHICS\_INVALID\_PHYSICAL\_MONITOR\_HANDLE | This function failed because an invalid monitor handle was passed to it. |
| 0xC01E058D<br>STATUS\_GRAPHICS\_MONITOR\_NO\_LONGER\_EXISTS | The operating system asynchronously destroyed the monitor that corresponds to this handle because the operating system's state changed. This error typically occurs because the monitor PDO associated with this handle was removed or stopped, or a display mode change occurred. A display mode change occurs when Windows sends a WM\_DISPLAYCHANGE message to applications. |
| 0xC01E05E0<br>STATUS\_GRAPHICS\_ONLY\_CONSOLE\_SESSION\_SUPPORTED | This function can be used only if a program is running in the local console session. It cannot be used if a program is running on a remote desktop session or on a terminal server session. |
| 0xC01E05E1<br>STATUS\_GRAPHICS\_NO\_DISPLAY\_DEVICE\_CORRESPONDS\_TO\_NAME | This function cannot find an actual GDI display device that corresponds to the specified GDI display device name. |
| 0xC01E05E2<br>STATUS\_GRAPHICS\_DISPLAY\_DEVICE\_NOT\_ATTACHED\_TO\_DESKTOP | The function failed because the specified GDI display device was not attached to the Windows desktop. |
| 0xC01E05E3<br>STATUS\_GRAPHICS\_MIRRORING\_DEVICES\_NOT\_SUPPORTED | This function does not support GDI mirroring display devices because GDI mirroring display devices do not have any physical monitors associated with them. |
| 0xC01E05E4<br>STATUS\_GRAPHICS\_INVALID\_POINTER | The function failed because an invalid pointer parameter was passed to it. A pointer parameter is invalid if it is null, is not correctly aligned, or points to an invalid address or to a kernel mode address. |
| 0xC01E05E5<br>STATUS\_GRAPHICS\_NO\_MONITORS\_CORRESPOND\_TO\_DISPLAY\_DEVICE | This function failed because the GDI device passed to it did not have a monitor associated with it. |
| 0xC01E05E6<br>STATUS\_GRAPHICS\_PARAMETER\_ARRAY\_TOO\_SMALL | An array passed to the function cannot hold all of the data that the function must copy into the array. |
| 0xC01E05E7<br>STATUS\_GRAPHICS\_INTERNAL\_ERROR | An internal error caused an operation to fail. |
| 0xC01E05E8<br>STATUS\_GRAPHICS\_SESSION\_TYPE\_CHANGE\_IN\_PROGRESS | The function failed because the current session is changing its type. This function cannot be called when the current session is changing its type. Three types of sessions currently exist: console, disconnected, and remote (RDP or ICA). |
| 0xC0210000<br>STATUS\_FVE\_LOCKED\_VOLUME | The volume must be unlocked before it can be used. |
| 0xC0210001<br>STATUS\_FVE\_NOT\_ENCRYPTED | The volume is fully decrypted and no key is available. |
| 0xC0210002<br>STATUS\_FVE\_BAD\_INFORMATION | The control block for the encrypted volume is not valid. |
| 0xC0210003<br>STATUS\_FVE\_TOO\_SMALL | Not enough free space remains on the volume to allow encryption. |
| 0xC0210004<br>STATUS\_FVE\_FAILED\_WRONG\_FS | The partition cannot be encrypted because the file system is not supported. |
| 0xC0210005<br>STATUS\_FVE\_FAILED\_BAD\_FS | The file system is inconsistent. Run the Check Disk utility. |
| 0xC0210006<br>STATUS\_FVE\_FS\_NOT\_EXTENDED | The file system does not extend to the end of the volume. |
| 0xC0210007<br>STATUS\_FVE\_FS\_MOUNTED | This operation cannot be performed while a file system is mounted on the volume. |
| 0xC0210008<br>STATUS\_FVE\_NO\_LICENSE | BitLocker Drive Encryption is not included with this version of Windows. |
| 0xC0210009<br>STATUS\_FVE\_ACTION\_NOT\_ALLOWED | The requested action was denied by the FVE control engine. |
| 0xC021000A<br>STATUS\_FVE\_BAD\_DATA | The data supplied is malformed. |
| 0xC021000B<br>STATUS\_FVE\_VOLUME\_NOT\_BOUND | The volume is not bound to the system. |
| 0xC021000C<br>STATUS\_FVE\_NOT\_DATA\_VOLUME | The volume specified is not a data volume. |
| 0xC021000D<br>STATUS\_FVE\_CONV\_READ\_ERROR | A read operation failed while converting the volume. |
| 0xC021000E<br>STATUS\_FVE\_CONV\_WRITE\_ERROR | A write operation failed while converting the volume. |
| 0xC021000F<br>STATUS\_FVE\_OVERLAPPED\_UPDATE | The control block for the encrypted volume was updated by another thread. Try again. |
| 0xC0210010<br>STATUS\_FVE\_FAILED\_SECTOR\_SIZE | The volume encryption algorithm cannot be used on this sector size. |
| 0xC0210011<br>STATUS\_FVE\_FAILED\_AUTHENTICATION | BitLocker recovery authentication failed. |
| 0xC0210012<br>STATUS\_FVE\_NOT\_OS\_VOLUME | The volume specified is not the boot operating system volume. |
| 0xC0210013<br>STATUS\_FVE\_KEYFILE\_NOT\_FOUND | The BitLocker startup key or recovery password could not be read from external media. |
| 0xC0210014<br>STATUS\_FVE\_KEYFILE\_INVALID | The BitLocker startup key or recovery password file is corrupt or invalid. |
| 0xC0210015<br>STATUS\_FVE\_KEYFILE\_NO\_VMK | The BitLocker encryption key could not be obtained from the startup key or the recovery password. |
| 0xC0210016<br>STATUS\_FVE\_TPM\_DISABLED | The TPM is disabled. |
| 0xC0210017<br>STATUS\_FVE\_TPM\_SRK\_AUTH\_NOT\_ZERO | The authorization data for the SRK of the TPM is not zero. |
| 0xC0210018<br>STATUS\_FVE\_TPM\_INVALID\_PCR | The system boot information changed or the TPM locked out access to BitLocker encryption keys until the computer is restarted. |
| 0xC0210019<br>STATUS\_FVE\_TPM\_NO\_VMK | The BitLocker encryption key could not be obtained from the TPM. |
| 0xC021001A<br>STATUS\_FVE\_PIN\_INVALID | The BitLocker encryption key could not be obtained from the TPM and PIN. |
| 0xC021001B<br>STATUS\_FVE\_AUTH\_INVALID\_APPLICATION | A boot application hash does not match the hash computed when BitLocker was turned on. |
| 0xC021001C<br>STATUS\_FVE\_AUTH\_INVALID\_CONFIG | The Boot Configuration Data (BCD) settings are not supported or have changed because BitLocker was enabled. |
| 0xC021001D<br>STATUS\_FVE\_DEBUGGER\_ENABLED | Boot debugging is enabled. Run Windows Boot Configuration Data Store Editor (bcdedit.exe) to turn it off. |
| 0xC021001E<br>STATUS\_FVE\_DRY\_RUN\_FAILED | The BitLocker encryption key could not be obtained. |
| 0xC021001F<br>STATUS\_FVE\_BAD\_METADATA\_POINTER | The metadata disk region pointer is incorrect. |
| 0xC0210020<br>STATUS\_FVE\_OLD\_METADATA\_COPY | The backup copy of the metadata is out of date. |
| 0xC0210021<br>STATUS\_FVE\_REBOOT\_REQUIRED | No action was taken because a system restart is required. |
| 0xC0210022<br>STATUS\_FVE\_RAW\_ACCESS | No action was taken because BitLocker Drive Encryption is in RAW access mode. |
| 0xC0210023<br>STATUS\_FVE\_RAW\_BLOCKED | BitLocker Drive Encryption cannot enter RAW access mode for this volume. |
| 0xC0210026<br>STATUS\_FVE\_NO\_FEATURE\_LICENSE | This feature of BitLocker Drive Encryption is not included with this version of Windows. |
| 0xC0210027<br>STATUS\_FVE\_POLICY\_USER\_DISABLE\_RDV\_NOT\_ALLOWED | Group policy does not permit turning off BitLocker Drive Encryption on roaming data volumes. |
| 0xC0210028<br>STATUS\_FVE\_CONV\_RECOVERY\_FAILED | Bitlocker Drive Encryption failed to recover from aborted conversion. This could be due to either all conversion logs being corrupted or the media being write-protected. |
| 0xC0210029<br>STATUS\_FVE\_VIRTUALIZED\_SPACE\_TOO\_BIG | The requested virtualization size is too big. |
| 0xC0210030<br>STATUS\_FVE\_VOLUME\_TOO\_SMALL | The drive is too small to be protected using BitLocker Drive Encryption. |
| 0xC0220001<br>STATUS\_FWP\_CALLOUT\_NOT\_FOUND | The callout does not exist. |
| 0xC0220002<br>STATUS\_FWP\_CONDITION\_NOT\_FOUND | The filter condition does not exist. |
| 0xC0220003<br>STATUS\_FWP\_FILTER\_NOT\_FOUND | The filter does not exist. |
| 0xC0220004<br>STATUS\_FWP\_LAYER\_NOT\_FOUND | The layer does not exist. |
| 0xC0220005<br>STATUS\_FWP\_PROVIDER\_NOT\_FOUND | The provider does not exist. |
| 0xC0220006<br>STATUS\_FWP\_PROVIDER\_CONTEXT\_NOT\_FOUND | The provider context does not exist. |
| 0xC0220007<br>STATUS\_FWP\_SUBLAYER\_NOT\_FOUND | The sublayer does not exist. |
| 0xC0220008<br>STATUS\_FWP\_NOT\_FOUND | The object does not exist. |
| 0xC0220009<br>STATUS\_FWP\_ALREADY\_EXISTS | An object with that GUID or LUID already exists. |
| 0xC022000A<br>STATUS\_FWP\_IN\_USE | The object is referenced by other objects and cannot be deleted. |
| 0xC022000B<br>STATUS\_FWP\_DYNAMIC\_SESSION\_IN\_PROGRESS | The call is not allowed from within a dynamic session. |
| 0xC022000C<br>STATUS\_FWP\_WRONG\_SESSION | The call was made from the wrong session and cannot be completed. |
| 0xC022000D<br>STATUS\_FWP\_NO\_TXN\_IN\_PROGRESS | The call must be made from within an explicit transaction. |
| 0xC022000E<br>STATUS\_FWP\_TXN\_IN\_PROGRESS | The call is not allowed from within an explicit transaction. |
| 0xC022000F<br>STATUS\_FWP\_TXN\_ABORTED | The explicit transaction has been forcibly canceled. |
| 0xC0220010<br>STATUS\_FWP\_SESSION\_ABORTED | The session has been canceled. |
| 0xC0220011<br>STATUS\_FWP\_INCOMPATIBLE\_TXN | The call is not allowed from within a read-only transaction. |
| 0xC0220012<br>STATUS\_FWP\_TIMEOUT | The call timed out while waiting to acquire the transaction lock. |
| 0xC0220013<br>STATUS\_FWP\_NET\_EVENTS\_DISABLED | The collection of network diagnostic events is disabled. |
| 0xC0220014<br>STATUS\_FWP\_INCOMPATIBLE\_LAYER | The operation is not supported by the specified layer. |
| 0xC0220015<br>STATUS\_FWP\_KM\_CLIENTS\_ONLY | The call is allowed for kernel-mode callers only. |
| 0xC0220016<br>STATUS\_FWP\_LIFETIME\_MISMATCH | The call tried to associate two objects with incompatible lifetimes. |
| 0xC0220017<br>STATUS\_FWP\_BUILTIN\_OBJECT | The object is built-in and cannot be deleted. |
| 0xC0220018<br>STATUS\_FWP\_TOO\_MANY\_BOOTTIME\_FILTERS | The maximum number of boot-time filters has been reached. |
| 0xC0220018<br>STATUS\_FWP\_TOO\_MANY\_CALLOUTS | The maximum number of callouts has been reached. |
| 0xC0220019<br>STATUS\_FWP\_NOTIFICATION\_DROPPED | A notification could not be delivered because a message queue has reached maximum capacity. |
| 0xC022001A<br>STATUS\_FWP\_TRAFFIC\_MISMATCH | The traffic parameters do not match those for the security association context. |
| 0xC022001B<br>STATUS\_FWP\_INCOMPATIBLE\_SA\_STATE | The call is not allowed for the current security association state. |
| 0xC022001C<br>STATUS\_FWP\_NULL\_POINTER | A required pointer is null. |
| 0xC022001D<br>STATUS\_FWP\_INVALID\_ENUMERATOR | An enumerator is not valid. |
| 0xC022001E<br>STATUS\_FWP\_INVALID\_FLAGS | The flags field contains an invalid value. |
| 0xC022001F<br>STATUS\_FWP\_INVALID\_NET\_MASK | A network mask is not valid. |
| 0xC0220020<br>STATUS\_FWP\_INVALID\_RANGE | An FWP\_RANGE is not valid. |
| 0xC0220021<br>STATUS\_FWP\_INVALID\_INTERVAL | The time interval is not valid. |
| 0xC0220022<br>STATUS\_FWP\_ZERO\_LENGTH\_ARRAY | An array that must contain at least one element has a zero length. |
| 0xC0220023<br>STATUS\_FWP\_NULL\_DISPLAY\_NAME | The displayData.name field cannot be null. |
| 0xC0220024<br>STATUS\_FWP\_INVALID\_ACTION\_TYPE | The action type is not one of the allowed action types for a filter. |
| 0xC0220025<br>STATUS\_FWP\_INVALID\_WEIGHT | The filter weight is not valid. |
| 0xC0220026<br>STATUS\_FWP\_MATCH\_TYPE\_MISMATCH | A filter condition contains a match type that is not compatible with the operands. |
| 0xC0220027<br>STATUS\_FWP\_TYPE\_MISMATCH | An FWP\_VALUE or FWPM\_CONDITION\_VALUE is of the wrong type. |
| 0xC0220028<br>STATUS\_FWP\_OUT\_OF\_BOUNDS | An integer value is outside the allowed range. |
| 0xC0220029<br>STATUS\_FWP\_RESERVED | A reserved field is nonzero. |
| 0xC022002A<br>STATUS\_FWP\_DUPLICATE\_CONDITION | A filter cannot contain multiple conditions operating on a single field. |
| 0xC022002B<br>STATUS\_FWP\_DUPLICATE\_KEYMOD | A policy cannot contain the same keying module more than once. |
| 0xC022002C<br>STATUS\_FWP\_ACTION\_INCOMPATIBLE\_WITH\_LAYER | The action type is not compatible with the layer. |
| 0xC022002D<br>STATUS\_FWP\_ACTION\_INCOMPATIBLE\_WITH\_SUBLAYER | The action type is not compatible with the sublayer. |
| 0xC022002E<br>STATUS\_FWP\_CONTEXT\_INCOMPATIBLE\_WITH\_LAYER | The raw context or the provider context is not compatible with the layer. |
| 0xC022002F<br>STATUS\_FWP\_CONTEXT\_INCOMPATIBLE\_WITH\_CALLOUT | The raw context or the provider context is not compatible with the callout. |
| 0xC0220030<br>STATUS\_FWP\_INCOMPATIBLE\_AUTH\_METHOD | The authentication method is not compatible with the policy type. |
| 0xC0220031<br>STATUS\_FWP\_INCOMPATIBLE\_DH\_GROUP | The Diffie-Hellman group is not compatible with the policy type. |
| 0xC0220032<br>STATUS\_FWP\_EM\_NOT\_SUPPORTED | An IKE policy cannot contain an Extended Mode policy. |
| 0xC0220033<br>STATUS\_FWP\_NEVER\_MATCH | The enumeration template or subscription will never match any objects. |
| 0xC0220034<br>STATUS\_FWP\_PROVIDER\_CONTEXT\_MISMATCH | The provider context is of the wrong type. |
| 0xC0220035<br>STATUS\_FWP\_INVALID\_PARAMETER | The parameter is incorrect. |
| 0xC0220036<br>STATUS\_FWP\_TOO\_MANY\_SUBLAYERS | The maximum number of sublayers has been reached. |
| 0xC0220037<br>STATUS\_FWP\_CALLOUT\_NOTIFICATION\_FAILED | The notification function for a callout returned an error. |
| 0xC0220038<br>STATUS\_FWP\_INCOMPATIBLE\_AUTH\_CONFIG | The IPsec authentication configuration is not compatible with the authentication type. |
| 0xC0220039<br>STATUS\_FWP\_INCOMPATIBLE\_CIPHER\_CONFIG | The IPsec cipher configuration is not compatible with the cipher type. |
| 0xC022003C<br>STATUS\_FWP\_DUPLICATE\_AUTH\_METHOD | A policy cannot contain the same auth method more than once. |
| 0xC0220100<br>STATUS\_FWP\_TCPIP\_NOT\_READY | The TCP/IP stack is not ready. |
| 0xC0220101<br>STATUS\_FWP\_INJECT\_HANDLE\_CLOSING | The injection handle is being closed by another thread. |
| 0xC0220102<br>STATUS\_FWP\_INJECT\_HANDLE\_STALE | The injection handle is stale. |
| 0xC0220103<br>STATUS\_FWP\_CANNOT\_PEND | The classify cannot be pended. |
| 0xC0230002<br>STATUS\_NDIS\_CLOSING | The binding to the network interface is being closed. |
| 0xC0230004<br>STATUS\_NDIS\_BAD\_VERSION | An invalid version was specified. |
| 0xC0230005<br>STATUS\_NDIS\_BAD\_CHARACTERISTICS | An invalid characteristics table was used. |
| 0xC0230006<br>STATUS\_NDIS\_ADAPTER\_NOT\_FOUND | Failed to find the network interface or the network interface is not ready. |
| 0xC0230007<br>STATUS\_NDIS\_OPEN\_FAILED | Failed to open the network interface. |
| 0xC0230008<br>STATUS\_NDIS\_DEVICE\_FAILED | The network interface has encountered an internal unrecoverable failure. |
| 0xC0230009<br>STATUS\_NDIS\_MULTICAST\_FULL | The multicast list on the network interface is full. |
| 0xC023000A<br>STATUS\_NDIS\_MULTICAST\_EXISTS | An attempt was made to add a duplicate multicast address to the list. |
| 0xC023000B<br>STATUS\_NDIS\_MULTICAST\_NOT\_FOUND | At attempt was made to remove a multicast address that was never added. |
| 0xC023000C<br>STATUS\_NDIS\_REQUEST\_ABORTED | The network interface aborted the request. |
| 0xC023000D<br>STATUS\_NDIS\_RESET\_IN\_PROGRESS | The network interface cannot process the request because it is being reset. |
| 0xC023000F<br>STATUS\_NDIS\_INVALID\_PACKET | An attempt was made to send an invalid packet on a network interface. |
| 0xC0230010<br>STATUS\_NDIS\_INVALID\_DEVICE\_REQUEST | The specified request is not a valid operation for the target device. |
| 0xC0230011<br>STATUS\_NDIS\_ADAPTER\_NOT\_READY | The network interface is not ready to complete this operation. |
| 0xC0230014<br>STATUS\_NDIS\_INVALID\_LENGTH | The length of the buffer submitted for this operation is not valid. |
| 0xC0230015<br>STATUS\_NDIS\_INVALID\_DATA | The data used for this operation is not valid. |
| 0xC0230016<br>STATUS\_NDIS\_BUFFER\_TOO\_SHORT | The length of the submitted buffer for this operation is too small. |
| 0xC0230017<br>STATUS\_NDIS\_INVALID\_OID | The network interface does not support this object identifier. |
| 0xC0230018<br>STATUS\_NDIS\_ADAPTER\_REMOVED | The network interface has been removed. |
| 0xC0230019<br>STATUS\_NDIS\_UNSUPPORTED\_MEDIA | The network interface does not support this media type. |
| 0xC023001A<br>STATUS\_NDIS\_GROUP\_ADDRESS\_IN\_USE | An attempt was made to remove a token ring group address that is in use by other components. |
| 0xC023001B<br>STATUS\_NDIS\_FILE\_NOT\_FOUND | An attempt was made to map a file that cannot be found. |
| 0xC023001C<br>STATUS\_NDIS\_ERROR\_READING\_FILE | An error occurred while NDIS tried to map the file. |
| 0xC023001D<br>STATUS\_NDIS\_ALREADY\_MAPPED | An attempt was made to map a file that is already mapped. |
| 0xC023001E<br>STATUS\_NDIS\_RESOURCE\_CONFLICT | An attempt to allocate a hardware resource failed because the resource is used by another component. |
| 0xC023001F<br>STATUS\_NDIS\_MEDIA\_DISCONNECTED | The I/O operation failed because the network media is disconnected or the wireless access point is out of range. |
| 0xC0230022<br>STATUS\_NDIS\_INVALID\_ADDRESS | The network address used in the request is invalid. |
| 0xC023002A<br>STATUS\_NDIS\_PAUSED | The offload operation on the network interface has been paused. |
| 0xC023002B<br>STATUS\_NDIS\_INTERFACE\_NOT\_FOUND | The network interface was not found. |
| 0xC023002C<br>STATUS\_NDIS\_UNSUPPORTED\_REVISION | The revision number specified in the structure is not supported. |
| 0xC023002D<br>STATUS\_NDIS\_INVALID\_PORT | The specified port does not exist on this network interface. |
| 0xC023002E<br>STATUS\_NDIS\_INVALID\_PORT\_STATE | The current state of the specified port on this network interface does not support the requested operation. |
| 0xC023002F<br>STATUS\_NDIS\_LOW\_POWER\_STATE | The miniport adapter is in a lower power state. |
| 0xC02300BB<br>STATUS\_NDIS\_NOT\_SUPPORTED | The network interface does not support this request. |
| 0xC023100F<br>STATUS\_NDIS\_OFFLOAD\_POLICY | The TCP connection is not offloadable because of a local policy setting. |
| 0xC0231012<br>STATUS\_NDIS\_OFFLOAD\_CONNECTION\_REJECTED | The TCP connection is not offloadable by the Chimney offload target. |
| 0xC0231013<br>STATUS\_NDIS\_OFFLOAD\_PATH\_REJECTED | The IP Path object is not in an offloadable state. |
| 0xC0232000<br>STATUS\_NDIS\_DOT11\_AUTO\_CONFIG\_ENABLED | The wireless LAN interface is in auto-configuration mode and does not support the requested parameter change operation. |
| 0xC0232001<br>STATUS\_NDIS\_DOT11\_MEDIA\_IN\_USE | The wireless LAN interface is busy and cannot perform the requested operation. |
| 0xC0232002<br>STATUS\_NDIS\_DOT11\_POWER\_STATE\_INVALID | The wireless LAN interface is power down and does not support the requested operation. |
| 0xC0232003<br>STATUS\_NDIS\_PM\_WOL\_PATTERN\_LIST\_FULL | The list of wake on LAN patterns is full. |
| 0xC0232004<br>STATUS\_NDIS\_PM\_PROTOCOL\_OFFLOAD\_LIST\_FULL | The list of low power protocol offloads is full. |
| 0xC0360001<br>STATUS\_IPSEC\_BAD\_SPI | The SPI in the packet does not match a valid IPsec SA. |
| 0xC0360002<br>STATUS\_IPSEC\_SA\_LIFETIME\_EXPIRED | The packet was received on an IPsec SA whose lifetime has expired. |
| 0xC0360003<br>STATUS\_IPSEC\_WRONG\_SA | The packet was received on an IPsec SA that does not match the packet characteristics. |
| 0xC0360004<br>STATUS\_IPSEC\_REPLAY\_CHECK\_FAILED | The packet sequence number replay check failed. |
| 0xC0360005<br>STATUS\_IPSEC\_INVALID\_PACKET | The IPsec header and/or trailer in the packet is invalid. |
| 0xC0360006<br>STATUS\_IPSEC\_INTEGRITY\_CHECK\_FAILED | The IPsec integrity check failed. |
| 0xC0360007<br>STATUS\_IPSEC\_CLEAR\_TEXT\_DROP | IPsec dropped a clear text packet. |
| 0xC0360008<br>STATUS\_IPSEC\_AUTH\_FIREWALL\_DROP | IPsec dropped an incoming ESP packet in authenticated firewall mode. This drop is benign. |
| 0xC0360009<br>STATUS\_IPSEC\_THROTTLE\_DROP | IPsec dropped a packet due to DOS throttle. |
| 0xC0368000<br>STATUS\_IPSEC\_DOSP\_BLOCK | IPsec Dos Protection matched an explicit block rule. |
| 0xC0368001<br>STATUS\_IPSEC\_DOSP\_RECEIVED\_MULTICAST | IPsec Dos Protection received an IPsec specific multicast packet which is not allowed. |
| 0xC0368002<br>STATUS\_IPSEC\_DOSP\_INVALID\_PACKET | IPsec Dos Protection received an incorrectly formatted packet. |
| 0xC0368003<br>STATUS\_IPSEC\_DOSP\_STATE\_LOOKUP\_FAILED | IPsec Dos Protection failed to lookup state. |
| 0xC0368004<br>STATUS\_IPSEC\_DOSP\_MAX\_ENTRIES | IPsec Dos Protection failed to create state because there are already maximum number of entries allowed by policy. |
| 0xC0368005<br>STATUS\_IPSEC\_DOSP\_KEYMOD\_NOT\_ALLOWED | IPsec Dos Protection received an IPsec negotiation packet for a keying module which is not allowed by policy. |
| 0xC0368006<br>STATUS\_IPSEC\_DOSP\_MAX\_PER\_IP\_RATELIMIT\_QUEUES | IPsec Dos Protection failed to create per internal IP ratelimit queue because there is already maximum number of queues allowed by policy. |
| 0xC038005B<br>STATUS\_VOLMGR\_MIRROR\_NOT\_SUPPORTED | The system does not support mirrored volumes. |
| 0xC038005C<br>STATUS\_VOLMGR\_RAID5\_NOT\_SUPPORTED | The system does not support RAID-5 volumes. |
| 0xC03A0014<br>STATUS\_VIRTDISK\_PROVIDER\_NOT\_FOUND | A virtual disk support provider for the specified file was not found. |
| 0xC03A0015<br>STATUS\_VIRTDISK\_NOT\_VIRTUAL\_DISK | The specified disk is not a virtual disk. |
| 0xC03A0016<br>STATUS\_VHD\_PARENT\_VHD\_ACCESS\_DENIED | The chain of virtual hard disks is inaccessible. The process has not been granted access rights to the parent virtual hard disk for the differencing disk. |
| 0xC03A0017<br>STATUS\_VHD\_CHILD\_PARENT\_SIZE\_MISMATCH | The chain of virtual hard disks is corrupted. There is a mismatch in the virtual sizes of the parent virtual hard disk and differencing disk. |
| 0xC03A0018<br>STATUS\_VHD\_DIFFERENCING\_CHAIN\_CYCLE\_DETECTED | The chain of virtual hard disks is corrupted. A differencing disk is indicated in its own parent chain. |
| 0xC03A0019<br>STATUS\_VHD\_DIFFERENCING\_CHAIN\_ERROR\_IN\_PARENT | The chain of virtual hard disks is inaccessible. There was an error opening a virtual hard disk further up the chain. |
| 0xC05D0000<br>STATUS\_SMB\_NO\_PREAUTH\_INTEGRITY\_HASH\_OVERLAP | Returned in response to a client negotiate request when the server does not support any of the hash algorithms in the request. |
| 0xC05D0001<br>STATUS\_SMB\_BAD\_CLUSTER\_DIALECT | The current cluster functional level does not support this SMB dialect. |
