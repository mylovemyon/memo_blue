```powershell
PS C:\Users\SANSDFIR> .\evtx_dump-v0.12.3.exe -o jsonl -f system.json E:\Windows\System32\winevt\Logs\System.evtx
PS C:\Users\SANSDFIR>
```

## cli
```powershell
PS C:\Users\SANSDFIR> .\duckdb.exe -cmd ".maxrows 10000" -cmd ".pager off" .\system.json
DuckDB v1.5.6 (Variegata)
Enter ".help" for usage hints.
system_db D
```
