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
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 15733" -T json -J smb
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
            "smb.mid": "20"
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
PS C:\Users\Public> tshark -r .\smbtorture.cap -Y "frame.number == 15734" -T json -J smb
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
            "smb.response_to": "15733",
            "smb.time": "0.001457000",
            "smb.cmd": "0x02",
            "smb.nt_status": "0xc0000034",
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
            "smb.mid": "20"
          },
          "Open Response (0x02)": {
            "smb.wct": "0",
            "smb.bcc": "0"
          }
        }
      }
    }
  }
]
```
