## 2.2.1.2.4 SMB_FILE_ATTRIBUTES
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/2198f480-e047-4df0-ba64-f28eadef00b9  
An unsigned 16-bit field that defines the basic file attributes supported by the SMB Protocol. In addition, exclusive search attributes (those Names prefixed with SMB\_SEARCH\_ATTRIBUTE) are defined for use when searching for files within a directory.

| Name and bitmask | Description |
| --- | --- |
| SMB\_FILE\_ATTRIBUTE\_NORMAL<br>0x0000 | Normal file. |
| SMB\_FILE\_ATTRIBUTE\_READONLY<br>0x0001 | Read-only file. |
| SMB\_FILE\_ATTRIBUTE\_HIDDEN<br>0x0002 | Hidden file. |
| SMB\_FILE\_ATTRIBUTE\_SYSTEM<br>0x0004 | System file. |
| SMB\_FILE\_ATTRIBUTE\_VOLUME<br>0x0008 | Volume Label. |
| SMB\_FILE\_ATTRIBUTE\_DIRECTORY<br>0x0010 | Directory file. |
| SMB\_FILE\_ATTRIBUTE\_ARCHIVE<br>0x0020 | File changed since last archive. |
| SMB\_SEARCH\_ATTRIBUTE\_READONLY<br>0x0100 | Search for Read-only files. |
| SMB\_SEARCH\_ATTRIBUTE\_HIDDEN<br>0x0200 | Search for Hidden files. |
| SMB\_SEARCH\_ATTRIBUTE\_SYSTEM<br>0x0400 | Search for System files. |
| SMB\_SEARCH\_ATTRIBUTE\_DIRECTORY<br>0x1000 | Search for Directory files. |
| SMB\_SEARCH\_ATTRIBUTE\_ARCHIVE<br>0x2000 | Search for files that have changed since they were last archived. |
| Other<br>0xC8C0 | Reserved. |



## 2.2.2.1 SMB_COM Command Codes
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/32b5d4b7-d90b-483f-ad6a-003fd110f0ec  
Description: A short description of the command. If a code or code range is marked as Reserved, this field lists its intended use.  
Status: Current status of the command's usage (Deprecated, Obsolescent, or Obsolete) as used in this context.
- C = Currently used
- D = Deprecated
- O = Obsolescent
- X = Obsolete
- N = Not implemented - The command code was reserved and in some cases documented, but the command was never implemented.

