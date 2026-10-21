---
layout: Conceptual
title: '[MS-CIFS]: SMB_COM Command Codes | Microsoft Learn'
canonicalUrl: https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/32b5d4b7-d90b-483f-ad6a-003fd110f0ec
ms.service: openspecs-windows
ROBOTS: INDEX, FOLLOW
uhfHeaderId: MSDocsHeader-OpenSpecs
ms.topic: reference
ms.author: cindyle
protocol_rendering: true
description: Following is a listing of all SMB commands used in CIFS and their associated command codes, as well as additional useful
locale: en-us
document_id: 0963a401-32f2-111f-b1aa-c80817ba8722
document_version_independent_id: 625098b8-77b5-9d7c-ae1d-e078c5af84ad
updated_at: 2025-06-10T18:40:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/open_specs_windows/blob/live/documentation/windows_protocols/MS-CIFS/32b5d4b7-d90b-483f-ad6a-003fd110f0ec.md
gitcommit: https://github.com/MicrosoftDocs/open_specs_windows/blob/0fd9895b75f1fb2627fa7d48862822d6a0b3faea/documentation/windows_protocols/MS-CIFS/32b5d4b7-d90b-483f-ad6a-003fd110f0ec.md
git_commit_id: 0fd9895b75f1fb2627fa7d48862822d6a0b3faea
site_name: Docs
depot_name: MSDN.openspecswindows
page_type: conceptual
toc_rel: toc.json
feedback_system: None
feedback_product_url: ''
feedback_help_link_type: ''
feedback_help_link_url: ''
word_count: 1244
asset_id: windows_protocols/ms-cifs/32b5d4b7-d90b-483f-ad6a-003fd110f0ec
moniker_range_name: 
monikers: []
item_type: Content
source_path: documentation/windows_protocols/MS-CIFS/32b5d4b7-d90b-483f-ad6a-003fd110f0ec.md
cmProducts:
- https://microsoft-devrel.poolparty.biz/DevRelOfferingOntology/12ed19f9-ebdf-4c8a-8bcd-7a681836774d
- https://authoring-docs-microsoft.poolparty.biz/devrel/540ac133-a371-4dbb-8f94-28d6cc77a70b
spProducts:
- https://microsoft-devrel.poolparty.biz/DevRelOfferingOntology/3a764584-4f97-452b-8f1d-36f19b12f6ae
- https://authoring-docs-microsoft.poolparty.biz/devrel/60bfc045-f127-4841-9d00-ea35495a5800
platformId: 67eac5d3-8712-2b90-8e6a-e89bfa7fa22f
---

# [MS-CIFS]: SMB_COM Command Codes | Microsoft Learn

