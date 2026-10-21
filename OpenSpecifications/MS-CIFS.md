## SMB_COM Command Codes 
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
| SMB\_COM\_CHECK\_DIRECTORY<br>SMBchkpth | 0x10 | Verify that the specified pathname resolves to a directory.<br><br>Listed as SMBchkpath in some documentation. | C | CORE |
| SMB\_COM\_PROCESS\_EXIT<br>SMBexit | 0x11 | Indicate process exit. | O | CORE |
| SMB\_COM\_SEEK<br>SMBlseek | 0x12 | Set the current file pointer within a file. | O | CORE |
| SMB\_COM\_LOCK\_AND\_READ<br>SMBlockread | 0x13 | Lock and read a byte-range within a file. | D | CorePlus |
| SMB\_COM\_WRITE\_AND\_UNLOCK<br>SMBwriteunlock | 0x14 | Write and unlock a byte-range within a file. | D | CorePlus |
| **Unused** | 0x15<br><br>...<br><br>0x19 |  |  |  |
| SMB\_COM\_READ\_RAW<br>SMBreadBraw | 0x1A | Read a block in raw mode. | D | CorePlus |
| SMB\_COM\_READ\_MPX<br>SMBreadBmpx | 0x1B | Multiplexed block read.<br><br>Listed as SMBreadmpx in some documentation. | O | LANMAN1.0 |
| SMB\_COM\_READ\_MPX\_SECONDARY<br>SMBreadBs | 0x1C | Multiplexed block read, secondary request. | X | LANMAN1.0 |
| SMB\_COM\_WRITE\_RAW<br>SMBwriteBraw | 0x1D | Write a block in raw mode. | D | CorePlus |
| SMB\_COM\_WRITE\_MPX<br>SMBwriteBmpx | 0x1E | Multiplexed block write. | O | LANMAN1.0 |
| SMB\_COM\_WRITE\_MPX\_SECONDARY<br>SMBwriteBs | 0x1F | Multiplexed block write, secondary request. | X | LANMAN1.0 |
| SMB\_COM\_WRITE\_COMPLETE<br>SMBwriteC | 0x20 | Raw block write, final response. | D | LANMAN1.0 |
| SMB\_COM\_QUERY\_SERVER | 0x21 | Reserved, but not implemented.<br><br>Also known as SMB\_COM\_QUERY\_INFORMATION\_SRV. | N |  |
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
| SMB\_COM\_NEW\_FILE\_SIZE | 0x30 | Reserved, but not implemented.<br><br>Also known as SMB\_COM\_SET\_NEW\_SIZE. | N |  |
| SMB\_COM\_CLOSE\_AND\_TREE\_DISC | 0x31 | Close an open file and tree disconnect. | N | NT LANMAN |
| [SMB_COM_TRANSACTION2<br>SMBtrans2 | 0x32 | Transaction 2 format request/response. | C | LANMAN1.2 |
| [SMB_COM_TRANSACTION2_SECONDARY<br>SMBtranss2 | 0x33 | Transaction 2 secondary request. | C | LANMAN1.2 |
| [SMB_COM_FIND_CLOSE2<br>SMBfindclose | 0x34 | Close an active search. | C | LANMAN1.2 |
| [SMB_COM_FIND_NOTIFY_CLOSE<br>SMBfindnclose | 0x35 | Notification of the closure of an active search. | N | LANMAN1.2 |
| **Unused** | 0x36<br><br>...<br><br>0x5F |  |  |  |
| **Reserved** | 0x60<br><br>...<br><br>0x6F | This range of codes was reserved for use by the "xenix1.1" dialect of SMB. See [\[MSFT-XEXTNP\]](https://go.microsoft.com/fwlink/?LinkId=162042). [\[XOPEN-SMB\]](https://go.microsoft.com/fwlink/?linkid=2297696) page 41 lists this range as "Reserved for proprietary dialects." | X | XENIX |
| [SMB_COM_TREE_CONNECT<br>SMBtcon | 0x70 | Tree connect. | D | CORE |
| [SMB_COM_TREE_DISCONNECT<br>SMBtdis | 0x71 | Tree disconnect. | C | CORE |
| [SMB_COM_NEGOTIATE<br>SMBnegprot | 0x72 | Negotiate protocol dialect. | C | CORE |
| [SMB_COM_SESSION_SETUP_ANDX<br>SMBsesssetupX | 0x73 | Session Setup with AndX chaining. | C | LANMAN1.0 |
| [SMB_COM_LOGOFF_ANDX<br>SMBulogoffX | 0x74 | User logoff with AndX chaining. | C | LANMAN1.2 |
| [SMB_COM_TREE_CONNECT_ANDX<br>SMBtconX | 0x75 | Tree connect with AndX chaining. | C | LANMAN1.0 |
| **Unused** | 0x76<br><br>...<br><br>0x7D |  |  |  |
| [SMB_COM_SECURITY_PACKAGE_ANDX<br>SMBsecpkgX | 0x7E | Negotiate security packages with AndX chaining. | X | LANMAN1.0 |
| **Unused** | 0x7F |  |  |  |
| [SMB_COM_QUERY_INFORMATION_DISK<br>SMBdskattr | 0x80 | Retrieve file system information from the server. | D | CORE |
| [SMB_COM_SEARCH<br>SMBsearch | 0x81 | Directory wildcard search. | D | CORE |
| [SMB_COM_FIND<br>SMBffirst | 0x82 | Start or continue an extended wildcard directory search. | D | LANMAN1.0 |
| [SMB_COM_FIND_UNIQUE<br>SMBfunique | 0x83 | Perform a one-time extended wildcard directory search. | D | LANMAN1.0 |
| [SMB_COM_FIND_CLOSE<br>SMBfclose | 0x84 | End an extended wildcard directory search. | D | LANMAN1.0 |
| **Unused** | 0x85<br><br>...<br><br>0x9F |  |  |  |
| [SMB_COM_NT_TRANSACT | 0xA0 | NT format transaction request/response. | C | NT LANMAN |
| [SMB_COM_NT_TRANSACT_SECONDARY | 0xA1 | NT format transaction secondary request. | C | NT LANMAN |
| [SMB_COM_NT_CREATE_ANDX | 0xA2 | Create or open a file or a directory. | C | NT LANMAN |
| **Unused** | 0xA3 |  |  |  |
| [SMB_COM_NT_CANCEL | 0xA4 | Cancel a request currently pending at the server. | C | NT LANMAN |
| [SMB_COM_NT_RENAME | 0xA5 | File rename with extended semantics. | O | NT LANMAN |
| **Unused** | 0xA6<br><br>...<br><br>0xBF |  |  |  |
| [SMB_COM_OPEN_PRINT_FILE<br>SMBsplopen | 0xC0 | Create a print queue spool file. | C | CORE |
| [SMB_COM_WRITE_PRINT_FILE<br>SMBsplwr | 0xC1 | Write to a print queue spool file. | D | CORE |
| [SMB_COM_CLOSE_PRINT_FILE<br>SMBsplclose | 0xC2 | Close a print queue spool file. | D | CORE |
| [SMB_COM_GET_PRINT_QUEUE<br>SMBsplretq | 0xC3 | Request print queue information. | X | CORE |
| **Unused** | 0xC4<br><br>...<br><br>0xCF |  |  |  |
| **Reserved** | 0xD0<br><br>...<br><br>0xD7 | Messenger Service command codes.<br><br>This range is reserved for use by the SMB Messenger Service. | O | CORE |
| [SMB_COM_READ_BULK | 0xD8 | Reserved, but not implemented. | N |  |
| [SMB_COM_WRITE_BULK | 0xD9 | Reserved, but not implemented. | N |  |
| [SMB_COM_WRITE_BULK_DATA | 0xDA | Reserved, but not implemented. | N |  |
| **Unused** | 0xDB<br><br>...<br><br>0xFD |  |  |  |
| [SMB_COM_INVALID<br>SMBinvalid | 0xFE | As the name suggests, this command code is a designated invalid command and SHOULD NOT be used. | C | LANMAN1.0 |
| [SMB_COM_NO_ANDX_COMMAND | 0xFF | Also known as the "NIL" command. It identifies the end of an AndX Chain, and is only valid in that context. | C | LANMAN1.0 |