| NT LAN Manager name and pre-NT LAN Manager name | Code | Description | Status | Earliest dialect |
| --- | --- | --- | --- | --- |
| SMB\_COM\_CREATE\_DIRECTORY<br>SMBmkdir | 0x00 | Create a new directory. | D | CORE |
| SMB\_COM\_DELETE\_DIRECTORY<br>SMBrmdir | 0x01 | Delete an empty directory. | C | CORE |
| SMB\_COM\_OPEN<br>SMBopen | 0x02 | Open a file. | D | CORE |
| SMB\_COM\_CREATE<br>SMBcreate | 0x03 | Create or open a file. | D | CORE |
| SMB\_COM\_CLOSE<br>SMBclose | 0x04 | Close a file. | C | CORE |
| SMB\_COM\_FLUSH<br>SMBflush | 0x05 | Flush data for a file, or all files associated with a client, PID pair. | C | CORE |
| SMB\_COM\_DELETE<br>SMBunlink | 0x06 | Delete a file. | C | CORE |
| SMB\_COM\_RENAME<br>SMBmv | 0x07 | Rename a file or set of files. | C | CORE |
| SMB\_COM\_QUERY\_INFORMATION<br>SMBgetattr | 0x08 | Get file attributes. | D | CORE |
| SMB\_COM\_SET\_INFORMATION<br>SMBsetattr | 0x09 | Set file attributes. | D | CORE |
| SMB\_COM\_READ<br>SMBread | 0x0A | Read from a file. | D | CORE |
| SMB\_COM\_WRITE<br>SMBwrite | 0x0B | Write to a file. | D | CORE |
| SMB\_COM\_LOCK\_BYTE\_RANGE<br>SMBlock | 0x0C | Request a byte-range lock on a file. | D | CORE |
| SMB\_COM\_UNLOCK\_BYTE\_RANGE<br>SMBunlock | 0x0D | Release a byte-range lock on a file. | D | CORE |
| SMB\_COM\_CREATE\_TEMPORARY<br>SMBctemp | 0x0E | Create a temporary file. | O | CORE |
| SMB\_COM\_CREATE\_NEW<br>SMBmknew | 0x0F | Create and open a new file. | D | CORE |
| SMB\_COM\_CHECK\_DIRECTORY<br>SMBchkpth | 0x10 | Verify that the specified pathname resolves to a directory.<br>Listed as SMBchkpath in some documentation. | C | CORE |
| SMB\_COM\_PROCESS\_EXIT<br>SMBexit | 0x11 | Indicate process exit. | O | CORE |
| SMB\_COM\_SEEK<br>SMBlseek | 0x12 | Set the current file pointer within a file. | O | CORE |
| SMB\_COM\_LOCK\_AND\_READ<br>SMBlockread | 0x13 | Lock and read a byte-range within a file. | D | CorePlus |
| SMB\_COM\_WRITE\_AND\_UNLOCK<br>SMBwriteunlock | 0x14 | Write and unlock a byte-range within a file. | D | CorePlus |
| **Unused** | 0x15<br>...<br>0x19 |  |  |  |
| SMB\_COM\_READ\_RAW<br>SMBreadBraw | 0x1A | Read a block in raw mode. | D | CorePlus |
| SMB\_COM\_READ\_MPX<br>SMBreadBmpx | 0x1B | Multiplexed block read.<br>Listed as SMBreadmpx in some documentation. | O | LANMAN1.0 |
| SMB\_COM\_READ\_MPX\_SECONDARY<br>SMBreadBs | 0x1C | Multiplexed block read, secondary request. | X | LANMAN1.0 |
| SMB\_COM\_WRITE\_RAW<br>SMBwriteBraw | 0x1D | Write a block in raw mode. | D | CorePlus |
| SMB\_COM\_WRITE\_MPX<br>SMBwriteBmpx | 0x1E | Multiplexed block write. | O | LANMAN1.0 |
| SMB\_COM\_WRITE\_MPX\_SECONDARY<br>SMBwriteBs | 0x1F | Multiplexed block write, secondary request. | X | LANMAN1.0 |
| SMB\_COM\_WRITE\_COMPLETE<br>SMBwriteC | 0x20 | Raw block write, final response. | D | LANMAN1.0 |
| SMB\_COM\_QUERY\_SERVER | 0x21 | Reserved, but not implemented.<br>Also known as SMB\_COM\_QUERY\_INFORMATION\_SRV. | N |  |
| SMB\_COM\_SET\_INFORMATION2<br>SMBsetattrE | 0x22 | Set an extended set of file attributes. | D | LANMAN1.0 |
| SMB\_COM\_QUERY\_INFORMATION2<br>SMBgetattrE | 0x23 | Get an extended set of file attributes. | D | LANMAN1.0 |
| SMB\_COM\_LOCKING\_ANDX<br>SMBlockingX | 0x24 | Lock multiple byte ranges; AndX chaining. | C | LANMAN1.0 |
| SMB\_COM\_TRANSACTION<br>SMBtrans | 0x25 | Transaction. | C | LANMAN1.0 |
| SMB\_COM\_TRANSACTION\_SECONDARY<br>SMBtranss | 0x26 | Transaction secondary request. | C | LANMAN1.0 |
| SMB\_COM\_IOCTL<br>SMBioctl | 0x27 | Pass an I/O Control function request to the server. | O | LANMAN1.0 |
| SMB\_COM\_IOCTL\_SECONDARY<br>SMBioctls | 0x28 | IOCTL secondary request. | N | LANMAN1.0 |
| SMB\_COM\_COPY<br>SMBcopy | 0x29 | Copy a file or directory. | X | LANMAN1.0 |
| SMB\_COM\_MOVE<br>SMBmove | 0x2A | Move a file or directory. | X | LANMAN1.0 |
| SMB\_COM\_ECHO<br>SMBecho | 0x2B | Echo request (ping). | C | LANMAN1.0 |
| SMB\_COM\_WRITE\_AND\_CLOSE<br>SMBwriteclose | 0x2C | Write to and close a file. | D | LANMAN1.0 |
| SMB\_COM\_OPEN\_ANDX<br>SMBopenX | 0x2D | Extended file open with AndX chaining. | D | LANMAN1.0 |
| SMB\_COM\_READ\_ANDX<br>SMBreadX | 0x2E | Extended file read with AndX chaining. | C | LANMAN1.0 |
| SMB\_COM\_WRITE\_ANDX<br>SMBwriteX | 0x2F | Extended file write with AndX chaining. | C | LANMAN1.0 |
| SMB\_COM\_NEW\_FILE\_SIZE | 0x30 | Reserved, but not implemented.<br>Also known as SMB\_COM\_SET\_NEW\_SIZE. | N |  |
| SMB\_COM\_CLOSE\_AND\_TREE\_DISC | 0x31 | Close an open file and tree disconnect. | N | NT LANMAN |
| [SMB_COM_TRANSACTION2<br>SMBtrans2 | 0x32 | Transaction 2 format request/response. | C | LANMAN1.2 |
| [SMB_COM_TRANSACTION2_SECONDARY<br>SMBtranss2 | 0x33 | Transaction 2 secondary request. | C | LANMAN1.2 |
| [SMB_COM_FIND_CLOSE2<br>SMBfindclose | 0x34 | Close an active search. | C | LANMAN1.2 |
| [SMB_COM_FIND_NOTIFY_CLOSE<br>SMBfindnclose | 0x35 | Notification of the closure of an active search. | N | LANMAN1.2 |
| **Unused** | 0x36<br>...<br>0x5F |  |  |  |
| **Reserved** | 0x60<br>...<br>0x6F | This range of codes was reserved for use by the "xenix1.1" dialect of SMB. See [\[MSFT-XEXTNP\]](https://go.microsoft.com/fwlink/?LinkId=162042). [\[XOPEN-SMB\]](https://go.microsoft.com/fwlink/?linkid=2297696) page 41 lists this range as "Reserved for proprietary dialects." | X | XENIX |
| [SMB_COM_TREE_CONNECT<br>SMBtcon | 0x70 | Tree connect. | D | CORE |
| [SMB_COM_TREE_DISCONNECT<br>SMBtdis | 0x71 | Tree disconnect. | C | CORE |
| [SMB_COM_NEGOTIATE<br>SMBnegprot | 0x72 | Negotiate protocol dialect. | C | CORE |
| [SMB_COM_SESSION_SETUP_ANDX<br>SMBsesssetupX | 0x73 | Session Setup with AndX chaining. | C | LANMAN1.0 |
| [SMB_COM_LOGOFF_ANDX<br>SMBulogoffX | 0x74 | User logoff with AndX chaining. | C | LANMAN1.2 |
| [SMB_COM_TREE_CONNECT_ANDX<br>SMBtconX | 0x75 | Tree connect with AndX chaining. | C | LANMAN1.0 |
| **Unused** | 0x76<br>...<br>0x7D |  |  |  |
| [SMB_COM_SECURITY_PACKAGE_ANDX<br>SMBsecpkgX | 0x7E | Negotiate security packages with AndX chaining. | X | LANMAN1.0 |
| **Unused** | 0x7F |  |  |  |
| [SMB_COM_QUERY_INFORMATION_DISK<br>SMBdskattr | 0x80 | Retrieve file system information from the server. | D | CORE |
| [SMB_COM_SEARCH<br>SMBsearch | 0x81 | Directory wildcard search. | D | CORE |
| [SMB_COM_FIND<br>SMBffirst | 0x82 | Start or continue an extended wildcard directory search. | D | LANMAN1.0 |
| [SMB_COM_FIND_UNIQUE<br>SMBfunique | 0x83 | Perform a one-time extended wildcard directory search. | D | LANMAN1.0 |
| [SMB_COM_FIND_CLOSE<br>SMBfclose | 0x84 | End an extended wildcard directory search. | D | LANMAN1.0 |
| **Unused** | 0x85<br>...<br>0x9F |  |  |  |
| [SMB_COM_NT_TRANSACT | 0xA0 | NT format transaction request/response. | C | NT LANMAN |
| [SMB_COM_NT_TRANSACT_SECONDARY | 0xA1 | NT format transaction secondary request. | C | NT LANMAN |
| [SMB_COM_NT_CREATE_ANDX | 0xA2 | Create or open a file or a directory. | C | NT LANMAN |
| **Unused** | 0xA3 |  |  |  |
| [SMB_COM_NT_CANCEL | 0xA4 | Cancel a request currently pending at the server. | C | NT LANMAN |
| [SMB_COM_NT_RENAME | 0xA5 | File rename with extended semantics. | O | NT LANMAN |
| **Unused** | 0xA6<br>...<br>0xBF |  |  |  |
| [SMB_COM_OPEN_PRINT_FILE<br>SMBsplopen | 0xC0 | Create a print queue spool file. | C | CORE |
| [SMB_COM_WRITE_PRINT_FILE<br>SMBsplwr | 0xC1 | Write to a print queue spool file. | D | CORE |
| [SMB_COM_CLOSE_PRINT_FILE<br>SMBsplclose | 0xC2 | Close a print queue spool file. | D | CORE |
| [SMB_COM_GET_PRINT_QUEUE<br>SMBsplretq | 0xC3 | Request print queue information. | X | CORE |
| **Unused** | 0xC4<br>...<br>0xCF |  |  |  |
| **Reserved** | 0xD0<br>...<br>0xD7 | Messenger Service command codes.<br>This range is reserved for use by the SMB Messenger Service. | O | CORE |
| [SMB_COM_READ_BULK | 0xD8 | Reserved, but not implemented. | N |  |
| [SMB_COM_WRITE_BULK | 0xD9 | Reserved, but not implemented. | N |  |
| [SMB_COM_WRITE_BULK_DATA | 0xDA | Reserved, but not implemented. | N |  |
| **Unused** | 0xDB<br>...<br>0xFD |  |  |  |
| [SMB_COM_INVALID<br>SMBinvalid | 0xFE | As the name suggests, this command code is a designated invalid command and SHOULD NOT be used. | C | LANMAN1.0 |
| [SMB_COM_NO_ANDX_COMMAND | 0xFF | Also known as the "NIL" command. It identifies the end of an AndX Chain, and is only valid in that context. | C | LANMAN1.0 |



## 2.2.2.4 SMB Error Classes and Codes
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/8f11e0f3-d545-46cc-97e6-f00569e3e1bc

**SUCCESS Class 0x00**

| Error code | NTSTATUS values | POSIX equivalent | Description |
| --- | --- | --- | --- |
| SUCCESS<br>0x0000 | STATUS\_OK | 0 | Everything worked, no problems. |

**ERRDOS Class 0x01**

| Error code | NTSTATUS values | POSIX equivalent | Description |
| --- | --- | --- | --- |
| ERRbadfunc<br>0x0001 | STATUS\_NOT\_IMPLEMENTED<br>0xC0000002<br>STATUS\_INVALID\_DEVICE\_REQUEST<br>0xC0000010<br>STATUS\_ILLEGAL\_FUNCTION<br>0xC00000AF | EINVAL | Invalid Function. |
| ERRbadfile<br>0x0002 | STATUS\_NO\_SUCH\_FILE<br>0xC000000F<br>STATUS\_NO\_SUCH\_DEVICE<br>0xC000000E<br>STATUS\_OBJECT\_NAME\_NOT\_FOUND<br>0xC0000034 | ENOENT | File not found. |
| ERRbadpath<br>0x0003 | STATUS\_OBJECT\_PATH\_INVALID<br>0xC0000039<br>STATUS\_OBJECT\_PATH\_NOT\_FOUND<br>0xC000003A<br>STATUS\_OBJECT\_PATH\_SYNTAX\_BAD<br>0xC000003B<br>STATUS\_DFS\_EXIT\_PATH\_FOUND<br>0xC000009B<br>STATUS\_REDIRECTOR\_NOT\_STARTED<br>0xC00000FB | ENOENT | A component in the path prefix is not a directory. |
| ERRnofids<br>0x0004 | STATUS\_TOO\_MANY\_OPENED\_FILES<br>0xC000011F | EMFILE | Too many open files. No FIDs are available. |
| ERRnoaccess<br>0x0005 | STATUS\_ACCESS\_DENIED<br>0xC0000022<br>STATUS\_INVALID\_LOCK\_SEQUENCE<br>0xC000001E<br>STATUS\_INVALID\_VIEW\_SIZE<br>0xC000001F<br>STATUS\_ALREADY\_COMMITTED<br>0xC0000021<br>STATUS\_PORT\_CONNECTION\_REFUSED<br>0xC0000041<br>STATUS\_THREAD\_IS\_TERMINATING<br>0xC000004B<br>STATUS\_DELETE\_PENDING<br>0xC0000056<br>STATUS\_PRIVILEGE\_NOT\_HELD<br>0xC0000061<br>STATUS\_LOGON\_FAILURE<br>0xC000006D<br>STATUS\_FILE\_IS\_A\_DIRECTORY<br>0xC00000BA<br>STATUS\_FILE\_RENAMED<br>0xC00000D5<br>STATUS\_PROCESS\_IS\_TERMINATING<br>0xC000010A<br>STATUS\_DIRECTORY\_NOT\_EMPTY<br>0xC0000101<br>STATUS\_CANNOT\_DELETE<br>0xC0000121<br>STATUS\_FILE\_DELETED<br>0xC0000123 | EPERM | Access denied. |
| ERRbadfid<br>0x0006 | STATUS\_SMB\_BAD\_FID<br>0x00060001<br>STATUS\_INVALID\_HANDLE<br>0xC0000008<br>STATUS\_OBJECT\_TYPE\_MISMATCH<br>0xC0000024<br>STATUS\_PORT\_DISCONNECTED<br>0xC0000037<br>STATUS\_INVALID\_PORT\_HANDLE<br>0xC0000042<br>STATUS\_FILE\_CLOSED<br>0xC0000128<br>STATUS\_HANDLE\_NOT\_CLOSABLE<br>0xC0000235 | EBADF | Invalid FID. |
| ERRbadmcb<br>0x0007 |  |  | Memory Control Blocks were destroyed. |
| ERRnomem<br>0x0008 | STATUS\_SECTION\_TOO\_BIG<br>0xC0000040<br>STATUS\_TOO\_MANY\_PAGING\_FILES<br>0xC0000097<br>STATUS\_INSUFF\_SERVER\_RESOURCES<br>0xC0000205 | ENOMEM | Insufficient server memory to perform the requested operation. |
| ERRbadmem<br>0x0009 |  | EFAULT | The server performed an invalid memory access (invalid address). |
| ERRbadenv<br>0x000A |  |  | Invalid environment. |
| ERRbadformat<br>0x000B |  |  | Invalid format. |
| ERRbadaccess<br>0x000C | STATUS\_OS2\_INVALID\_ACCESS<br>0x000C0001<br>STATUS\_ACCESS\_DENIED<br>0xC00000CA |  | Invalid open mode. |
| ERRbaddata<br>0x000D | STATUS\_DATA\_ERROR<br>0xC000009C | E2BIG | Bad data. (May be generated by IOCTL calls on the server.) |
| ERRbaddrive<br>0x000F |  | ENXIO | Invalid drive specified. |
| ERRremcd<br>0x0010 | STATUS\_DIRECTORY\_NOT\_EMPTY<br>0xC0000101 |  | Remove of directory failed because it was not empty. |
| ERRdiffdevice<br>0x0011 | STATUS\_NOT\_SAME\_DEVICE<br>0xC00000D4 | EXDEV | A file system operation (such as a rename) across two devices was attempted. |
| ERRnofiles<br>0x0012 | STATUS\_NO\_MORE\_FILES<br>0x80000006 |  | No (more) files found following a file search command. |
| ERRgeneral<br>0x001F | STATUS\_UNSUCCESSFUL<br>0xC0000001 |  | General error. |
| ERRbadshare<br>0x0020 | STATUS\_SHARING\_VIOLATION<br>0xC0000043 | ETXTBSY | Sharing violation. A requested open mode conflicts with the sharing mode of an existing file handle. |
| ERRlock<br>0x0021 | STATUS\_FILE\_LOCK\_CONFLICT<br>0xC0000054<br>STATUS\_LOCK\_NOT\_GRANTED<br>0xC0000055 | EDEADLOCK | A lock request specified an invalid locking mode, or conflicted with an existing file lock. |
| ERReof<br>0x0026 | STATUS\_END\_OF\_FILE<br>0xC0000011 | EEOF | Attempted to read beyond the end of the file. |
| ERRunsup<br>0x0032 | STATUS\_NOT\_SUPPORTED<br>0XC00000BB |  | This command is not supported by the server. |
| ERRfilexists<br>0x0050 | STATUS\_OBJECT\_NAME\_COLLISION<br>0xC0000035 | EEXIST | An attempt to create a file or directory failed because an object with the same pathname already exists. |
| ERRinvalidparam<br>0x0057 | STATUS\_INVALID\_PARAMETER<br>0xC000000D |  | A parameter supplied with the message is invalid. |
| ERRunknownlevel<br>0x007C | STATUS\_OS2\_INVALID\_LEVEL<br>0x007C0001 |  | Invalid information level. |
| ERRinvalidseek<br>0x0083 | STATUS\_OS2\_NEGATIVE\_SEEK<br>0x00830001 |  | An attempt was made to seek to a negative absolute offset within a file. |
| ERROR\_NOT\_LOCKED<br>0x009E | STATUS\_RANGE\_NOT\_LOCKED<br>0xC000007E |  | The byte range specified in an unlock request was not locked. |
| ERROR\_NO\_MORE\_SEARCH\_HANDLES<br>0x0071 | STATUS\_OS2\_NO\_MORE\_SIDS<br>0x00710001 |  | Maximum number of searches has been exhausted. |
| ERROR\_CANCEL\_VIOLATION<br>0x00AD | STATUS\_OS2\_CANCEL\_VIOLATION<br>0x00AD0001 |  | No lock request was outstanding for the supplied cancel region. |
| ERROR\_ATOMIC\_LOCKS\_NOT\_SUPPORTED<br>0x00AE | STATUS\_OS2\_ATOMIC\_LOCKS\_NOT\_SUPPORTED<br>0x00AE0001 |  | The file system does not support atomic changes to the lock type. |
| ERRbadpipe<br>0x00E6 | STATUS\_INVALID\_INFO\_CLASS<br>0xC0000003<br>STATUS\_INVALID\_PIPE\_STATE<br>0xC00000AD<br>STATUS\_INVALID\_READ\_MODE<br>0xC00000B4 |  | Invalid named pipe. |
| ERROR\_CANNOT\_COPY<br>0x010A | STATUS\_OS2\_CANNOT\_COPY<br>0x010A0001 |  | The copy functions cannot be used. |
| ERRpipebusy<br>0x00E7 | STATUS\_INSTANCE\_NOT\_AVAILABLE<br>0xC00000AB<br>STATUS\_PIPE\_NOT\_AVAILABLE<br>0xC00000AC<br>STATUS\_PIPE\_BUSY<br>0xC00000AE |  | All instances of the designated named pipe are busy. |
| ERRpipeclosing<br>0x00E8 | STATUS\_PIPE\_CLOSING<br>0xC00000B1<br>STATUS\_PIPE\_EMPTY<br>0xC00000D9 |  | The designated named pipe is in the process of being closed. |
| ERRnotconnected<br>0x00E9 | STATUS\_PIPE\_DISCONNECTED<br>0xC00000B0 |  | The designated named pipe exists, but there is no server process listening on the server side. |
| ERRmoredata<br>0x00EA | STATUS\_BUFFER\_OVERFLOW<br>0x80000005<br>STATUS\_MORE\_PROCESSING\_REQUIRED<br>0xC0000016 |  | There is more data available to read on the designated named pipe. |
| ERRbadealist<br>0x00FF |  |  | Inconsistent extended attribute list. |
| ERROR\_EAS\_<br>DIDNT\_FIT<br>0x0113 | STATUS\_EA\_TOO\_LARGE<br>0xC0000050<br>STATUS\_OS2\_EAS\_DIDNT\_FIT<br>0x01130001 |  | Either there are no extended attributes, or the available extended attributes did not fit into the response. |
| ERROR\_EAS\_<br>NOT\_SUPPORTED<br>0x011A | STATUS\_EAS\_NOT\_SUPPORTED<br>0xC000004F |  | The server file system does not support Extended Attributes. |
| ERROR\_EA\_ACCESS\_DENIED<br>0x03E2 | STATUS\_OS2\_EA\_ACCESS\_DENIED<br>0x03E20001 |  | Access to the extended attribute was denied. |
| ERR\_NOTIFY\_ENUM\_DIR<br>0x03FE | STATUS\_NOTIFY\_ENUM\_DIR<br>0x0000010C |  | More changes have occurred within the directory than will fit within the specified Change Notify response buffer. |

**ERRSRV Class 0x02**

| Error code | NTSTATUS values | POSIX equivalent | Description |
| --- | --- | --- | --- |
| ERRerror<br>0x0001 | STATUS\_INVALID\_SMB<br>0x00010002 |  | Unspecified server error. |
| ERRbadpw<br>0x0002 | STATUS\_WRONG\_PASSWORD<br>0xC000006A |  | Invalid password. |
| ERRbadpath<br>0x0003 | STATUS\_PATH\_NOT\_COVERED<br>0xC0000257 |  | DFS pathname not on local server. |
| ERRaccess<br>0x0004 | STATUS\_NETWORK\_ACCESS\_DENIED<br>0xC00000CA | EACCES | Access denied. The specified UID does not have permission to execute the requested command within the current context (TID). |
| ERRinvtid<br>0x0005 | STATUS\_NETWORK\_NAME\_DELETED<br>0xC00000C9<br>STATUS\_SMB\_BAD\_TID<br>0x00050002 |  | The TID specified in the command was invalid.<br>Earlier documentation, with the exception of [\[SNIA\]](https://go.microsoft.com/fwlink/?LinkId=90519), refers to this error code as ERRinvnid (Invalid Network Path Identifier). [SNIA] uses both names. |
| ERRinvnetname<br>0x0006 | STATUS\_BAD\_NETWORK\_NAME<br>0xC00000CC |  | Invalid server name in Tree Connect. |
| ERRinvdevice<br>0x0007 | STATUS\_BAD\_DEVICE\_TYPE<br>0xC00000CB |  | A printer request was made to a non-printer device or, conversely, a non-printer request was made to a printer device. |
| ERRinvsess<br>0x0010 |  |  | Invalid Connection ID (CID). This error code is only defined when the Direct IPX connectionless transport is in use. |
| ERRworking<br>0x0011 |  |  | A command with matching MID or SequenceNumber is currently being processed. This error code is defined only when the Direct IPX connectionless transport is in use. |
| ERRnotme<br>0x0012 |  |  | Incorrect NetBIOS Called Name when starting an SMB session over Direct IPX. This error code is only defined when the Direct IPX connectionless transport is in use. |
| ERRbadcmd<br>0x0016 | STATUS\_SMB\_BAD\_COMMAND<br>0x00160002 |  | An unknown SMB command code was received by the server. |
| ERRqfull<br>0x0031 | STATUS\_PRINT\_QUEUE\_FULL<br>0xC00000C6 |  | Print queue is full - too many queued items. |
| ERRqtoobig<br>0x0032 | STATUS\_NO\_SPOOL\_SPACE<br>0xC00000C7 |  | Print queue is full - no space for queued item, or queued item too big. |
| ERRqeof<br>0x0033 |  |  | End Of File on print queue dump. |
| ERRinvpfid<br>0x0034 | STATUS\_PRINT\_CANCELLED<br>0xC00000C8 |  | Invalid FID for print file. |
| ERRsmbcmd<br>0x0040 | STATUS\_NOT\_IMPLEMENTED<br>0xC0000002 |  | Unrecognized SMB command code. |
| ERRsrverror<br>0x0041 | STATUS\_UNEXPECTED\_NETWORK\_ERROR<br>0xC00000C4 |  | Internal server error. |
| ERRfilespecs<br>0x0043 |  |  | The FID and pathname contain incompatible values. |
| ERRbadpermits<br>0x0045 | STATUS\_NETWORK\_ACCESS\_DENIED<br>0xC00000CA |  | An invalid combination of access permissions for a file or directory was presented. The server cannot set the requested attributes. |
| ERRsetattrmode<br>0x0047 |  |  | The attribute mode presented in a set mode request was invalid. |
| ERRtimeout<br>0x0058 | STATUS\_UNEXPECTED\_NETWORK\_ERROR<br>0xC00000C4<br>STATUS\_IO\_TIMEOUT<br>0xC00000B5 |  | Operation timed out. |
| ERRnoresource<br>0x0059 | STATUS\_REQUEST\_NOT\_ACCEPTED<br>0xC00000D0 |  | No resources currently available for this SMB request. |
| ERRtoomanyuids<br>0x005A | STATUS\_TOO\_MANY\_SESSIONS<br>0xC00000CE |  | Too many UIDs active for this SMB connection. |
| ERRbaduid<br>0x005B | STATUS\_SMB\_BAD\_UID<br>0x005B0002 |  | The UID specified is not known as a valid ID on this server session. |
| ERRnotconnected<br>0x00E9 | STATUS\_PIPE\_DISCONNECTED<br>0xC00000B0 | EPIPE | Write to a named pipe with no reader. |
| ERRusempx<br>0x00FA | STATUS\_SMB\_USE\_MPX<br>0x00FA0002 |  | Temporarily unable to support RAW mode transfers. Use MPX mode. |
| ERRusestd<br>0x00FB | STATUS\_SMB\_USE\_STANDARD<br>0x00FB0002 |  | Temporarily unable to support RAW or MPX mode transfers. Use standard read/write. |
| ERRcontmpx<br>0x00FC | STATUS\_SMB\_CONTINUE\_MPX<br>0x00FC0002 |  | Continue in MPX mode.<br>This error code is reserved for future use. |
| ERRaccountExpired<br>0x08BF | **STATUS\_ACCOUNT\_DISABLED**<br>0xC0000072<br>STATUS\_ACCOUNT\_EXPIRED<br>0xC0000193 |  | User account on the target machine is disabled or has expired. |
| ERRbadClient<br>0x08C0 | STATUS\_INVALID\_WORKSTATION<br>0xC0000070 |  | The client does not have permission to access this server. |
| ERRbadLogonTime<br>0x08C1 | STATUS\_INVALID\_LOGON\_HOURS<br>0xC000006F |  | Access to the server is not permitted at this time. |
| ERRpasswordExpired<br>0x08C2 | STATUS\_PASSWORD\_EXPIRED<br>0xC0000071<br>STATUS\_PASSWORD\_MUST\_CHANGE<br>0xC0000224 |  | The user's password has expired. |
| ERRnosupport<br>0xFFFF | STATUS\_SMB\_NO\_SUPPORT<br>0XFFFF0002 |  | Function not supported by the server. |

**ERRHRD Class 0x03**

| Error code | NTSTATUS values | POSIX equivalent | Description |
| --- | --- | --- | --- |
| ERRnowrite<br>0x0013 | STATUS\_MEDIA\_WRITE\_PROTECTED<br>0xC00000A2 | EROFS | Attempt to modify a read-only file system. |
| ERRbadunit<br>0x0014 |  | ENODEV | Unknown unit. |
| ERRnotready<br>0x0015 | STATUS\_NO\_MEDIA\_IN\_DEVICE<br>0xC0000013 | EUCLEAN | Drive not ready. |
| ERRbadcmd<br>0x0016 | STATUS\_INVALID\_DEVICE\_STATE<br>0xC0000184 |  | Unknown command. |
| ERRdata<br>0x0017 | STATUS\_DATA\_ERROR<br>0xC000003E<br>STATUS\_CRC\_ERROR<br>0xC000003F | EIO | Data error (incorrect CRC). |
| ERRbadreq<br>0x0018 | STATUS\_DATA\_ERROR<br>0xC000003E | ERANGE | Bad request structure length. |
| ERRseek<br>0x0019 |  |  | Seek error. |
| ERRbadmedia<br>0x001A | STATUS\_DISK\_CORRUPT\_ERROR<br>0xC0000032 |  | Unknown media type. |
| ERRbadsector<br>0x001B | STATUS\_NONEXISTENT\_SECTOR<br>0xC0000015 |  | Sector not found. |
| ERRnopaper<br>0x001C | STATUS\_DEVICE\_PAPER\_EMPTY<br>0x8000000E |  | Printer out of paper. |
| ERRwrite<br>0x001D |  |  | Write fault. |
| ERRread<br>0x001E |  |  | Read fault. |
| ERRgeneral<br>0x001F |  |  | General hardware failure. |
| ERRbadshare<br>0x0020 | STATUS\_SHARING\_VIOLATION<br>0xC0000043 | ETXTBSY | An attempted open operation conflicts with an existing open. |
| ERRlock<br>0x0021 | STATUS\_FILE\_LOCK\_CONFLICT<br>0xC0000054 | EDEADLOCK | A lock request specified an invalid locking mode, or conflicted with an existing file lock. |
| ERRwrongdisk<br>0x0022 | STATUS\_WRONG\_VOLUME<br>0xC0000012 |  | The wrong disk was found in a drive. |
| ERRFCBUnavail<br>0x0023 |  |  | No server-side File Control Blocks are available to process the request. |
| ERRsharebufexc<br>0x0024 |  |  | A sharing buffer has been exceeded. |
| ERRdiskfull<br>0x0027 | STATUS\_DISK\_FULL<br>0xC000007F | ENOSPC | No space on file system. |

**ERRCMD Class 0xFF**

The ERRCMD error class is used to indicate that the server received a command that was not in the SMB format. No error codes are defined for use with the ERRCMD (0XFF) class.
