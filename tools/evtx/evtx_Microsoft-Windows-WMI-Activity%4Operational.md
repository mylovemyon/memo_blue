```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f WMI-Activity%4Operational.json C:\Windows\System32\winevt\Logs\Microsoft-Windows-WMI-Activity%4Operational.evtx
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
         FROM read_json('.\WMI-Activity%4Operational.json')
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         GROUP BY ALL
         ORDER BY ALL;
┌─────────┬──────────────┐
│ EventID │ count_star() │
│  int64  │    int64     │
├─────────┼──────────────┤
│    5857 │        80435 │
│    5858 │        48269 │
│    5859 │           96 │
│    5860 │          529 │
│    5861 │           99 │
└─────────┴──────────────┘
```



## 5857 (WMI wmiprvse execution)
```sql
memory D SELECT Event.System.Computer, Event.UserData.Operation_StartedOperational.HostProcess, Event.UserData.Operation_StartedOperational.ProviderName, Event.UserData.Operation_StartedOperational.ProviderPath, Event.UserData.Operation_StartedOperational.Code, COUNT(*)
         FROM read_json('.\WMI-Activity%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 5857
         GROUP BY ALL
         ORDER BY Event.UserData.Operation_StartedOperational.HostProcess, Event.UserData.Operation_StartedOperational.ProviderName;
┌─────────────────────┬──────────────┬───────────────────────────────────┬─────────────────────────────────────────────┬─────────┬──────────────┐
│      Computer       │ HostProcess  │           ProviderName            │                ProviderPath                 │  Code   │ count_star() │
│       varchar       │   varchar    │              varchar              │                   varchar                   │ varchar │    int64     │
├─────────────────────┼──────────────┼───────────────────────────────────┼─────────────────────────────────────────────┼─────────┼──────────────┤
│ rd01.shieldbase.com │ wmiprvse.exe │ CIMWin32                          │ %systemroot%\system32\wbem\cimwin32.dll     │ 0x0     │            4 │
│ rd01.shieldbase.com │ wmiprvse.exe │ CIMWin32a                         │ %systemroot%\system32\wbem\wmipcima.dll     │ 0x0     │          225 │
│ rd01.shieldbase.com │ wmiprvse.exe │ DeliveryOptimizationMIProv        │ %SystemRoot%\system32\domgmt.dll            │ 0x0     │           25 │
│ rd01.shieldbase.com │ wmiprvse.exe │ MSVDS__PROVIDER                   │ %systemroot%\system32\wbem\vdswmi.dll       │ 0x0     │            1 │
│ rd01.shieldbase.com │ wmiprvse.exe │ MSVSS__PROVIDER                   │ %systemroot%\system32\wbem\vsswmi.dll       │ 0x0     │           10 │
│ rd01.shieldbase.com │ wmiprvse.exe │ MS_NT_EVENTLOG_PROVIDER           │ %systemroot%\system32\wbem\ntevt.dll        │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ PowerWmiProvider                  │ %systemroot%\system32\umpowmi.dll           │ 0x0     │            1 │
│ rd01.shieldbase.com │ wmiprvse.exe │ RegProv                           │ %systemroot%\system32\wbem\stdprov.dll      │ 0x0     │            1 │
│ rd01.shieldbase.com │ wmiprvse.exe │ RouteProvider                     │ %systemroot%\system32\wbem\wmipiprt.dll     │ 0x0     │            4 │
│ rd01.shieldbase.com │ wmiprvse.exe │ StorageWMI                        │ %SystemRoot%\System32\storagewmi.dll        │ 0x0     │            1 │
│ rd01.shieldbase.com │ wmiprvse.exe │ UserProfileProvider               │ %systemroot%\system32\profprov.dll          │ 0x0     │           25 │
│ rd01.shieldbase.com │ wmiprvse.exe │ WMI Kernel Trace Event Provider   │ C:\Windows\System32\wbem\krnlprov.dll       │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ WMIProv                           │ %systemroot%\system32\wbem\wmiprov.dll      │ 0x0     │           30 │
│ rd01.shieldbase.com │ wmiprvse.exe │ WhqlProvider                      │ %systemroot%\system32\signdrv.dll           │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ Win32_DeviceGuard                 │ %SystemRoot%\System32\Win32_DeviceGuard.dll │ 0x0     │           25 │
│ rd01.shieldbase.com │ wmiprvse.exe │ Win32_TpmProvider                 │ C:\Windows\System32\wbem\Win32_TPM.dll      │ 0x0     │           25 │
│ rd01.shieldbase.com │ wmiprvse.exe │ Win32_WIN32_TERMINALSERVICE_Prov  │ %SystemRoot%\system32\tscfgwmi.dll          │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ Win32_WIN32_TSGENERALSETTING_Prov │ %SystemRoot%\system32\tscfgwmi.dll          │ 0x0     │            5 │
│ rd01.shieldbase.com │ wmiprvse.exe │ WmiPerfClass                      │ C:\Windows\System32\wbem\WmiPerfClass.dll   │ 0x0     │           29 │
│ rd01.shieldbase.com │ wmiprvse.exe │ smbwmiv2                          │ %SystemRoot%\System32\smbwmiv2.dll          │ 0x0     │            1 │
└─────────────────────┴──────────────┴───────────────────────────────────┴─────────────────────────────────────────────┴─────────┴──────────────┘
  20 rows                                                                                                                             6 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\WMI-Activity%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '5857' AND je.fullkey LIKE '$.Event.UserData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌─────────────────────────────────────────────────────────────────┬──────────────┐
│                             fullkey                             │ count_star() │
│                             varchar                             │    int64     │
├─────────────────────────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.Operation_StartedOperational                   │         2718 │
│ $.Event.UserData.Operation_StartedOperational.#attributes       │         2718 │
│ $.Event.UserData.Operation_StartedOperational.#attributes.xmlns │         2718 │
│ $.Event.UserData.Operation_StartedOperational.Code              │         2718 │
│ $.Event.UserData.Operation_StartedOperational.HostProcess       │         2718 │
│ $.Event.UserData.Operation_StartedOperational.ProcessID         │         2718 │
│ $.Event.UserData.Operation_StartedOperational.ProviderName      │         2718 │
│ $.Event.UserData.Operation_StartedOperational.ProviderPath      │         2718 │
└─────────────────────────────────────────────────────────────────┴──────────────┘
```



