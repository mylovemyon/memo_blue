## 0x00 (SMB_COM_CREATE_DIRECTORY)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/e6e870ad-7037-4b79-ac54-4a42a1ba4561
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 223" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x00",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "212"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "10"
          },
          "Create Directory Request (0x00)": {
            "smb.wct": "0",
            "smb.bcc": "31",
            "smb.buffer_format": "4",
            "smb.dir_name": "\\testsfileinfo"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 224" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "223",
            "smb.time": "0.013373000",
            "smb.cmd": "0x00",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "212"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "10"
          },
          "Create Directory Response (0x00)": {
            "smb.file": "\\testsfileinfo",
            "smb.wct": "0",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```



## 0x01 (SMB_COM_DELETE_DIRECTORY)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/0bca354c-42d9-46b7-a0ae-d8c6870242ca
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 221" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x01",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "212"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "9"
          },
          "Delete Directory Request (0x01)": {
            "smb.wct": "0",
            "smb.bcc": "31",
            "smb.buffer_format": "4",
            "smb.dir_name": "\\testsfileinfo"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 222" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "221",
            "smb.time": "0.001726000",
            "smb.cmd": "0x01",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "212"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "9"
          },
          "Delete Directory Response (0x01)": {
            "smb.file": "\\testsfileinfo",
            "smb.wct": "0",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```



## 0x02 (SMB_COM_OPEN)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/ec064de8-6538-401e-8c73-b37231c36f2b
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 15751" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x02",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "15702"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "29"
          },
          "Open Request (0x02)": {
            "smb.wct": "2",
            "smb.access.desired": "0x00ff",
            "smb.access.desired_tree": {
              "smb.access.writethrough": "0",
              "smb.access.caching": "0",
              "smb.access.locality": "0",
              "smb.access.sharing": "7",
              "smb.access.mode": "7"
            },
            "smb.search.attribute": "0x0000",
            "smb.search.attribute_tree": {
              "smb.search.attribute.read_only": "0",
              "smb.search.attribute.hidden": "0",
              "smb.search.attribute.system": "0",
              "smb.search.attribute.volume": "0",
              "smb.search.attribute.directory": "0",
              "smb.search.attribute.archive": "0"
            },
            "smb.bcc": "53",
            "smb.buffer_format": "4",
            "smb.file": "\\rawopen\\torture_open.txt"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 15752" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "15751",
            "smb.time": "0.000949000",
            "smb.cmd": "0x02",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "15702"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "29"
          },
          "Open Response (0x02)": {
            "smb.wct": "7",
            "smb.fid": "0x4003",
            "smb.fid_tree": {
              "smb.fid.opened_in": "15752"
            },
            "smb.file_attribute": "0x0020",
            "smb.file_attribute_tree": {
              "smb.file_attribute.archive": "1",
              "smb.file_attribute.directory": "0",
              "smb.file_attribute.volume": "0",
              "smb.file_attribute.system": "0",
              "smb.file_attribute.hidden": "0",
              "smb.file_attribute.read_only": "0"
            },
            "smb.last_write.time": "2006-02-17T05:32:14.000000000Z",
            "smb.file_size": "7",
            "smb.access.granted": "0x00ff",
            "smb.access.granted_tree": {
              "smb.access.writethrough": "0",
              "smb.access.caching": "0",
              "smb.access.locality": "0",
              "smb.access.sharing": "7",
              "smb.access.mode": "7"
            },
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```



## 0x03 (SMB_COM_CREATE)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/87622f43-3758-4bf9-b1fb-35109f0e5c15
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 16643" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x03",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "15702"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "434"
          },
          "Create Request (0x03)": {
            "smb.wct": "3",
            "smb.file_attribute": "0x0000",
            "smb.file_attribute_tree": {
              "smb.file_attribute.archive": "0",
              "smb.file_attribute.directory": "0",
              "smb.file_attribute.volume": "0",
              "smb.file_attribute.system": "0",
              "smb.file_attribute.hidden": "0",
              "smb.file_attribute.read_only": "0"
            },
            "smb.create.time": "1970-01-01T00:00:00.000000000Z",
            "smb.bcc": "57",
            "smb.buffer_format": "4",
            "smb.file": "\\rawopen\\torture_create.txt"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 16644" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "16643",
            "smb.time": "0.001451000",
            "smb.cmd": "0x03",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "15702"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "434"
          },
          "Create Response (0x03)": {
            "smb.wct": "1",
            "smb.fid": "0x8009",
            "smb.fid_tree": {
              "smb.fid.opened_in": "16644"
            },
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```



## 0x04 (SMB_COM_CLOSE)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/10059dd2-ae0a-48a2-a95c-a92505e9145f
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 188" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x04",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "57"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "70"
          },
          "Close Request (0x04)": {
            "smb.wct": "3",
            "smb.fid": "0x4000",
            "smb.last_write.time": "1970-01-01T00:00:00.000000000Z",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 189" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "188",
            "smb.time": "0.000728000",
            "smb.cmd": "0x04",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "57"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "70"
          },
          "Close Response (0x04)": {
            "smb.fid": "0x4000",
            "smb.wct": "0",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```



## 0x05 (SMB_COM_FLUSH)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/10059dd2-ae0a-48a2-a95c-a92505e9145f
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 15660" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x05",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "15605"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "32"
          },
          "Flush Request (0x05)": {
            "smb.wct": "1",
            "smb.fid": "0x4002",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 15661" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "15660",
            "smb.time": "0.000493000",
            "smb.cmd": "0x05",
            "smb.nt_status": "0xc0000008",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "15605"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "32"
          },
          "Flush Response (0x05)": {
            "smb.fid": "0x4002",
            "smb.wct": "0",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```



## 0x06 (SMB_COM_DELETE)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/e455faa4-d996-43a5-87eb-9993b0ceb896
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 194" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x06",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "57"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "73"
          },
          "Delete Request (0x06)": {
            "smb.wct": "1",
            "smb.search.attribute": "0x0016",
            "smb.search.attribute_tree": {
              "smb.search.attribute.read_only": "0",
              "smb.search.attribute.hidden": "1",
              "smb.search.attribute.system": "1",
              "smb.search.attribute.volume": "0",
              "smb.search.attribute.directory": "1",
              "smb.search.attribute.archive": "0"
            },
            "smb.bcc": "47",
            "smb.buffer_format": "4",
            "smb.file": "\\torture_qfileinfo.txt"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 195" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "194",
            "smb.time": "0.001024000",
            "smb.cmd": "0x06",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "57"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "73"
          },
          "Delete Response (0x06)": {
            "smb.file": "\\torture_qfileinfo.txt",
            "smb.wct": "0",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```



## 0x07 (SMB_COM_RENAME)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/d78c549c-9ab8-4d92-bbbc-6843bed943f6
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 19436" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x07",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "19420"
            },
            "smb.pid": "31482",
            "smb.uid": "2048",
            "smb.mid": "12"
          },
          "Rename Request (0x07)": {
            "smb.wct": "1",
            "smb.search.attribute": "0x0000",
            "smb.search.attribute_tree": {
              "smb.search.attribute.read_only": "0",
              "smb.search.attribute.hidden": "0",
              "smb.search.attribute.system": "0",
              "smb.search.attribute.volume": "0",
              "smb.search.attribute.directory": "0",
              "smb.search.attribute.archive": "0"
            },
            "smb.bcc": "91",
            "smb.buffer_format": "4",
            "smb.old_file": "\\testrename\\test1.txt",
            "smb.buffer_format": "4",
            "smb.file": "\\testrename\\test2.txt"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 19437" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "19436",
            "smb.time": "0.003814000",
            "smb.cmd": "0x07",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "19420"
            },
            "smb.pid": "31482",
            "smb.uid": "2048",
            "smb.mid": "12"
          },
          "Rename Response (0x07)": {
            "smb.old_file": "\\testrename\\test1.txt",
            "smb.file": "\\testrename\\test2.txt",
            "smb.wct": "0",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```



## 0x08 (SMB_COM_QUERY_INFORMATION)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/d36b4a5c-df1b-4255-aa5b-ac6ef5c2fb7c
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 70" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x08",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "57"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "11"
          },
          "Query Information Request (0x08)": {
            "smb.wct": "0",
            "smb.bcc": "47",
            "smb.buffer_format": "4",
            "smb.file": "\\torture_qfileinfo.txt"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 71" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "70",
            "smb.time": "0.000943000",
            "smb.cmd": "0x08",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "57"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "11"
          },
          "Query Information Response (0x08)": {
            "smb.wct": "10",
            "smb.file_attribute": "0x0020",
            "smb.file_attribute_tree": {
              "smb.file_attribute.archive": "1",
              "smb.file_attribute.directory": "0",
              "smb.file_attribute.volume": "0",
              "smb.file_attribute.system": "0",
              "smb.file_attribute.hidden": "0",
              "smb.file_attribute.read_only": "0"
            },
            "smb.last_write.time": "2006-02-17T05:31:58.000000000Z",
            "smb.file_size": "7",
            "smb.reserved": "00:00:00:00:00:00:00:00:00:00",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```



## 0x09 (SMB_COM_SET_INFORMATION)
https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cifs/e3cd0acd-aa84-4fbf-8c9d-3e7d3bb3fd52
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 253" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.cmd": "0x09",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x08",
            "smb.flags_tree": {
              "smb.flags.response": "0",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "212"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "25"
          },
          "Set Information Request (0x09)": {
            "smb.wct": "8",
            "smb.file_attribute": "0x0001",
            "smb.file_attribute_tree": {
              "smb.file_attribute.archive": "0",
              "smb.file_attribute.directory": "0",
              "smb.file_attribute.volume": "0",
              "smb.file_attribute.system": "0",
              "smb.file_attribute.hidden": "0",
              "smb.file_attribute.read_only": "1"
            },
            "smb.last_write.time": "2005-11-18T05:31:58.000000000Z",
            "smb.reserved": "00:00:00:00:00:00:00:00:00:00",
            "smb.bcc": "71",
            "smb.buffer_format": "4",
            "smb.file": "\\testsfileinfo\\fname_test_18.txt",
            "smb.extra_byte_parameters": "04:00:00:00"
          }
        }
      }
    }
  }
]
```
```json
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 254" -T json -J smb
[
  {
    "_index": "packets-2005-11-19",
    "_score": null,
    "_source": {
      "layers": {
        "frame": {
          "filtered": "frame"
        },
        "eth": {
          "filtered": "eth"
        },
        "ip": {
          "filtered": "ip"
        },
        "tcp": {
          "filtered": "tcp"
        },
        "nbss": {
          "filtered": "nbss"
        },
        "smb": {
          "SMB Header": {
            "smb.server_component": "0x424d53ff",
            "smb.response_to": "253",
            "smb.time": "0.001016000",
            "smb.cmd": "0x09",
            "smb.nt_status": "0x00000000",
            "smb.flags": "0x88",
            "smb.flags_tree": {
              "smb.flags.response": "1",
              "smb.flags.notify": "0",
              "smb.flags.oplock": "0",
              "smb.flags.canon": "0",
              "smb.flags.caseless": "1",
              "smb.flags.receive_buffer": "0",
              "smb.flags.lock": "0"
            },
            "smb.flags2": "0xc803",
            "smb.flags2_tree": {
              "smb.flags2.string": "1",
              "smb.flags2.nt_error": "1",
              "smb.flags2.roe": "0",
              "smb.flags2.dfs": "0",
              "smb.flags2.esn": "1",
              "smb.flags2.reparse_path": "0",
              "smb.flags2.long_names_used": "0",
              "smb.flags2.sec_sig_required": "0",
              "smb.flags2.compressed": "0",
              "smb.flags2.sec_sig": "0",
              "smb.flags2.ea": "1",
              "smb.flags2.long_names_allowed": "1"
            },
            "smb.pid.high": "0",
            "smb.signature": "00:00:00:00:00:00:00:00",
            "smb.reserved": "00:00",
            "smb.tid": "2048",
            "smb.tid_tree": {
              "smb.path": "\\\\192.168.114.129\\TEST",
              "smb.fid.mapped_in": "212"
            },
            "smb.pid": "31043",
            "smb.uid": "2048",
            "smb.mid": "25"
          },
          "Set Information Response (0x09)": {
            "smb.wct": "0",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```
