# START

#plateform/multiple #target/local #cat/MAP

% boris, map, views, sichten, hilfe, howto, index, konvention, achse, weg, kat, start
## [MAP] Drei Sichten - so nutzt du das Arsenal (LESEN)
arsenal ist UND-Suche: jedes Wort muss matchen (Titel/Tags/Kommando). !wort schliesst aus.
Titel-Spalte zeigt die ACHSE: WEG (Angriffsweg) / KAT (Technik) / START (Setup). CAT-Spalte = der Bucket (RELAY/CREDS/...). Tippe weg / kat / start um eine ganze Achse zu filtern.
TOOL-Sicht: certipy/nxc/secretsdump... -> alles mit dem Tool. WEG-Sicht: relay/adcs/rbcd... -> geordnete Kette. KATEGORIE-Sicht: creds/lateral/persistence... -> Menue aller Varianten einer Technik. Kombinieren: 'creds nxc', 'relay !responder'.
```
echo 'TOOL x WEG x KATEGORIE  |  z.B.: creds  |  creds nxc  |  adcs goldencert  |  lateral !psexec  |  tippe boris fuer ALLES'
```

% boris, map, views, tool, toolsicht, vokabular, certipy, nxc, secretsdump, impacket
## [MAP] TOOL-Sicht - kanonische Tool-Woerter
Jede Karte traegt genau ein Tool-Wort.
```
echo 'certipy | nxc | responder | ntlmrelayx | coercer | bloodhound | nmap | msf | manspider | getST | rbcd | addcomputer | secretsdump | lsassy | donpapi | pypykatz | psexec | wmiexec | evilwinrm | ticketer'
```

% boris, map, views, weg, wegsicht, angriffsweg, path, relay, adcs, coerce, rbcd, vulns, recon
## [MAP] WEG-Sicht - Angriffswege (geordnete Ketten; = Dateien 10-20)
```
echo 'relay = Responder+NTLM-Relay+SOCKS | adcs = ESC1/ESC8/Golden-Cert | coerce = Zwangs-Auth | rbcd = Resource-Based Delegation | vulns = CVE-Quickcheck | recon = Discovery/Enum'
```

% boris, map, views, kategorie, technik, tactic, creds, lateral, persistence, dcsync
## [MAP] KATEGORIE-Sicht - Techniken/Taktiken (Menues; = Dateien 30+)
Kategorie = ein Job + ALLE Varianten (ATT&CK-artig, ungeordnet). Ergaenzt die Weg-Sicht.
```
echo 'creds (=dump) = Credential Dumping | lateral (=exec) = Lateral Movement/Exec | persistence (=persist,dcsync) = Persistence/DCSync/Tickets'
```

% boris, map, konvention, naming, regel, impacket, certipy, verbindlich, aufruf
## [MAP] NAMENS-REGELN (verbindlich fuer ALLE Commands)
Impacket IMMER als impacket-<tool> (NIE psexec.py/secretsdump.py). Certipy IMMER certipy-ad (NIE certipy). Standalone-Skripte (PetitPotam.py/Coercer.py/dfscoerce.py) behalten .py - das sind KEINE impacket-Tools.
```
echo 'impacket-psexec/secretsdump/wmiexec/getST/... (nie .py)  |  certipy-ad (nie certipy)'
```

% boris, map, workflow, quickwins, reihenfolge, engagement, start
## [MAP] Empfohlene Reihenfolge (Quick Wins)
1) globals setzen  2) relay im Hintergrund  3) adcs (find->ESC1/ESC8->ggf Golden-Cert)  4) vulns scannen  5) rbcd wo GenericWrite/MAQ. Danach: creds dumpen -> lateral bewegen -> persistence. coerce speist relay+esc8.
```
echo 'globals -> relay -> adcs -> vulns -> rbcd -> creds -> lateral -> persistence'
```