## 5858 (WMI Query Error)
```sql
memory D SELECT Event.System.Computer, Event.UserData.Operation_ClientFailure.ClientMachine, Event.UserData.Operation_ClientFailure.User, Event.UserData.Operation_ClientFailure.Operation, Event.UserData.Operation_ClientFailure.ResultCode, Event.UserData.Operation_ClientFailure.Component, Event.UserData.Operation_ClientFailure.PossibleCause, COUNT(*)
         FROM read_json('.\WMI-Activity%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 5858 AND Event.UserData.Operation_ClientFailure.User != 'NT AUTHORITY\SYSTEM'
         GROUP BY ALL
         ORDER BY Event.UserData.Operation_ClientFailure.User, Event.UserData.Operation_ClientFailure.Operation, Event.UserData.Operation_ClientFailure.ResultCode;
┌─────────────────────┬───────────────┬──────────────────────────────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬────────────┬───────────┬───────────────┬──────────────┐
│      Computer       │ ClientMachine │             User             │                                                   Operation                                                    │ ResultCode │ Component │ PossibleCause │ count_star() │
│       varchar       │    varchar    │           varchar            │                                                    varchar                                                     │  varchar   │  varchar  │    varchar    │    int64     │
├─────────────────────┼───────────────┼──────────────────────────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼────────────┼───────────┼───────────────┼──────────────┤
│ rd01.shieldbase.com │ RD01          │ NT AUTHORITY\NETWORK SERVICE │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT Manufacturer, Product FROM Win32_BaseBoard                │ 0x80041032 │ Unknown   │ Unknown       │            8 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\rsydow-a          │ Start IWbemServices::CreateInstanceEnum - root\cimv2 : Win32_ComputerSystem                                    │ 0x80041032 │ Unknown   │ Unknown       │            1 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\tdungan           │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT Family,VirtualizationFirmwareEnabled FROM Win32_Processor │ 0x80041032 │ Unknown   │ Unknown       │            3 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\tdungan           │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT HypervisorPresent FROM Win32_ComputerSystem               │ 0x80041032 │ Unknown   │ Unknown       │           10 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\tdungan           │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT Manufacturer, Model FROM Win32_ComputerSystem             │ 0x80041032 │ Unknown   │ Unknown       │            3 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\tdungan           │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT Manufacturer,Product FROM Win32_BaseBoard                 │ 0x80041032 │ Unknown   │ Unknown       │            3 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\tdungan           │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT Manufacturer,Version FROM Win32_BIOS                      │ 0x80041032 │ Unknown   │ Unknown       │            9 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\wacsvc            │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT * FROM Win32_ComputerSystemProduct                        │ 0x80041032 │ Unknown   │ Unknown       │            1 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\wacsvc            │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT * FROM Win32_OperatingSystem                              │ 0x80041032 │ Unknown   │ Unknown       │            1 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\wacsvc            │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT * FROM Win32_PhysicalMemory WHERE Tag='Physical Memory 0' │ 0x80041032 │ Unknown   │ Unknown       │            1 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\wacsvc            │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT Family,VirtualizationFirmwareEnabled FROM Win32_Processor │ 0x80041032 │ Unknown   │ Unknown       │            1 │
│ rd01.shieldbase.com │ RD01          │ shieldbase\wacsvc            │ Start IWbemServices::ExecQuery - ROOT\CIMV2 : SELECT HypervisorPresent FROM Win32_ComputerSystem               │ 0x80041032 │ Unknown   │ Unknown       │            1 │
└─────────────────────┴───────────────┴──────────────────────────────┴────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴────────────┴───────────┴───────────────┴──────────────┘
  12 rows                                                                                                                                                                                                                           8 columns
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\WMI-Activity%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '5858' AND je.fullkey LIKE '$.Event.UserData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌────────────────────────────────────────────────────────────┬──────────────┐
│                          fullkey                           │ count_star() │
│                          varchar                           │    int64     │
├────────────────────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.Operation_ClientFailure                   │         3836 │
│ $.Event.UserData.Operation_ClientFailure.#attributes       │         3836 │
│ $.Event.UserData.Operation_ClientFailure.#attributes.xmlns │         3836 │
│ $.Event.UserData.Operation_ClientFailure.ClientMachine     │         3836 │
│ $.Event.UserData.Operation_ClientFailure.ClientProcessId   │         3836 │
│ $.Event.UserData.Operation_ClientFailure.Component         │         3836 │
│ $.Event.UserData.Operation_ClientFailure.Id                │         3836 │
│ $.Event.UserData.Operation_ClientFailure.Operation         │         3836 │
│ $.Event.UserData.Operation_ClientFailure.PossibleCause     │         3836 │
│ $.Event.UserData.Operation_ClientFailure.ResultCode        │         3836 │
│ $.Event.UserData.Operation_ClientFailure.User              │         3836 │
└────────────────────────────────────────────────────────────┴──────────────┘
  11 rows                                                         2 columns
```
ResultCode
```
0x80041001: WBEM_E_FAILED - Call failed.
0x80041002: WBEM_E_NOT_FOUND - Object cannot be found.
0x80041003: WBEM_E_ACCESS_DENIED - Current user does not have permission to perform the action.
0x80041004: WBEM_E_PROVIDER_FAILURE - Provider has failed at some time other than during initialization.
0x80041005: WBEM_E_TYPE_MISMATCH - Type mismatch occurred.
0x80041006: WBEM_E_OUT_OF_MEMORY - Not enough memory for the operation.
0x80041007: WBEM_E_INVALID_CONTEXT - The IWbemContext object is not valid.
0x80041008: WBEM_E_INVALID_PARAMETER - One of the parameters to the call is not correct.
0x80041009: WBEM_E_NOT_AVAILABLE - Resource, typically a remote server, is not currently available.
0x8004100A: WBEM_E_CRITICAL_ERROR - Internal, critical, and unexpected error occurred. Report the error to Microsoft Technical Support.
0x8004100B: WBEM_E_INVALID_STREAM - One or more network packets were corrupted during a remote session.
0x8004100C: WBEM_E_NOT_SUPPORTED - Feature or operation is not supported.
0x8004100D: WBEM_E_INVALID_SUPERCLASS - Parent class specified is not valid.
0x8004100E: WBEM_E_INVALID_NAMESPACE - Namespace specified cannot be found.
0x8004100F: WBEM_E_INVALID_OBJECT - Specified instance is not valid.
0x80041010: WBEM_E_INVALID_CLASS - Specified class is not valid.
0x80041011: WBEM_E_PROVIDER_NOT_FOUND - Provider referenced in the schema does not have a corresponding registration.
0x80041012: WBEM_E_INVALID_PROVIDER_REGISTRATION - Provider referenced in the schema has an incorrect or incomplete registration.
0x80041013: WBEM_E_PROVIDER_LOAD_FAILURE - COM cannot locate a provider referenced in the schema.
0x80041014: WBEM_E_INITIALIZATION_FAILURE - Component, such as a provider, failed to initialize for internal reasons.
0x80041015: WBEM_E_TRANSPORT_FAILURE - Networking error that prevents normal operation has occurred.
0x80041016: WBEM_E_INVALID_OPERATION - Requested operation is not valid. This error usually applies to invalid attempts to delete classes or properties.
0x80041017: WBEM_E_INVALID_QUERY - Query was not syntactically valid.
0x80041018: WBEM_E_INVALID_QUERY_TYPE - Requested query language is not supported.
0x80041019: WBEM_E_ALREADY_EXISTS - In a put operation, the wbemChangeFlagCreateOnly flag was specified, but the instance already exists.
0x8004101A: WBEM_E_OVERRIDE_NOT_ALLOWED - Not possible to perform the add operation on this qualifier because the owning object does not permit overrides.
0x8004101B: WBEM_E_PROPAGATED_QUALIFIER - User attempted to delete a qualifier that was not owned. The qualifier was inherited from a parent class.
0x8004101C: WBEM_E_PROPAGATED_PROPERTY - User attempted to delete a property that was not owned. The property was inherited from a parent class.
0x8004101D: WBEM_E_UNEXPECTED - Client made an unexpected and illegal sequence of calls, such as calling EndEnumeration before calling BeginEnumeration.
0x8004101E: WBEM_E_ILLEGAL_OPERATION - User requested an illegal operation, such as spawning a class from an instance.
0x8004101F: WBEM_E_CANNOT_BE_KEY - Illegal attempt to specify a key qualifier on a property that cannot be a key. The keys are specified in the class definition for an object and cannot be altered on a per-instance basis.
0x80041020: WBEM_E_INCOMPLETE_CLASS - Current object is not a valid class definition. Either it is incomplete or it has not been registered with WMI using SWbemObject.Put_.
0x80041021: WBEM_E_INVALID_SYNTAX - Query is syntactically not valid.
0x80041022: WBEM_E_NONDECORATED_OBJECT - Reserved for future use.
0x80041023: WBEM_E_READ_ONLY - An attempt was made to modify a read-only property.
0x80041024: WBEM_E_PROVIDER_NOT_CAPABLE - Provider cannot perform the requested operation. This can include a query that is too complex, retrieving an instance, creating or updating a class, deleting a class, or enumerating a class.
0x80041025: WBEM_E_CLASS_HAS_CHILDREN - Attempt was made to make a change that invalidates a subclass.
0x80041026: WBEM_E_CLASS_HAS_INSTANCES - Attempt was made to delete or modify a class that has instances.
0x80041027: WBEM_E_QUERY_NOT_IMPLEMENTED - Reserved for future use.
0x80041028: WBEM_E_ILLEGAL_NULL - Value of Nothing/NULL was specified for a property that must have a value, such as one that is marked by a Key, Indexed, or Not_Null qualifier.
0x80041029: WBEM_E_INVALID_QUALIFIER_TYPE - Variant value for a qualifier was provided that is not a legal qualifier type.
0x8004102A: WBEM_E_INVALID_PROPERTY_TYPE - CIM type specified for a property is not valid.
0x8004102B: WBEM_E_VALUE_OUT_OF_RANGE - Request was made with an out-of-range value or it is incompatible with the type.
0x8004102C: WBEM_E_CANNOT_BE_SINGLETON - Illegal attempt was made to make a class singleton, such as when the class is derived from a non-singleton class.
0x8004102D: WBEM_E_INVALID_CIM_TYPE - CIM type specified is not valid.
0x8004102E: WBEM_E_INVALID_METHOD - Requested method is not available.
0x8004102F: WBEM_E_INVALID_METHOD_PARAMETERS - Parameters provided for the method are not valid.
0x80041030: WBEM_E_SYSTEM_PROPERTY - There was an attempt to get qualifiers on a system property.
0x80041031: WBEM_E_INVALID_PROPERTY - Property type is not recognized.
0x80041032: WBEM_E_CALL_CANCELLED - Asynchronous process has been canceled internally or by the user. Note that due to the timing and nature of the asynchronous operation, the operation may not have been truly canceled.
0x80041033: WBEM_E_SHUTTING_DOWN - User has requested an operation while WMI is in the process of shutting down.
0x80041034: WBEM_E_PROPAGATED_METHOD - Attempt was made to reuse an existing method name from a parent class and the signatures do not match.
0x80041035: WBEM_E_UNSUPPORTED_PARAMETER - One or more parameter values, such as a query text, is too complex or unsupported. WMI is therefore requested to retry the operation with simpler parameters.
0x80041036: WBEM_E_MISSING_PARAMETER_ID - Parameter was missing from the method call.
0x80041037: WBEM_E_INVALID_PARAMETER_ID - Method parameter has an ID qualifier that is not valid.
0x80041038: WBEM_E_NONCONSECUTIVE_PARAMETER_IDS - One or more of the method parameters have ID qualifiers that are out of sequence.
0x80041039: WBEM_E_PARAMETER_ID_ON_RETVAL - Return value for a method has an ID qualifier.
0x8004103A: WBEM_E_INVALID_OBJECT_PATH - Specified object path was not valid.
0x8004103B: WBEM_E_OUT_OF_DISK_SPACE - Disk is out of space or the 4 GB limit on WMI repository (CIM repository) size is reached.
0x8004103C: WBEM_E_BUFFER_TOO_SMALL - Supplied buffer was too small to hold all of the objects in the enumerator or to read a string property.
0x8004103D: WBEM_E_UNSUPPORTED_PUT_EXTENSION - Provider does not support the requested put operation.
0x8004103E: WBEM_E_UNKNOWN_OBJECT_TYPE - Object with an incorrect type or version was encountered during marshaling.
0x8004103F: WBEM_E_UNKNOWN_PACKET_TYPE - Packet with an incorrect type or version was encountered during marshaling.
0x80041040: WBEM_E_MARSHAL_VERSION_MISMATCH - Packet has an unsupported version.
0x80041041: WBEM_E_MARSHAL_INVALID_SIGNATURE - Packet appears to be corrupt.
0x80041042: WBEM_E_INVALID_QUALIFIER - Attempt was made to mismatch qualifiers, such as putting [key] on an object instead of a property.
0x80041043: WBEM_E_INVALID_DUPLICATE_PARAMETER - Duplicate parameter was declared in a CIM method.
0x80041044: WBEM_E_TOO_MUCH_DATA - Reserved for future use.
0x80041045: WBEM_E_SERVER_TOO_BUSY - Call to IWbemObjectSink::Indicate has failed. The provider can refire the event.
0x80041046: WBEM_E_INVALID_FLAVOR - Specified qualifier flavor was not valid.
0x80041047: WBEM_E_CIRCULAR_REFERENCE - Attempt was made to create a reference that is circular (for example, deriving a class from itself).
0x80041048: WBEM_E_UNSUPPORTED_CLASS_UPDATE - Specified class is not supported.
0x80041049: WBEM_E_CANNOT_CHANGE_KEY_INHERITANCE - Attempt was made to change a key when instances or subclasses are already using the key.
0x80041050: WBEM_E_CANNOT_CHANGE_INDEX_INHERITANCE - An attempt was made to change an index when instances or subclasses are already using the index.
0x80041051: WBEM_E_TOO_MANY_PROPERTIES - Attempt was made to create more properties than the current version of the class supports.
0x80041052: WBEM_E_UPDATE_TYPE_MISMATCH - Property was redefined with a conflicting type in a derived class.
0x80041053: WBEM_E_UPDATE_OVERRIDE_NOT_ALLOWED - Attempt was made in a derived class to override a qualifier that cannot be overridden.
0x80041054: WBEM_E_UPDATE_PROPAGATED_METHOD - Method was re-declared with a conflicting signature in a derived class.
0x80041055: WBEM_E_METHOD_NOT_IMPLEMENTED - Attempt was made to execute a method not marked with [implemented] in any relevant class.
0x80041056: WBEM_E_METHOD_DISABLED - Attempt was made to execute a method marked with [disabled].
0x80041057: WBEM_E_REFRESHER_BUSY - Refresher is busy with another operation.
0x80041058: WBEM_E_UNPARSABLE_QUERY - Filtering query is syntactically not valid.
0x80041059: WBEM_E_NOT_EVENT_CLASS - The FROM clause of a filtering query references a class that is not an event class (not derived from __Event).
0x8004105A: WBEM_E_MISSING_GROUP_WITHIN - A GROUP BY clause was used without the corresponding GROUP WITHIN clause.
0x8004105B: WBEM_E_MISSING_AGGREGATION_LIST - A GROUP BY clause was used. Aggregation on all properties is not supported.
0x8004105C: WBEM_E_PROPERTY_NOT_AN_OBJECT - Dot notation was used on a property that is not an embedded object.
0x8004105D: WBEM_E_AGGREGATING_BY_OBJECT - A GROUP BY clause references a property that is an embedded object without using dot notation.
0x8004105F: WBEM_E_UNINTERPRETABLE_PROVIDER_QUERY - Event provider registration query (__EventProviderRegistration) did not specify the classes for which events were provided.
0x80041060: WBEM_E_BACKUP_RESTORE_WINMGMT_RUNNING - Request was made to back up or restore the repository while it was in use by WinMgmt.exe, or by the SVCHOST process that contains the WMI service.
0x80041061: WBEM_E_QUEUE_OVERFLOW - Asynchronous delivery queue overflowed from the event consumer being too slow.
0x80041062: WBEM_E_PRIVILEGE_NOT_HELD - Operation failed because the client did not have the necessary security privilege.
0x80041063: WBEM_E_INVALID_OPERATOR - Operator is not valid for this property type.
0x80041064: WBEM_E_LOCAL_CREDENTIALS - User specified a username/password/authority on a local connection. The user must use a blank username/password and rely on default security.
0x80041065: WBEM_E_CANNOT_BE_ABSTRACT - Class was made abstract when its parent class is not abstract.
0x80041066: WBEM_E_AMENDED_OBJECT - Amended object was written without the WBEM_FLAG_USE_AMENDED_QUALIFIERS flag being specified.
0x80041067: WBEM_E_CLIENT_TOO_SLOW - Client did not retrieve objects quickly enough from an enumeration. This constant is returned when a client creates an enumeration object, but does not retrieve objects from the enumerator in a timely fashion, causing the enumerator's object caches to back up.
0x80041068: WBEM_E_NULL_SECURITY_DESCRIPTOR - Null security descriptor was used.
0x80041069: WBEM_E_TIMED_OUT - Operation timed out.
0x8004106A: WBEM_E_INVALID_ASSOCIATION - Association is not valid.
0x8004106B: WBEM_E_AMBIGUOUS_OPERATION - Operation was ambiguous.
0x8004106C: WBEM_E_QUOTA_VIOLATION - WMI is taking up too much memory. This can be caused by low memory availability or excessive memory consumption by WMI.
0x8004106D: WBEM_E_TRANSACTION_CONFLICT - Operation resulted in a transaction conflict.
0x8004106E: WBEM_E_FORCED_ROLLBACK - Transaction forced a rollback.
0x8004106F: WBEM_E_UNSUPPORTED_LOCALE - Locale used in the call is not supported.
0x80041070: WBEM_E_HANDLE_OUT_OF_DATE - Object handle is out-of-date.
0x80041071: WBEM_E_CONNECTION_FAILED - Connection to the SQL database failed.
0x80041072: WBEM_E_INVALID_HANDLE_REQUEST - Handle request was not valid.
0x80041073: WBEM_E_PROPERTY_NAME_TOO_WIDE - Property name contains more than 255 characters.
0x80041074: WBEM_E_CLASS_NAME_TOO_WIDE - Class name contains more than 255 characters.
0x80041075: WBEM_E_METHOD_NAME_TOO_WIDE - Method name contains more than 255 characters.
0x80041076: WBEM_E_QUALIFIER_NAME_TOO_WIDE - Qualifier name contains more than 255 characters.
0x80041077: WBEM_E_RERUN_COMMAND - The SQL command must be rerun because there is a deadlock in SQL. This can be returned only when data is being stored in an SQL database.
0x80041078: WBEM_E_DATABASE_VER_MISMATCH - The database version does not match the version that the repository driver processes.
0x80041079: WBEM_E_VETO_DELETE - WMI cannot execute the delete operation because the provider does not allow it.
0x8004107A: WBEM_E_VETO_PUT - WMI cannot execute the put operation because the provider does not allow it.
0x80041080: WBEM_E_INVALID_LOCALE - Specified locale identifier was not valid for the operation.
0x80041081: WBEM_E_PROVIDER_SUSPENDED - Provider is suspended.
0x80041082: WBEM_E_SYNCHRONIZATION_REQUIRED - Object must be written to the WMI repository and retrieved again before the requested operation can succeed. This constant is returned when an object must be committed and retrieved to see the property value.
0x80041083: WBEM_E_NO_SCHEMA - Operation cannot be completed; no schema is available.
0x119FD010: WBEM_E_PROVIDER_ALREADY_REGISTERED - Provider cannot be registered because it is already registered.
0x80041085: WBEM_E_PROVIDER_NOT_REGISTERED - Provider was not registered.
0x80041086: WBEM_E_FATAL_TRANSPORT_ERROR - A fatal transport error occurred.
0x80041087: WBEM_E_ENCRYPTED_CONNECTION_REQUIRED - User attempted to set a computer name or domain without an encrypted connection.
0x80041088: WBEM_E_PROVIDER_TIMED_OUT - A provider failed to report results within the specified timeout.
0x80041089: WBEM_E_NO_KEY - User attempted to put an instance with no defined key.
0x8004108A: WBEM_E_PROVIDER_DISABLED - User attempted to register a provider instance but the COM server for the provider instance was unloaded.
0x80042001: WBEMESS_E_REGISTRATION_TOO_BROAD - Provider registration overlaps with the system event domain.
0x80042002: WBEMESS_E_REGISTRATION_TOO_PRECISE - A WITHIN clause was not used in this query.
0x80042003: WBEMESS_E_AUTHZ_NOT_PRIVILEGED - This computer does not have the necessary domain permissions to support the security functions that relate to the created subscription instance. Contact the Domain Administrator to get this computer added to the Windows Authorization Access Group.
0x80043001: WBEM_E_RETRY_LATER - Reserved for future use.
0x80043002: WBEM_E_RESOURCE_CONTENTION - Reserved for future use.
0x80044001: WBEMMOF_E_EXPECTED_QUALIFIER_NAME - Expected a qualifier name.
0x80044002: WBEMMOF_E_EXPECTED_SEMI - Expected semicolon or '='.
0x80044003: WBEMMOF_E_EXPECTED_OPEN_BRACE - Expected an opening brace.
0x80044004: WBEMMOF_E_EXPECTED_CLOSE_BRACE - Missing closing brace or an illegal array element.
0x80044005: WBEMMOF_E_EXPECTED_CLOSE_BRACKET - Expected a closing bracket.
0x80044006: WBEMMOF_E_EXPECTED_CLOSE_PAREN - Expected closing parenthesis.
0x80044007: WBEMMOF_E_ILLEGAL_CONSTANT_VALUE - Numeric value out of range or strings without quotes.
0x80044008: WBEMMOF_E_EXPECTED_TYPE_IDENTIFIER - Expected a type identifier.
0x80044009: WBEMMOF_E_EXPECTED_OPEN_PAREN - Expected an open parenthesis.
0x8004400A: WBEMMOF_E_UNRECOGNIZED_TOKEN - Unexpected token in the file.
0x8004400B: WBEMMOF_E_UNRECOGNIZED_TYPE - Unrecognized or unsupported type identifier.
0x8004400C: WBEMMOF_E_EXPECTED_PROPERTY_NAME - Expected property or method name.
0x8004400D: WBEMMOF_E_TYPEDEF_NOT_SUPPORTED - Typedefs and enumerated types are not supported.
0x8004400E: WBEMMOF_E_UNEXPECTED_ALIAS - Only a reference to a class object can have an alias value.
0x8004400F: WBEMMOF_E_UNEXPECTED_ARRAY_INIT - Unexpected array initialization. Arrays must be declared with [].
0x80044010: WBEMMOF_E_INVALID_AMENDMENT_SYNTAX - Namespace path syntax is not valid.
0x80044011: WBEMMOF_E_INVALID_DUPLICATE_AMENDMENT - Duplicate amendment specifiers.
0x80044012: WBEMMOF_E_INVALID_PRAGMA - pragma must be followed by a valid keyword.
0x80044013: WBEMMOF_E_INVALID_NAMESPACE_SYNTAX - Namespace path syntax is not valid.
0x80044014: WBEMMOF_E_EXPECTED_CLASS_NAME - Unexpected character in class name must be an identifier.
0x80044015: WBEMMOF_E_TYPE_MISMATCH - The value specified cannot be made into the appropriate type.
0x80044016: WBEMMOF_E_EXPECTED_ALIAS_NAME - Dollar sign must be followed by an alias name as an identifier.
0x80044017: WBEMMOF_E_INVALID_CLASS_DECLARATION - Class declaration is not valid.
0x80044018: WBEMMOF_E_INVALID_INSTANCE_DECLARATION - The instance declaration is not valid. It must start with "instance of"
0x80044019: WBEMMOF_E_EXPECTED_DOLLAR - Expected dollar sign. An alias in the form "$name" must follow the "as" keyword.
0x8004401A: WBEMMOF_E_CIMTYPE_QUALIFIER - "CIMTYPE" qualifier cannot be specified directly in a MOF file. Use standard type notation.
0x8004401B: WBEMMOF_E_DUPLICATE_PROPERTY - Duplicate property name was found in the MOF.
0x8004401C: WBEMMOF_E_INVALID_NAMESPACE_SPECIFICATION - Namespace syntax is not valid. References to other servers are not allowed.
0x8004401D: WBEMMOF_E_OUT_OF_RANGE - Value out of range.
0x8004401E: WBEMMOF_E_INVALID_FILE - The file is not a valid text MOF file or binary MOF file.
0x8004401F: WBEMMOF_E_ALIASES_IN_EMBEDDED - Embedded objects cannot be aliases.
0x80044020: WBEMMOF_E_NULL_ARRAY_ELEM - NULL elements in an array are not supported.
0x80044021: WBEMMOF_E_DUPLICATE_QUALIFIER - Qualifier was used more than once on the object.
0x80044022: WBEMMOF_E_EXPECTED_FLAVOR_TYPE - Expected a flavor type such as ToInstance, ToSubClass, EnableOverride, or DisableOverride.
0x80044023: WBEMMOF_E_INCOMPATIBLE_FLAVOR_TYPES - Combining EnableOverride and DisableOverride on same qualifier is not legal.
0x80044024: WBEMMOF_E_MULTIPLE_ALIASES - An alias cannot be used twice.
0x80044025: WBEMMOF_E_INCOMPATIBLE_FLAVOR_TYPES2 - Combining Restricted, and ToInstance or ToSubClass is not legal.
0x80044026: WBEMMOF_E_NO_ARRAYS_RETURNED - Methods cannot return array values.
0x80044027: WBEMMOF_E_MUST_BE_IN_OR_OUT - Arguments must have an In or Out qualifier.
0x80044028: WBEMMOF_E_INVALID_FLAGS_SYNTAX - Flags syntax is not valid.
0x80044029: WBEMMOF_E_EXPECTED_BRACE_OR_BAD_TYPE - The final brace and semi-colon for a class are missing.
0x8004402A: WBEMMOF_E_UNSUPPORTED_CIMV22_QUAL_VALUE - A CIM version 2.2 feature is not supported for a qualifier value.
0x8004402B: WBEMMOF_E_UNSUPPORTED_CIMV22_DATA_TYPE - The CIM version 2.2 data type is not supported.
0x8004402C: WBEMMOF_E_INVALID_DELETEINSTANCE_SYNTAX - The delete instance syntax is not valid. It should be pragma DeleteInstance("instancepath", FAIL|NOFAIL)
0x8004402D: WBEMMOF_E_INVALID_QUALIFIER_SYNTAX - The qualifier syntax is not valid. It should be qualifiername:type=value,scope(class|instance), flavorname.
0x8004402E: WBEMMOF_E_QUALIFIER_USED_OUTSIDE_SCOPE - The qualifier is used outside of its scope.
0x8004402F: WBEMMOF_E_ERROR_CREATING_TEMP_FILE - Error creating temporary file. The temporary file is an intermediate stage in the MOF compilation.
0x80044030: WBEMMOF_E_ERROR_INVALID_INCLUDE_FILE - A file included in the MOF by the preprocessor command include is not valid.
0x80044031: WBEMMOF_E_INVALID_DELETECLASS_SYNTAX - The syntax for the preprocessor commands pragma deleteinstance or pragma deleteclass is not valid.
```



