# HELP

#plateform/multiple #target/local #cat/MAP

% p-map, boris, map, index, seiten, pages, aliases, hilfe, howto, uebersicht
## [MAP] SEITEN - tippe einen Alias, spring direkt rein
Jeder Alias oeffnet arsenal vorgefiltert auf EINE saubere Seite (Seed 'boris p-name'). Bare 'a' = voller Korpus / globale Cross-Suche. LHF = Low-Hanging-Fruits (geordnete Startkette).
```
echo 'lhf || relay adcs coerce rbcd vulns || recon esxi || creds lateral persist || setup map || a = ALLES'
```

% p-map, boris, map, suche, filter, howto, crossref
## [MAP] Wie die Suche tickt (UND-Filter / Ausschluss / Cross-Suche)
UND-Suche: jedes Wort muss matchen. In einer Seite weiter tippen schaerft, Backspace auf den Seiten-Key faellt in den Vollkorpus. Bare Tool-/Technik-Wort sucht QUER ueber alle Seiten (bewusst).
```
echo 'schaerfen: +wort | ausschliessen: !wort | quer: certipy-ad / dcsync / goldencert / kcache | mirror ausblenden: !mirror'
```

% p-map, boris, map, spalten, columns, kind, cat, lesen
## [MAP] Spalten lesen: KIND . BUCKET . Schritt
Titel-Spalte = KIND (LHF/PATH/MENU/SETUP/HELP). CAT-Spalte = Bucket (RELAY/ADCS/CREDS...). Erster Tag = Seiten-Key (der Alias).
```
echo 'KIND . BUCKET . [CODE] Beschreibung ... Kommando'
```

% p-map, boris, map, tool, toolsicht, vokabular, quer
## [MAP] TOOL-quer - tippe ein Tool-Wort fuer alle Vorkommen
```
echo 'certipy-ad | nxc | responder | ntlmrelayx | coercer | bloodhound | nmap | secretsdump | lsassy | donpapi | pypykatz | psexec | wmiexec | evilwinrm | getST | ticketer'
```

% p-map, boris, map, konvention, naming, regel, impacket, certipy
## [MAP] NAMENS-REGELN (verbindlich fuer ALLE Commands)
Impacket IMMER impacket-tool (NIE psexec.py/secretsdump.py). Certipy IMMER certipy-ad (NIE certipy). Standalone (PetitPotam.py/Coercer.py/dfscoerce.py) behalten .py.
```
echo 'impacket-<tool> (nie .py)  |  certipy-ad (nie certipy)'
```
