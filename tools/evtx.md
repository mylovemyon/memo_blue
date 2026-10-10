https://github.com/omerbenamram/evtx

# command
## --help
```
Utility to parse EVTX files

Usage: evtx_dump-v0.12.3.exe [OPTIONS] [INPUT] [COMMAND]

Commands:
  extract-wevt-templates   Build a WEVT template cache from PE files (EXE/DLL)
  dump-template-instances  Dump BinXML TemplateInstance substitution arrays from EVTX records (JSONL)
  apply-wevt-cache         Render a WEVT template using an offline cache + substitution values
  help                     Print this message or the help of the given subcommand(s)

Arguments:
  [INPUT]
          Input EVTX file path, or '-' to read from stdin. Required unless using a subcommand.

Options:
  -t, --threads <num-threads>
          Sets the number of worker threads, defaults to number of CPU cores.

          [default: 0]

  -o, --format <output-format>
          Sets the output format:
          "xml"   - prints XML output.
          "json"  - prints JSON output.
          "jsonl" - (jsonlines) same as json with --no-indent --dont-show-record-number


          [default: xml]
          [possible values: json, xml, jsonl]

  -f, --output <output-target>
          Writes output to the file specified instead of stdout, errors will still be printed to stderr.
          Will ask for confirmation before overwriting files, to allow overwriting, pass `--no-confirm-overwrite`
          Will create parent directories if needed.

      --no-confirm-overwrite
          When set, will not ask for confirmation before overwriting files, useful for automation

      --events <event-ranges>
          When set, only the specified events (offseted reltaive to file) will be outputted.
          For example:
              --events=1 will output the first event.
              --events=0-10,20-30 will output events 0-10 and 20-30.


      --validate-checksums
          When set, chunks with invalid checksums will not be parsed. Usually dirty files have bad checksums, so using this flag will result in fewer records.

      --no-indent
          When set, output will not be indented.

      --separate-json-attributes
          If outputting JSON, XML Element's attributes will be stored in a separate object named '<ELEMENTNAME>_attributes', with <ELEMENTNAME> containing the value of the node.

      --dont-show-record-number
          When set, `Record <id>` will not be printed.

      --ansi-codec <ansi-codec>
          When set, controls the codec of ansi encoded strings the file.

          [default: windows-1252]
          [possible values: ascii, ibm866, iso-8859-1, iso-8859-2, iso-8859-3, iso-8859-4, iso-8859-5, iso-8859-6, iso-8859-7, iso-8859-8, iso-8859-10, iso-8859-13, iso-8859-14, iso-8859-15, iso-8859-16, koi8-r, koi8-u, mac-roman, windows-874, windows-1250, windows-1251, windows-1252, windows-1253, windows-1254, windows-1255, windows-1256, windows-1257, windows-1258, mac-cyrillic, utf-8, windows-949, euc-jp, windows-31j, gbk, gb18030, hz, big5-2003, pua-mapped-binary, iso-8859-8-i]

      --wevt-cache <WEVTCACHE>
          Path to a WEVT template cache file (`.wevtcache`). When set, evtx_dump will try to render records using this cache if the embedded EVTX template expansion fails.

      --stop-after-one-error
          When set, will exit after any failure of reading a record. Useful for debugging.

  -v...
          Sets debug prints level for the application:
              -v   - info
              -vv  - debug
              -vvv - trace
          NOTE: trace output is only available in debug builds, as it is extremely verbose.

  -h, --help
          Print help (see a summary with '-h')

  -V, --version
          Print version
```


## -o
```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f security.json E:\Windows\System32\winevt\Logs\Security.evtx
PS C:\Users\SANSDFIR>
```