## 5860 (WMI Registration of Temporary Event Consumer)
```sql
memory D SELECT Event.System.Computer, Event.UserData.Operation_TemporaryEssStarted.ClientMachine, Event.UserData.Operation_TemporaryEssStarted.User, Event.UserData.Operation_TemporaryEssStarted.NamespaceName, Event.UserData.Operation_TemporaryEssStarted.Query, Event.UserData.Operation_TemporaryEssStarted.PossibleCause, COUNT(*)
         FROM read_json('.\WMI-Activity%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 5860
         GROUP BY ALL
         ORDER BY Event.UserData.Operation_TemporaryEssStarted.User, Event.UserData.Operation_TemporaryEssStarted.NamespaceName, Event.UserData.Operation_TemporaryEssStarted.Query;
┌─────────────────────┬───────────────┬────────────────────────────┬─────────────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬───────────────┬──────────────┐
│      Computer       │ ClientMachine │            User            │    NamespaceName    │                                                                                 Query                                                                                 │ PossibleCause │ count_star() │
│       varchar       │    varchar    │          varchar           │       varchar       │                                                                                varchar                                                                                │    varchar    │    int64     │
├─────────────────────┼───────────────┼────────────────────────────┼─────────────────────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼───────────────┼──────────────┤
│ rd01.shieldbase.com │ RD01          │ NT AUTHORITY\LOCAL SERVICE │ ROOT\SecurityCenter │ SELECT * FROM __InstanceOperationEvent WHERE TargetInstance ISA 'AntiVirusProduct' OR TargetInstance ISA 'FirewallProduct' OR TargetInstance ISA 'AntiSpywareProduct' │ Temporary     │            4 │
│ rd01.shieldbase.com │ RD01          │ NT AUTHORITY\SYSTEM        │ ROOT\CIMV2          │ SELECT * FROM Win32_ProcessStartTrace WHERE ProcessName = 'wsmprovhost.exe'                                                                                           │ Temporary     │            5 │
│ rd01.shieldbase.com │ RD01          │ NT AUTHORITY\SYSTEM        │ root\CIMV2          │ select * from __InstanceCreationEvent within 3 where TargetInstance ISA 'Win32_LogicalDisk'                                                                           │ Temporary     │            4 │
└─────────────────────┴───────────────┴────────────────────────────┴─────────────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴───────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\WMI-Activity%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '5860' AND je.fullkey LIKE '$.Event.UserData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌──────────────────────────────────────────────────────────────────┬──────────────┐
│                             fullkey                              │ count_star() │
│                             varchar                              │    int64     │
├──────────────────────────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.Operation_TemporaryEssStarted                   │         1005 │
│ $.Event.UserData.Operation_TemporaryEssStarted.#attributes       │         1005 │
│ $.Event.UserData.Operation_TemporaryEssStarted.#attributes.xmlns │         1005 │
│ $.Event.UserData.Operation_TemporaryEssStarted.ClientMachine     │         1005 │
│ $.Event.UserData.Operation_TemporaryEssStarted.NamespaceName     │         1005 │
│ $.Event.UserData.Operation_TemporaryEssStarted.PossibleCause     │         1005 │
│ $.Event.UserData.Operation_TemporaryEssStarted.Processid         │         1005 │
│ $.Event.UserData.Operation_TemporaryEssStarted.Query             │         1005 │
│ $.Event.UserData.Operation_TemporaryEssStarted.User              │         1005 │
└──────────────────────────────────────────────────────────────────┴──────────────┘
```