Following is a listing of all [SMB](760f8b7f-9a8a-4f0c-a044-1501a83a933b#gt_09dbec39-5e75-4d9a-babf-1c9f1d499625) commands used in [CIFS](760f8b7f-9a8a-4f0c-a044-1501a83a933b#gt_fb37399e-d72d-41b6-b073-086da2d96e09) and their associated command codes, as well as additional useful information. The table reads as follows:

**NT LAN Manager name and pre-NT LAN Manager name:** Current name of command and alternate name used in older documentation, if available. If a code or code range is marked **Unused**, it is undefined and reserved for future use. If a code or code range is marked **Reserved**, it is or was reserved for a specific purpose. Both of these indicate that client implementations SHOULD NOT send messages using any of those command codes.

**Code:** An SMB command code.

**Description:** A short description of the command. If a code or code range is marked as Reserved, this field lists its intended use.

**Status:** Current status of the command's usage ([Deprecated](760f8b7f-9a8a-4f0c-a044-1501a83a933b#gt_129acedf-ccf1-4e13-8df7-3bc59eff6f6d), [Obsolescent](760f8b7f-9a8a-4f0c-a044-1501a83a933b#gt_8377951d-81ca-44a2-af83-2a6d6388c121), or [Obsolete](760f8b7f-9a8a-4f0c-a044-1501a83a933b#gt_73fb9153-adab-4f17-b472-426f982e360c)) as used in this context.

- C = Currently used
- D = Deprecated
- O = Obsolescent
- X = Obsolete
- N = Not implemented - The command code was reserved and in some cases documented, but the command was never implemented.

**Earliest dialect:** Earliest known dialect in which this command appears.

| NT LAN Manager name and pre-NT LAN Manager name | Code | Description | Status | Earliest dialect |
| --- | --- | --- | --- | --- |
| SMB\_COM\_CREATE\_DIRECTORY (section [2.2.4.1](e6e870ad-7037-4b79-ac54-4a42a1ba4561))<br><br>SMBmkdir | 0x00 | Create a new directory. | D | CORE |
| SMB\_COM\_DELETE\_DIRECTORY (section [2.2.4.2](0bca354c-42d9-46b7-a0ae-d8c6870242ca))<br><br>SMBrmdir | 0x01 | Delete an empty directory. | C | CORE |
| SMB\_COM\_OPEN (section [2.2.4.3](ec064de8-6538-401e-8c73-b37231c36f2b))<br><br>SMBopen | 0x02 | Open a file. | D | CORE |
| SMB\_COM\_CREATE (section [2.2.4.4](87622f43-3758-4bf9-b1fb-35109f0e5c15))<br><br>SMBcreate | 0x03 | Create or open a file. | D | CORE |
| SMB\_COM\_CLOSE (section [2.2.4.5](10059dd2-ae0a-48a2-a95c-a92505e9145f))<br><br>SMBclose | 0x04 | Close a file. | C | CORE |
| SMB\_COM\_FLUSH (section [2.2.4.6](32acdf03-011d-4e93-b169-a787f21dc13d))<br><br>SMBflush | 0x05 | Flush data for a file, or all files associated with a client, PID pair. | C | CORE |
| SMB\_COM\_DELETE (section [2.2.4.7](e455faa4-d996-43a5-87eb-9993b0ceb896))<br><br>SMBunlink | 0x06 | Delete a file. | C | CORE |
| SMB\_COM\_RENAME (section [2.2.4.8](d78c549c-9ab8-4d92-bbbc-6843bed943f6))<br><br>SMBmv | 0x07 | Rename a file or set of files. | C | CORE |
| SMB\_COM\_QUERY\_INFORMATION (section [2.2.4.9](d36b4a5c-df1b-4255-aa5b-ac6ef5c2fb7c))<br><br>SMBgetattr | 0x08 | Get file attributes. | D | CORE |
| SMB\_COM\_SET\_INFORMATION (section [2.2.4.10](e3cd0acd-aa84-4fbf-8c9d-3e7d3bb3fd52))<br><br>SMBsetattr | 0x09 | Set file attributes. | D | CORE |
| SMB\_COM\_READ (section [2.2.4.11](b88922dd-b18e-46e0-9f74-08eaace9a95c))<br><br>SMBread | 0x0A | Read from a file. | D | CORE |
| SMB\_COM\_WRITE (section [2.2.4.12](5f3ebf6a-5d06-43ee-9429-c8cc1b58eef5))<br><br>SMBwrite | 0x0B | Write to a file. | D | CORE |
| SMB\_COM\_LOCK\_BYTE\_RANGE (section [2.2.4.13](21f7b95a-56c6-482d-80d6-881ec0e6db69))<br><br>SMBlock | 0x0C | Request a byte-range lock on a file. | D | CORE |
| SMB\_COM\_UNLOCK\_BYTE\_RANGE (section [2.2.4.14](3cfce682-97d8-499b-8a2c-ef000f5d6b26))<br><br>SMBunlock | 0x0D | Release a byte-range lock on a file. | D | CORE |
| SMB\_COM\_CREATE\_TEMPORARY (section [2.2.4.15](6ea3a4b2-2a9b-4749-a4a4-41efebdf4015))<br><br>SMBctemp | 0x0E | Create a temporary file. | O | CORE |
| SMB\_COM\_CREATE\_NEW (section [2.2.4.16](161fa213-ba9d-4bad-9483-29e8b5872dca))<br><br>SMBmknew | 0x0F | Create and open a new file. | D | CORE |
| SMB\_COM\_CHECK\_DIRECTORY (section [2.2.4.17](6a989d51-30bf-4ceb-a46e-7ae1cee6b516))<br><br>SMBchkpth | 0x10 | Verify that the specified pathname resolves to a directory.<br><br>Listed as SMBchkpath in some documentation. | C | CORE |
| SMB\_COM\_PROCESS\_EXIT (section [2.2.4.18](233f62a6-f565-478d-b9b8-2b58ff347547))<br><br>SMBexit | 0x11 | Indicate process exit. | O | CORE |
| SMB\_COM\_SEEK (section [2.2.4.19](80846ca9-8b50-4183-85c6-01c4e586227e))<br><br>SMBlseek | 0x12 | Set the current file pointer within a file. | O | CORE |
| SMB\_COM\_LOCK\_AND\_READ (section [2.2.4.20](88a423e7-8232-4b22-904d-d9e6cc0a226e))<br><br>SMBlockread | 0x13 | Lock and read a byte-range within a file. | D | CorePlus |
| SMB\_COM\_WRITE\_AND\_UNLOCK (section [2.2.4.21](5006049a-e39b-4dac-83f2-0ec64c731c9c))<br><br>SMBwriteunlock | 0x14 | Write and unlock a byte-range within a file. | D | CorePlus |
| **Unused** | 0x15<br><br>...<br><br>0x19 |  |  |  |
| SMB\_COM\_READ\_RAW (section [2.2.4.22](a8c3a184-272c-4168-bbb2-dcc621c503a0))<br><br>SMBreadBraw | 0x1A | Read a block in raw mode. | D | CorePlus |
| SMB\_COM\_READ\_MPX (section [2.2.4.23](9688c718-1f35-43f2-80c5-30d8a59ac305))<br><br>SMBreadBmpx | 0x1B | Multiplexed block read.<br><br>Listed as SMBreadmpx in some documentation. | O | LANMAN1.0 |
| SMB\_COM\_READ\_MPX\_SECONDARY (section [2.2.4.24](f0c06fcc-6238-4119-be52-e3e9606d209b))<br><br>SMBreadBs | 0x1C | Multiplexed block read, secondary request. | X | LANMAN1.0 |
| SMB\_COM\_WRITE\_RAW (section [2.2.4.25](5feebf73-e3b3-4bbd-a449-7aea0a4cf87e))<br><br>SMBwriteBraw | 0x1D | Write a block in raw mode. | D | CorePlus |
| SMB\_COM\_WRITE\_MPX (section [2.2.4.26](ab9a9440-9c22-49fd-859e-2fd81c57e9d9))<br><br>SMBwriteBmpx | 0x1E | Multiplexed block write. | O | LANMAN1.0 |
| SMB\_COM\_WRITE\_MPX\_SECONDARY (section [2.2.4.27](d07bc94a-9da8-43f7-8777-9e9033891ef7))<br><br>SMBwriteBs | 0x1F | Multiplexed block write, secondary request. | X | LANMAN1.0 |
| SMB\_COM\_WRITE\_COMPLETE (section [2.2.4.28](1e82640c-cd31-49ee-9729-84b30ee1132c))<br><br>SMBwriteC | 0x20 | Raw block write, final response. | D | LANMAN1.0 |
| SMB\_COM\_QUERY\_SERVER (section [2.2.4.29](d7ad4160-5758-4685-9f68-0e6c531982a2)) | 0x21 | Reserved, but not implemented.<br><br>Also known as SMB\_COM\_QUERY\_INFORMATION\_SRV. | N |  |
| SMB\_COM\_SET\_INFORMATION2 (section [2.2.4.30](cfcda87d-7634-4902-a137-c60a1f4a5ae5))<br><br>SMBsetattrE | 0x22 | Set an extended set of file attributes. | D | LANMAN1.0 |
| SMB\_COM\_QUERY\_INFORMATION2 (section [2.2.4.31](33ebe09e-4c9d-4adc-b23b-40e4348c704f))<br><br>SMBgetattrE | 0x23 | Get an extended set of file attributes. | D | LANMAN1.0 |
| SMB\_COM\_LOCKING\_ANDX (section [2.2.4.32](df492170-a2e8-40d1-b7d5-eb29364047e1))<br><br>SMBlockingX | 0x24 | Lock multiple byte ranges; AndX chaining. | C | LANMAN1.0 |
| SMB\_COM\_TRANSACTION (section [2.2.4.33](0ed1ad9f-ab96-4a7a-b94a-0915f3796781))<br><br>SMBtrans | 0x25 | Transaction. | C | LANMAN1.0 |
| SMB\_COM\_TRANSACTION\_SECONDARY (section [2.2.4.34](a4c64387-1dc4-45fb-b01f-9ad8b69e83e1))<br><br>SMBtranss | 0x26 | Transaction secondary request. | C | LANMAN1.0 |
| SMB\_COM\_IOCTL (section [2.2.4.35](0d8f5f17-16af-499d-a192-a5fd85fbb7e1))<br><br>SMBioctl | 0x27 | Pass an I/O Control function request to the server. | O | LANMAN1.0 |
| SMB\_COM\_IOCTL\_SECONDARY (section [2.2.4.36](3a5f8e47-16e6-484d-9346-6c4cbdc22dec))<br><br>SMBioctls | 0x28 | IOCTL secondary request. | N | LANMAN1.0 |
| SMB\_COM\_COPY (section [2.2.4.37](14b0f5c5-6fa8-4e1a-9a59-7556206bcd56))<br><br>SMBcopy | 0x29 | Copy a file or directory. | X | LANMAN1.0 |
| SMB\_COM\_MOVE (section [2.2.4.38](817ee280-ffc9-443d-b9f3-475c4c02a4f1))<br><br>SMBmove | 0x2A | Move a file or directory. | X | LANMAN1.0 |
| SMB\_COM\_ECHO (section [2.2.4.39](8c854352-67c6-47f7-a60d-a6c87b6b3aac))<br><br>SMBecho | 0x2B | Echo request (ping). | C | LANMAN1.0 |
| SMB\_COM\_WRITE\_AND\_CLOSE (section [2.2.4.40](029b038c-4d4b-42fc-8c51-99eb23055e9c))<br><br>SMBwriteclose | 0x2C | Write to and close a file. | D | LANMAN1.0 |
| SMB\_COM\_OPEN\_ANDX (section [2.2.4.41](49a0f97d-c4a7-48a3-bf50-46d816825729))<br><br>SMBopenX | 0x2D | Extended file open with AndX chaining. | D | LANMAN1.0 |
| SMB\_COM\_READ\_ANDX (section [2.2.4.42](129aa093-574b-483e-a55d-df334606a622))<br><br>SMBreadX | 0x2E | Extended file read with AndX chaining. | C | LANMAN1.0 |
| SMB\_COM\_WRITE\_ANDX (section [2.2.4.43](81aec377-0ff4-4fc4-bc56-8f05b70c3e42))<br><br>SMBwriteX | 0x2F | Extended file write with AndX chaining. | C | LANMAN1.0 |
| SMB\_COM\_NEW\_FILE\_SIZE (section [2.2.4.44](e3b0e8ec-a0f3-48d7-92b9-25715e5ec6c8)) | 0x30 | Reserved, but not implemented.<br><br>Also known as SMB\_COM\_SET\_NEW\_SIZE. | N |  |
| SMB\_COM\_CLOSE\_AND\_TREE\_DISC (section [2.2.4.45](3b4c6712-d77c-48ed-90d8-653956601ecd)) | 0x31 | Close an open file and tree disconnect. | N | NT LANMAN |
| [SMB_COM_TRANSACTION2 (section 2.2.4.46)](3d9d8f3e-dc70-410d-a3fc-6f4a881e8cab)<br><br>SMBtrans2 | 0x32 | Transaction 2 format request/response. | C | LANMAN1.2 |
| [SMB_COM_TRANSACTION2_SECONDARY (section 2.2.4.47)](80207e03-6cd6-4bbe-863f-db52f4d2cb1a)<br><br>SMBtranss2 | 0x33 | Transaction 2 secondary request. | C | LANMAN1.2 |
| [SMB_COM_FIND_CLOSE2 (section 2.2.4.48)](31cdb10b-8c1b-4ee9-9ad2-3221c3941760)<br><br>SMBfindclose | 0x34 | Close an active search. | C | LANMAN1.2 |
| [SMB_COM_FIND_NOTIFY_CLOSE (section 2.2.4.49)](98e3f3b8-adf7-4dfa-a633-91c19f0b83b0)<br><br>SMBfindnclose | 0x35 | Notification of the closure of an active search. | N | LANMAN1.2 |
| **Unused** | 0x36<br><br>...<br><br>0x5F |  |  |  |
| **Reserved** | 0x60<br><br>...<br><br>0x6F | This range of codes was reserved for use by the "xenix1.1" dialect of SMB. See [\[MSFT-XEXTNP\]](https://go.microsoft.com/fwlink/?LinkId=162042). [\[XOPEN-SMB\]](https://go.microsoft.com/fwlink/?linkid=2297696) page 41 lists this range as "Reserved for proprietary dialects." | X | XENIX |
| [SMB_COM_TREE_CONNECT (section 2.2.4.50)](4a6fc9ea-de6d-484d-a59b-3ba68a6d760c)<br><br>SMBtcon | 0x70 | Tree connect. | D | CORE |
| [SMB_COM_TREE_DISCONNECT (section 2.2.4.51)](31cc172a-8084-4f0b-aad6-d8d69da76a0e)<br><br>SMBtdis | 0x71 | Tree disconnect. | C | CORE |
| [SMB_COM_NEGOTIATE (section 2.2.4.52)](96ccc2bd-67ba-463a-bb73-fd6a9265199e)<br><br>SMBnegprot | 0x72 | Negotiate protocol dialect. | C | CORE |
| [SMB_COM_SESSION_SETUP_ANDX (section 2.2.4.53)](d902407c-e73b-46f5-8f9e-a2de2b6085a2)<br><br>SMBsesssetupX | 0x73 | Session Setup with AndX chaining. | C | LANMAN1.0 |
| [SMB_COM_LOGOFF_ANDX (section 2.2.4.54)](53800b5c-f0c6-4b9c-baeb-1ad6b08ecb6b)<br><br>SMBulogoffX | 0x74 | User logoff with AndX chaining. | C | LANMAN1.2 |
| [SMB_COM_TREE_CONNECT_ANDX (section 2.2.4.55)](a105173a-d854-4950-be28-3d3240529ec3)<br><br>SMBtconX | 0x75 | Tree connect with AndX chaining. | C | LANMAN1.0 |
| **Unused** | 0x76<br><br>...<br><br>0x7D |  |  |  |
| [SMB_COM_SECURITY_PACKAGE_ANDX (section 2.2.4.56)](adb39707-dd58-4d27-8aa0-7a98c04cff42)<br><br>SMBsecpkgX | 0x7E | Negotiate security packages with AndX chaining. | X | LANMAN1.0 |
| **Unused** | 0x7F |  |  |  |
| [SMB_COM_QUERY_INFORMATION_DISK (section 2.2.4.57)](c5b02889-bcf4-4ad1-9bd7-014614179107)<br><br>SMBdskattr | 0x80 | Retrieve file system information from the server. | D | CORE |
| [SMB_COM_SEARCH (section 2.2.4.58)](d33e8472-1356-406d-88ed-bd9fc10b060b)<br><br>SMBsearch | 0x81 | Directory wildcard search. | D | CORE |
| [SMB_COM_FIND (section 2.2.4.59)](5df45d03-d4e9-4dfd-850f-639363b8dffd)<br><br>SMBffirst | 0x82 | Start or continue an extended wildcard directory search. | D | LANMAN1.0 |
| [SMB_COM_FIND_UNIQUE (section 2.2.4.60)](828fff83-d37b-4deb-8118-24c950dca87a)<br><br>SMBfunique | 0x83 | Perform a one-time extended wildcard directory search. | D | LANMAN1.0 |
| [SMB_COM_FIND_CLOSE (section 2.2.4.61)](3ffcd296-c7cc-4393-8ab0-6c902a928eec)<br><br>SMBfclose | 0x84 | End an extended wildcard directory search. | D | LANMAN1.0 |
| **Unused** | 0x85<br><br>...<br><br>0x9F |  |  |  |
| [SMB_COM_NT_TRANSACT (section 2.2.4.62)](55db04d6-105f-45d1-84ac-6972c0a1ddc8) | 0xA0 | NT format transaction request/response. | C | NT LANMAN |
| [SMB_COM_NT_TRANSACT_SECONDARY (section 2.2.4.63)](0941c749-cbf3-4c1b-91b2-b013a7473827) | 0xA1 | NT format transaction secondary request. | C | NT LANMAN |
| [SMB_COM_NT_CREATE_ANDX (section 2.2.4.64)](d3f83a7e-493b-4d29-b21c-55768b93e144) | 0xA2 | Create or open a file or a directory. | C | NT LANMAN |
| **Unused** | 0xA3 |  |  |  |
| [SMB_COM_NT_CANCEL (section 2.2.4.65)](bf04c12b-e5ee-4107-9b76-0e5ffda9cc3f) | 0xA4 | Cancel a request currently pending at the server. | C | NT LANMAN |
| [SMB_COM_NT_RENAME (section 2.2.4.66)](014a4147-4206-4ab2-a167-b58a4d11f1a7) | 0xA5 | File rename with extended semantics. | O | NT LANMAN |
| **Unused** | 0xA6<br><br>...<br><br>0xBF |  |  |  |
| [SMB_COM_OPEN_PRINT_FILE (section 2.2.4.67)](4cce0e9f-ab27-40f7-97cc-6f12b4a9afef)<br><br>SMBsplopen | 0xC0 | Create a print queue spool file. | C | CORE |
| [SMB_COM_WRITE_PRINT_FILE (section 2.2.4.68)](1b14601f-89a5-4e21-b2ac-0bf1d2374957)<br><br>SMBsplwr | 0xC1 | Write to a print queue spool file. | D | CORE |
| [SMB_COM_CLOSE_PRINT_FILE (section 2.2.4.69)](c4993aee-d13b-4a6e-87bd-efdaf7506906)<br><br>SMBsplclose | 0xC2 | Close a print queue spool file. | D | CORE |
| [SMB_COM_GET_PRINT_QUEUE (section 2.2.4.70)](8aaa6b27-b144-4cd6-9171-102217b1406d)<br><br>SMBsplretq | 0xC3 | Request print queue information. | X | CORE |
| **Unused** | 0xC4<br><br>...<br><br>0xCF |  |  |  |
| **Reserved** | 0xD0<br><br>...<br><br>0xD7 | Messenger Service command codes.<br><br>This range is reserved for use by the SMB Messenger Service. See [\[MS-MSRP\]](../ms-msrp/b3dd697e-6e3e-456d-a536-04ddde16ac95), and section 6 of [\[SMB-CORE\]](https://go.microsoft.com/fwlink/?LinkId=164301). | O | CORE |
| [SMB_COM_READ_BULK (section 2.2.4.71)](c5d7c2d7-4c99-4bd8-b4ef-a756f09e114a) | 0xD8 | Reserved, but not implemented. | N |  |
| [SMB_COM_WRITE_BULK (section 2.2.4.72)](a5baa104-0ad0-4088-9d96-848aa59aef3b) | 0xD9 | Reserved, but not implemented. | N |  |
| [SMB_COM_WRITE_BULK_DATA (section 2.2.4.73)](0cc41665-80d5-49aa-af4e-6fff0ed1820f) | 0xDA | Reserved, but not implemented. | N |  |
| **Unused** | 0xDB<br><br>...<br><br>0xFD |  |  |  |
| [SMB_COM_INVALID (section 2.2.4.74)](56cd8dd2-98cb-4ef7-a088-5c53905e0fc0)<br><br>SMBinvalid | 0xFE | As the name suggests, this command code is a designated invalid command and SHOULD NOT be used. | C | LANMAN1.0 |
| [SMB_COM_NO_ANDX_COMMAND (section 2.2.4.75)](10921e06-804f-4b5a-92a5-1cc562f43068) | 0xFF | Also known as the "NIL" command. It identifies the end of an AndX Chain, and is only valid in that context. See section [2.2.3.4](fc4d19f7-8040-426d-9154-7219c57453c8). | C | LANMAN1.0 |