## 5861 (WMI Registration of Permanent Event Consumer)
```sql
memory D SELECT Event.System.Computer, Event.UserData.Operation_ESStoConsumerBinding.ESS, Event.UserData.Operation_ESStoConsumerBinding.Namespace, Event.UserData.Operation_ESStoConsumerBinding.CONSUMER, Event.UserData.Operation_ESStoConsumerBinding.PossibleCause, COUNT(*)
         FROM read_json('.\WMI-Activity%4Operational-rd01.json', sample_size = -1, map_inference_threshold = -1)
         WHERE Event.System.TimeCreated."#attributes".SystemTime >= TIMESTAMP '2023-01-01 00:00:00' AND Event.System.TimeCreated."#attributes".SystemTime < TIMESTAMP '2023-02-01 00:00:00'
         AND Event.System.EventID = 5861
         GROUP BY ALL
         ORDER BY Event.UserData.Operation_ESStoConsumerBinding.ESS, Event.UserData.Operation_ESStoConsumerBinding.Namespace, Event.UserData.Operation_ESStoConsumerBinding.CONSUMER;
┌─────────────────────┬──────────────────────┬───────────────────────┬──────────────────────────────────────────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┬──────────────┐
│      Computer       │         ESS          │       Namespace       │                     CONSUMER                     │                                                                                                                                                                                                                                                                            PossibleCause                                                                                                                                                                                                                                                                             │ count_star() │
│       varchar       │       varchar        │        varchar        │                     varchar                      │                                                                                                                                                                                                                                                                               varchar                                                                                                                                                                                                                                                                                │    int64     │
├─────────────────────┼──────────────────────┼───────────────────────┼──────────────────────────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼──────────────┤
│ rd01.shieldbase.com │ SCM Event Log Filter │ //./root/subscription │ NTEventLogEventConsumer="SCM Event Log Consumer" │ Binding EventFilter: \ninstance of __EventFilter\n{\n\tCreatorSID = {1, 2, 0, 0, 0, 0, 0, 5, 32, 0, 0, 0, 32, 2, 0, 0};\n\tEventNamespace = "root\\cimv2";\n\tName = "SCM Event Log Filter";\n\tQuery = "select * from MSFT_SCMEventLogEvent";\n\tQueryLanguage = "WQL";\n};\nPerm. Consumer: \ninstance of NTEventLogEventConsumer\n{\n\tCategory = 0;\n\tCreatorSID = {1, 2, 0, 0, 0, 0, 0, 5, 32, 0, 0, 0, 32, 2, 0, 0};\n\tEventType = 1;\n\tName = "SCM Event Log Consumer";\n\tNameOfUserSIDProperty = "sid";\n\tSourceName = "Service Control Manager";\n};\n │            4 │
└─────────────────────┴──────────────────────┴───────────────────────┴──────────────────────────────────────────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┴──────────────┘
```
fullkey
```sql
memory D SELECT je.fullkey, COUNT(*)
         FROM read_json_objects('.\WMI-Activity%4Operational-rd01.json') AS e, json_tree(e.json) AS je
         WHERE e.json->>'$.Event.System.EventID' = '5861' AND je.fullkey LIKE '$.Event.UserData.%'
         GROUP BY je.fullkey
         ORDER BY je.fullkey;
┌───────────────────────────────────────────────────────────────────┬──────────────┐
│                              fullkey                              │ count_star() │
│                              varchar                              │    int64     │
├───────────────────────────────────────────────────────────────────┼──────────────┤
│ $.Event.UserData.Operation_ESStoConsumerBinding                   │           15 │
│ $.Event.UserData.Operation_ESStoConsumerBinding.#attributes       │           15 │
│ $.Event.UserData.Operation_ESStoConsumerBinding.#attributes.xmlns │           15 │
│ $.Event.UserData.Operation_ESStoConsumerBinding.CONSUMER          │           15 │
│ $.Event.UserData.Operation_ESStoConsumerBinding.ESS               │           15 │
│ $.Event.UserData.Operation_ESStoConsumerBinding.Namespace         │           15 │
│ $.Event.UserData.Operation_ESStoConsumerBinding.PossibleCause     │           15 │
└───────────────────────────────────────────────────────────────────┴──────────────┘
```
